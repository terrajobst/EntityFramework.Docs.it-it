---
title: Operazioni personalizzate migrazioni - Entity Framework Core
author: bricelam
ms.author: bricelam
ms.date: 11/07/2017
ms.openlocfilehash: d715fe0408f25eb75c3160af79bb98fc87e41b17
ms.sourcegitcommit: 269c8a1a457a9ad27b4026c22c4b1a76991fb360
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/18/2018
ms.locfileid: "46284070"
---
<a name="custom-migrations-operations"></a><span data-ttu-id="99e97-102">Operazioni di migrazioni personalizzato</span><span class="sxs-lookup"><span data-stu-id="99e97-102">Custom Migrations Operations</span></span>
============================
<span data-ttu-id="99e97-103">L'API MigrationBuilder consente di eseguire molti tipi diversi di operazioni durante una migrazione, ma è lungi dall'essere completo.</span><span class="sxs-lookup"><span data-stu-id="99e97-103">The MigrationBuilder API allows you to perform many different kinds of operations during a migration, but it's far from exhaustive.</span></span> <span data-ttu-id="99e97-104">Tuttavia, l'API è anche estendibile che consente di definire le proprie operazioni.</span><span class="sxs-lookup"><span data-stu-id="99e97-104">However, the API is also extensible allowing you to define your own operations.</span></span> <span data-ttu-id="99e97-105">Esistono due modi per estendere l'API: usando il `Sql()` metodo, o definendo personalizzato `MigrationOperation` oggetti.</span><span class="sxs-lookup"><span data-stu-id="99e97-105">There are two ways to extend the API: Using the `Sql()` method, or by defining custom `MigrationOperation` objects.</span></span>

<span data-ttu-id="99e97-106">Per illustrare questo concetto, esaminiamo l'implementazione di un'operazione che crea un utente del database usando ogni approccio.</span><span class="sxs-lookup"><span data-stu-id="99e97-106">To illustrate, let's look at implementing an operation that creates a database user using each approach.</span></span> <span data-ttu-id="99e97-107">Nel nostro le migrazioni, è necessario abilitare la scrittura di codice seguente:</span><span class="sxs-lookup"><span data-stu-id="99e97-107">In our migrations, we want to enable writing the following code:</span></span>

``` csharp
migrationBuilder.CreateUser("SQLUser1", "Password");
```

<a name="using-migrationbuildersql"></a><span data-ttu-id="99e97-108">Usando MigrationBuilder.Sql()</span><span class="sxs-lookup"><span data-stu-id="99e97-108">Using MigrationBuilder.Sql()</span></span>
----------------------------
<span data-ttu-id="99e97-109">Il modo più semplice per implementare un'operazione personalizzata consiste nel definire un metodo di estensione che chiama `MigrationBuilder.Sql()`.</span><span class="sxs-lookup"><span data-stu-id="99e97-109">The easiest way to implement a custom operation is to define an extension method that calls `MigrationBuilder.Sql()`.</span></span>
<span data-ttu-id="99e97-110">Ecco un esempio che genera l'istruzione Transact-SQL appropriato.</span><span class="sxs-lookup"><span data-stu-id="99e97-110">Here is an example that generates the appropriate Transact-SQL.</span></span>

``` csharp
static MigrationBuilder CreateUser(
    this MigrationBuilder migrationBuilder,
    string name,
    string password)
    => migrationBuilder.Sql($"CREATE USER {name} WITH PASSWORD '{password}';");
```

<span data-ttu-id="99e97-111">Se le migrazioni devono supportare più provider di database, è possibile usare il `MigrationBuilder.ActiveProvider` proprietà.</span><span class="sxs-lookup"><span data-stu-id="99e97-111">If your migrations need to support multiple database providers, you can use the `MigrationBuilder.ActiveProvider` property.</span></span> <span data-ttu-id="99e97-112">Di seguito è riportato un esempio che supportano Microsoft SQL Server e PostgreSQL.</span><span class="sxs-lookup"><span data-stu-id="99e97-112">Here's an example supporting both Microsoft SQL Server and PostgreSQL.</span></span>

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

<span data-ttu-id="99e97-113">Questo approccio funziona solo se si conosce ogni provider in cui verrà applicato l'operazione personalizzata.</span><span class="sxs-lookup"><span data-stu-id="99e97-113">This approach only works if you know every provider where your custom operation will be applied.</span></span>

<a name="using-a-migrationoperation"></a><span data-ttu-id="99e97-114">Usando un MigrationOperation</span><span class="sxs-lookup"><span data-stu-id="99e97-114">Using a MigrationOperation</span></span>
---------------------------
<span data-ttu-id="99e97-115">Per separare l'operazione personalizzata da SQL, è possibile definire il proprio `MigrationOperation` per rappresentarlo.</span><span class="sxs-lookup"><span data-stu-id="99e97-115">To decouple the custom operation from the SQL, you can define your own `MigrationOperation` to represent it.</span></span> <span data-ttu-id="99e97-116">L'operazione viene quindi passato al provider e può pertanto determinare per generare SQL appropriato.</span><span class="sxs-lookup"><span data-stu-id="99e97-116">The operation is then passed to the provider so it can determine the appropriate SQL to generate.</span></span>

``` csharp
class CreateUserOperation : MigrationOperation
{
    public string Name { get; set; }
    public string Password { get; set; }
}
```

<span data-ttu-id="99e97-117">Con questo approccio, il metodo di estensione deve semplicemente aggiungere una di queste operazioni per `MigrationBuilder.Operations`.</span><span class="sxs-lookup"><span data-stu-id="99e97-117">With this approach, the extension method just needs to add one of these operations to `MigrationBuilder.Operations`.</span></span>

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

<span data-ttu-id="99e97-118">Questo approccio richiede che ogni provider sapere come generare SQL per questa operazione nella loro `IMigrationsSqlGenerator` servizio.</span><span class="sxs-lookup"><span data-stu-id="99e97-118">This approach requires each provider to know how to generate SQL for this operation in their `IMigrationsSqlGenerator` service.</span></span> <span data-ttu-id="99e97-119">Di seguito è riportato un esempio si esegue l'override del generatore di SQL Server per gestire la nuova operazione.</span><span class="sxs-lookup"><span data-stu-id="99e97-119">Here is an example overriding the SQL Server's generator to handle the new operation.</span></span>

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
        var stringMapping = Dependencies.TypeMappingSource.FindMapping(typeof(string));

        builder
            .Append("CREATE USER ")
            .Append(sqlHelper.DelimitIdentifier(operation.Name))
            .Append(" WITH PASSWORD = ")
            .Append(stringMapping.GenerateSqlLiteral(operation.Password))
            .AppendLine(sqlHelper.StatementTerminator)
            .EndCommand();
    }
}
```

<span data-ttu-id="99e97-120">Sostituire il servizio di generatore sql migrazioni predefinito con quello aggiornato.</span><span class="sxs-lookup"><span data-stu-id="99e97-120">Replace the default migrations sql generator service with the updated one.</span></span>

``` csharp
protected override void OnConfiguring(DbContextOptionsBuilder options)
    => options
        .UseSqlServer(connectionString)
        .ReplaceService<IMigrationsSqlGenerator, MyMigrationsSqlGenerator>();
```
