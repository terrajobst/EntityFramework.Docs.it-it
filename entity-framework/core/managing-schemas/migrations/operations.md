---
title: Operazioni personalizzate migrazioni - Entity Framework Core
author: bricelam
ms.author: bricelam
ms.date: 11/07/2017
ms.openlocfilehash: dcf11c44dcc9f6008b8290a89dd8c042e5ec5771
ms.sourcegitcommit: 2b787009fd5be5627f1189ee396e708cd130e07b
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/13/2018
ms.locfileid: "45489141"
---
<a name="custom-migrations-operations"></a>Operazioni di migrazioni personalizzato
============================
L'API MigrationBuilder consente di eseguire molti tipi diversi di operazioni durante una migrazione, ma è lungi dall'essere completo. Tuttavia, l'API è anche estendibile che consente di definire le proprie operazioni. Esistono due modi per estendere l'API: usando il `Sql()` metodo, o definendo personalizzato `MigrationOperation` oggetti.

Per illustrare questo concetto, esaminiamo l'implementazione di un'operazione che crea un utente del database usando ogni approccio. Nel nostro le migrazioni, è necessario abilitare la scrittura di codice seguente:

``` csharp
migrationBuilder.CreateUser("SQLUser1", "Password");
```

<a name="using-migrationbuildersql"></a>Usando MigrationBuilder.Sql()
----------------------------
Il modo più semplice per implementare un'operazione personalizzata consiste nel definire un metodo di estensione che chiama `MigrationBuilder.Sql()`.
Ecco un esempio che genera l'istruzione Transact-SQL appropriato.

``` csharp
static MigrationBuilder CreateUser(
    this MigrationBuilder migrationBuilder,
    string name,
    string password)
    => migrationBuilder.Sql($"CREATE USER {name} WITH PASSWORD '{password}';");
```

Se le migrazioni devono supportare più provider di database, è possibile usare il `MigrationBuilder.ActiveProvider` proprietà. Di seguito è riportato un esempio che supportano Microsoft SQL Server e PostgreSQL.

``` csharp
static MigrationBuilder CreateUser(
    this MigrationBuilder migrationBuilder,
    string name,
    string password)
{
    switch (migrationBuilder.ActiveProvider)
    {
        case "Npgsql.EntityFrameworkCore.PostgreSQL":
            return migrationBuilder
                .Sql($"CREATE USER {name} WITH PASSWORD '{password}';");

        case "Microsoft.EntityFrameworkCore.SqlServer":
            return migrationBuilder
                .Sql($"CREATE USER {name} WITH PASSWORD = '{password}';");
    }

    return migrationBuilder;
}
```

Questo approccio funziona solo se si conosce ogni provider in cui verrà applicato l'operazione personalizzata.

<a name="using-a-migrationoperation"></a>Usando un MigrationOperation
---------------------------
Per separare l'operazione personalizzata da SQL, è possibile definire il proprio `MigrationOperation` per rappresentarlo. L'operazione viene quindi passato al provider e può pertanto determinare per generare SQL appropriato.

``` csharp
class CreateUserOperation : MigrationOperation
{
    public string Name { get; set; }
    public string Password { get; set; }
}
```

Con questo approccio, il metodo di estensione deve semplicemente aggiungere una di queste operazioni per `MigrationBuilder.Operations`.

``` csharp
static MigrationBuilder CreateUser(
    this MigrationBuilder migrationBuilder,
    string name,
    string password)
{
    migrationBuilder.Operations.Add(
        new CreateUserOperation
        {
            Name = name,
            Password = password
        });

    return migrationBuilder;
}
```

Questo approccio richiede che ogni provider sapere come generare SQL per questa operazione nella loro `IMigrationsSqlGenerator` servizio. Di seguito è riportato un esempio si esegue l'override del generatore di SQL Server per gestire la nuova operazione.

``` csharp
class MyMigrationsSqlGenerator : SqlServerMigrationsSqlGenerator
{
    public MyMigrationsSqlGenerator(
        MigrationsSqlGeneratorDependencies dependencies,
        IMigrationsAnnotationProvider migrationsAnnotations)
        : base(dependencies, migrationsAnnotations)
    {
    }

    protected override void Generate(
        MigrationOperation operation,
        IModel model,
        MigrationCommandListBuilder builder)
    {
        if (operation is CreateUserOperation createUserOperation)
        {
            Generate(createUserOperation, builder);
        }
        else
        {
            base.Generate(operation, model, builder);
        }
    }

    private void Generate(
        CreateUserOperation operation,
        MigrationCommandListBuilder builder)
    {
        var sqlHelper = Dependencies.SqlGenerationHelper;
        var stringMapping = Dependencies.TypeMapper.GetMapping(typeof(string));

        builder
            .Append("CREATE USER ")
            .Append(sqlHelper.DelimitIdentifier(name))
            .Append(" WITH PASSWORD = ")
            .Append(stringMapping.GenerateSqlLiteral(password))
            .AppendLine(sqlHelper.StatementTerminator)
            .EndCommand();
    }
}
```

Sostituire il servizio di generatore sql migrazioni predefinito con quello aggiornato.

``` csharp
protected override void OnConfiguring(DbContextOptionsBuilder options)
    => options
        .UseSqlServer(connectionString)
        .ReplaceService<IMigrationsSqlGenerator, MyMigrationsSqlGenerator>();
```
