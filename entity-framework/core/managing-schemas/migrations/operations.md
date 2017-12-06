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
<a name="custom-migrations-operations"></a><span data-ttu-id="3ab78-102">Operazioni di migrazione personalizzata</span><span class="sxs-lookup"><span data-stu-id="3ab78-102">Custom Migrations Operations</span></span>
============================
<span data-ttu-id="3ab78-103">L'API MigrationBuilder consente di eseguire molti tipi diversi di operazioni durante una migrazione, ma è lontano dal completo.</span><span class="sxs-lookup"><span data-stu-id="3ab78-103">The MigrationBuilder API allows you to perform many different kinds of operations during a migration, but it's far from exhaustive.</span></span> <span data-ttu-id="3ab78-104">Tuttavia, l'API può anche essere estesa che consente di definire operazioni personalizzate.</span><span class="sxs-lookup"><span data-stu-id="3ab78-104">However, the API is also extensible allowing you to define your own operations.</span></span> <span data-ttu-id="3ab78-105">Esistono due modi per estendere l'API: utilizzando il `Sql()` metodo, o definendo personalizzato `MigrationOperation` oggetti.</span><span class="sxs-lookup"><span data-stu-id="3ab78-105">There are two ways to extend the API: Using the `Sql()` method, or by defining custom `MigrationOperation` objects.</span></span>

<span data-ttu-id="3ab78-106">Per illustrare, esaminiamo l'implementazione di un'operazione che crea un utente del database utilizzando ciascun approccio.</span><span class="sxs-lookup"><span data-stu-id="3ab78-106">To illustrate, let's look at implementing an operation that creates a database user using each approach.</span></span> <span data-ttu-id="3ab78-107">Il nostro delle migrazioni, si vuole abilitare la scrittura di codice riportato di seguito:</span><span class="sxs-lookup"><span data-stu-id="3ab78-107">In our migrations, we want to enable writing the following code:</span></span>

``` csharp
migrationBuilder.CreateUser("SQLUser1", "Password");
```

<a name="using-migrationbuildersql"></a><span data-ttu-id="3ab78-108">Utilizzando MigrationBuilder.Sql()</span><span class="sxs-lookup"><span data-stu-id="3ab78-108">Using MigrationBuilder.Sql()</span></span>
----------------------------
<span data-ttu-id="3ab78-109">Il modo più semplice per implementare un'operazione personalizzata consiste nel definire un metodo di estensione che chiama `MigrationBuilder.Sql()`.</span><span class="sxs-lookup"><span data-stu-id="3ab78-109">The easiest way to implement a custom operation is to define an extension method that calls `MigrationBuilder.Sql()`.</span></span>
<span data-ttu-id="3ab78-110">Di seguito è riportato un esempio che genera l'istruzione Transact-SQL appropriato.</span><span class="sxs-lookup"><span data-stu-id="3ab78-110">Here is an example that generates the appropriate Transact-SQL.</span></span>

``` csharp
static MigrationBuilder CreateUser(
    this MigrationBuilder migrationBuilder,
    string name,
    string password)
    => migrationBuilder.Sql($"CREATE USER {name} WITH PASSWORD '{password}';");
```

<span data-ttu-id="3ab78-111">Se le migrazioni necessario supportare più provider di database, è possibile utilizzare il `MigrationBuilder.ActiveProvider` proprietà.</span><span class="sxs-lookup"><span data-stu-id="3ab78-111">If your migrations need to support multiple database providers, you can use the `MigrationBuilder.ActiveProvider` property.</span></span> <span data-ttu-id="3ab78-112">Di seguito è riportato un esempio che supporta Microsoft SQL Server e PostreSQL.</span><span class="sxs-lookup"><span data-stu-id="3ab78-112">Here's an example supporting both Microsoft SQL Server and PostreSQL.</span></span>

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

<span data-ttu-id="3ab78-113">Questo approccio funziona solo se si conoscono tutti i provider in cui verrà applicato l'operazione personalizzata.</span><span class="sxs-lookup"><span data-stu-id="3ab78-113">This approach only works if you know every provider where your custom operation will be applied.</span></span>

<a name="using-a-migrationoperation"></a><span data-ttu-id="3ab78-114">Utilizzando un MigrationOperation</span><span class="sxs-lookup"><span data-stu-id="3ab78-114">Using a MigrationOperation</span></span>
---------------------------
<span data-ttu-id="3ab78-115">Per separare l'operazione personalizzata da SQL, è possibile definirne di propri `MigrationOperation` per rappresentarlo.</span><span class="sxs-lookup"><span data-stu-id="3ab78-115">To decouple the custom operation from the SQL, you can define your own `MigrationOperation` to represent it.</span></span> <span data-ttu-id="3ab78-116">L'operazione viene quindi passato al provider, pertanto può determinare per generare SQL appropriato.</span><span class="sxs-lookup"><span data-stu-id="3ab78-116">The operation is then passed to the provider so it can determine the appropriate SQL to generate.</span></span>

``` csharp
class CreateUserOperation : MigrationOperation
{
    public string Name { get; set; }
    public string Password { get; set; }
}
```

<span data-ttu-id="3ab78-117">Con questo approccio, il metodo di estensione deve semplicemente una di queste operazioni per aggiungere `MigrationBuilder.Operations`.</span><span class="sxs-lookup"><span data-stu-id="3ab78-117">With this approach, the extension method just needs to add one of these operations to `MigrationBuilder.Operations`.</span></span>

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

<span data-ttu-id="3ab78-118">Questo approccio richiede che ogni provider imparare a generare SQL per questa operazione nella loro `IMigrationsSqlGenerator` servizio.</span><span class="sxs-lookup"><span data-stu-id="3ab78-118">This approach requires each provider to know how to generate SQL for this operation in their `IMigrationsSqlGenerator` service.</span></span> <span data-ttu-id="3ab78-119">Di seguito è riportato un esempio si esegue l'override del generatore di SQL Server per gestire la nuova operazione.</span><span class="sxs-lookup"><span data-stu-id="3ab78-119">Here is an example overriding the SQL Server's generator to handle the new operation.</span></span>

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

<span data-ttu-id="3ab78-120">Sostituire il servizio di generatore sql migrazioni predefinito con quello aggiornato.</span><span class="sxs-lookup"><span data-stu-id="3ab78-120">Replace the default migrations sql generator service with the updated one.</span></span>

``` csharp
protected override void OnConfiguring(DbContextOptionsBuilder options)
    => options
        .UseSqlServer(connectionString)
        .ReplaceService<IMigrationsSqlGenerator, MyMigrationsSqlGenerator>();
```
