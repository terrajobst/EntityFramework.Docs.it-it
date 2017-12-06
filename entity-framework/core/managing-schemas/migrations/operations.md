---
title: Operazioni di migrazioni personalizzato - Core EF
author: bricelam
ms.author: bricelam
ms.date: 11/7/2017
ms.technology: entity-framework-core
ms.openlocfilehash: d41409dee034e84d22092a5f9111dd79c87dcec3
ms.sourcegitcommit: b467368cc350e6059fdc0949e042a41cb11e61d9
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/15/2017
---
<a name="custom-migrations-operations"></a>Operazioni di migrazione personalizzata
============================
L'API MigrationBuilder consente di eseguire molti tipi diversi di operazioni durante una migrazione, ma è lontano dal completo. Tuttavia, l'API può anche essere estesa che consente di definire operazioni personalizzate. Esistono due modi per estendere l'API: utilizzando il `Sql()` metodo, o definendo personalizzato `MigrationOperation` oggetti.

Per illustrare, esaminiamo l'implementazione di un'operazione che crea un utente del database utilizzando ciascun approccio. Il nostro delle migrazioni, si vuole abilitare la scrittura di codice riportato di seguito:

``` csharp
migrationBuilder.CreateUser("SQLUser1", "Password");
```

<a name="using-migrationbuildersql"></a>Utilizzando MigrationBuilder.Sql()
----------------------------
Il modo più semplice per implementare un'operazione personalizzata consiste nel definire un metodo di estensione che chiama `MigrationBuilder.Sql()`.
Di seguito è riportato un esempio che genera l'istruzione Transact-SQL appropriato.

``` csharp
static MigrationBuilder CreateUser(
    this MigrationBuilder migrationBuilder,
    string name,
    string password)
    => migrationBuilder.Sql($"CREATE USER {name} WITH PASSWORD '{password}';");
```

Se le migrazioni necessario supportare più provider di database, è possibile utilizzare il `MigrationBuilder.ActiveProvider` proprietà. Di seguito è riportato un esempio che supporta Microsoft SQL Server e PostreSQL.

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
}
```

Questo approccio funziona solo se si conoscono tutti i provider in cui verrà applicato l'operazione personalizzata.

<a name="using-a-migrationoperation"></a>Utilizzando un MigrationOperation
---------------------------
Per separare l'operazione personalizzata da SQL, è possibile definirne di propri `MigrationOperation` per rappresentarlo. L'operazione viene quindi passato al provider, pertanto può determinare per generare SQL appropriato.

``` csharp
class CreateUserOperation : MigrationOperation
{
    public string Name { get; set; }
    public string Password { get; set; }
}
```

Con questo approccio, il metodo di estensione deve semplicemente una di queste operazioni per aggiungere `MigrationBuilder.Operations`.

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

Questo approccio richiede che ogni provider imparare a generare SQL per questa operazione nella loro `IMigrationsSqlGenerator` servizio. Di seguito è riportato un esempio si esegue l'override del generatore di SQL Server per gestire la nuova operazione.

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
