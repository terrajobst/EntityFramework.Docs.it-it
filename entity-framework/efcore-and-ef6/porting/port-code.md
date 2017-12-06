---
title: Porting da EF6 a EF Core - il Porting di un modello basato su codice
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: 2dce1a50-7d84-4856-abf6-2763dd9be99d
uid: efcore-and-ef6/porting/port-code
ms.openlocfilehash: a0fa4f9a7028f56666fb993185cb03eddb9a2cd1
ms.sourcegitcommit: 01a75cd483c1943ddd6f82af971f07abde20912e
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 10/27/2017
---
# <a name="porting-an-ef6-code-based-model-to-ef-core"></a><span data-ttu-id="4c97f-102">Porting di un modello basato su codice EF6 a EF Core</span><span class="sxs-lookup"><span data-stu-id="4c97f-102">Porting an EF6 Code-Based Model to EF Core</span></span>

<span data-ttu-id="4c97f-103">Se sei arrivato a leggere tutti i problemi e si è pronti a porta, di seguito sono riportate alcune linee guida che consentono di iniziare.</span><span class="sxs-lookup"><span data-stu-id="4c97f-103">If you've read all the caveats and you are ready to port, then here are some guidelines to help you get started.</span></span>

## <a name="install-ef-core-nuget-packages"></a><span data-ttu-id="4c97f-104">Installare i pacchetti NuGet Core EF</span><span class="sxs-lookup"><span data-stu-id="4c97f-104">Install EF Core NuGet packages</span></span>

<span data-ttu-id="4c97f-105">Per l'utilizzo di Entity Framework Core, si installa il pacchetto NuGet per il provider di database che si desidera utilizzare.</span><span class="sxs-lookup"><span data-stu-id="4c97f-105">To use EF Core, you install the NuGet package for the database provider you want to use.</span></span> <span data-ttu-id="4c97f-106">Ad esempio, se la destinazione SQL Server, è preferibile installare `Microsoft.EntityFrameworkCore.SqlServer`.</span><span class="sxs-lookup"><span data-stu-id="4c97f-106">For example, when targeting SQL Server, you would install `Microsoft.EntityFrameworkCore.SqlServer`.</span></span> <span data-ttu-id="4c97f-107">Vedere [i provider di Database](../../core/providers/index.md) per informazioni dettagliate.</span><span class="sxs-lookup"><span data-stu-id="4c97f-107">See [Database Providers](../../core/providers/index.md) for details.</span></span>

<span data-ttu-id="4c97f-108">Se si intende utilizzare migrazioni, quindi è necessario installare anche il `Microsoft.EntityFrameworkCore.Tools` pacchetto.</span><span class="sxs-lookup"><span data-stu-id="4c97f-108">If you are planning to use migrations, then you should also install the `Microsoft.EntityFrameworkCore.Tools` package.</span></span>

<span data-ttu-id="4c97f-109">È possibile lasciare il pacchetto NuGet EF6 (EntityFramework) installato, come EF Core ed EF6 possono essere utilizzati side-by-side nella stessa applicazione.</span><span class="sxs-lookup"><span data-stu-id="4c97f-109">It is fine to leave the EF6 NuGet package (EntityFramework) installed, as EF Core and EF6 can be used side-by-side in the same application.</span></span> <span data-ttu-id="4c97f-110">Tuttavia, se non si intende utilizzare EF6 nelle aree dell'applicazione, quindi disinstallare il pacchetto consente di assegnare gli errori di compilazione in parti di codice che richiedono attenzione.</span><span class="sxs-lookup"><span data-stu-id="4c97f-110">However, if you aren't intending to use EF6 in any areas of your application, then uninstalling the package will help give compile errors on pieces of code that need attention.</span></span>

## <a name="swap-namespaces"></a><span data-ttu-id="4c97f-111">Scambia gli spazi dei nomi</span><span class="sxs-lookup"><span data-stu-id="4c97f-111">Swap namespaces</span></span>

<span data-ttu-id="4c97f-112">La maggior parte delle API che si utilizzano in EF6 presenti il `System.Data.Entity` dello spazio dei nomi (e correlate sottospazi dei nomi).</span><span class="sxs-lookup"><span data-stu-id="4c97f-112">Most APIs that you use in EF6 are in the `System.Data.Entity` namespace (and related sub-namespaces).</span></span> <span data-ttu-id="4c97f-113">È la prima modifica di codice da scambiare per il `Microsoft.EntityFrameworkCore` dello spazio dei nomi.</span><span class="sxs-lookup"><span data-stu-id="4c97f-113">The first code change is to swap to the `Microsoft.EntityFrameworkCore` namespace.</span></span> <span data-ttu-id="4c97f-114">In genere iniziare con il file di codice di contesto derivata e quindi tenta di risolvere da qui, risolvere gli errori di compilazione che si verificano.</span><span class="sxs-lookup"><span data-stu-id="4c97f-114">You would typically start with your derived context code file and then work out from there, addressing compilation errors as they occur.</span></span>

## <a name="context-configuration-connection-etc"></a><span data-ttu-id="4c97f-115">Configurazione del contesto (connessione e così via).</span><span class="sxs-lookup"><span data-stu-id="4c97f-115">Context configuration (connection etc.)</span></span>

<span data-ttu-id="4c97f-116">Come descritto in [verificare EF verrà operazione principale per l'applicazione](ensure-requirements.md), Core EF ha meno chiave per la connessione al database di rilevamento.</span><span class="sxs-lookup"><span data-stu-id="4c97f-116">As described in [Ensure EF Core Will Work for Your Application](ensure-requirements.md), EF Core has less magic around detecting the database to connect to.</span></span> <span data-ttu-id="4c97f-117">È necessario eseguire l'override di `OnConfiguring` metodo sul contesto derivata e utilizzare l'API specifiche del provider di database per la connessione al database del programma di installazione.</span><span class="sxs-lookup"><span data-stu-id="4c97f-117">You will need to override the `OnConfiguring` method on your derived context, and use the database provider specific API to setup the connection to the database.</span></span>

<span data-ttu-id="4c97f-118">La maggior parte delle applicazioni EF6 archiviano la stringa di connessione nelle applicazioni `App/Web.config` file.</span><span class="sxs-lookup"><span data-stu-id="4c97f-118">Most EF6 applications store the connection string in the applications `App/Web.config` file.</span></span> <span data-ttu-id="4c97f-119">In Entity Framework Core, leggere la stringa di connessione utilizzando il `ConfigurationManager` API.</span><span class="sxs-lookup"><span data-stu-id="4c97f-119">In EF Core, you read this connection string using the `ConfigurationManager` API.</span></span> <span data-ttu-id="4c97f-120">Potrebbe essere necessario aggiungere un riferimento di `System.Configuration` assembly framework per poter usare questa API.</span><span class="sxs-lookup"><span data-stu-id="4c97f-120">You may need to add a reference to the `System.Configuration` framework assembly to be able to use this API.</span></span>

``` csharp
public class BloggingContext : DbContext
{
    public DbSet<Blog> Blogs { get; set; }
    public DbSet<Post> Posts { get; set; }

    protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
    {
      optionsBuilder.UseSqlServer(ConfigurationManager.ConnectionStrings["BloggingDatabase"].ConnectionString);
    }
}
```

## <a name="update-your-code"></a><span data-ttu-id="4c97f-121">Aggiornare il codice</span><span class="sxs-lookup"><span data-stu-id="4c97f-121">Update your code</span></span>

<span data-ttu-id="4c97f-122">A questo punto, si tratta di errori di compilazione di indirizzamento e revisione del codice per vedere se le modifiche del comportamento è necessario tenere conto.</span><span class="sxs-lookup"><span data-stu-id="4c97f-122">At this point, it's a matter of addressing compilation errors and reviewing code to see if the behavior changes will impact you.</span></span>

## <a name="existing-migrations"></a><span data-ttu-id="4c97f-123">Migrazioni esistente</span><span class="sxs-lookup"><span data-stu-id="4c97f-123">Existing migrations</span></span>

<span data-ttu-id="4c97f-124">Non è effettivamente di porta esistente migrazioni EF6 a EF Core in modo appropriato.</span><span class="sxs-lookup"><span data-stu-id="4c97f-124">There isn't really a feasible way to port existing EF6 migrations to EF Core.</span></span>

<span data-ttu-id="4c97f-125">Se possibile, è consigliabile presupporre che tutte le migrazioni precedenti da EF6 sono state applicate al database e quindi avviare la migrazione dello schema da che punto usando EF Core.</span><span class="sxs-lookup"><span data-stu-id="4c97f-125">If possible, it is best to assume that all previous migrations from EF6 have been applied to the database and then start migrating the schema from that point using EF Core.</span></span> <span data-ttu-id="4c97f-126">A tale scopo, utilizzare il `Add-Migration` comando per aggiungere una migrazione dopo che il modello viene trasferito a EF Core.</span><span class="sxs-lookup"><span data-stu-id="4c97f-126">To do this, you would use the `Add-Migration` command to add a migration once the model is ported to EF Core.</span></span> <span data-ttu-id="4c97f-127">Quindi rimuovere tutto il codice dal `Up` e `Down` metodi della migrazione di scaffolding.</span><span class="sxs-lookup"><span data-stu-id="4c97f-127">You would then remove all code from the `Up` and `Down` methods of the scaffolded migration.</span></span> <span data-ttu-id="4c97f-128">Migrazioni successive verranno confrontate al modello quando è stata scaffolding che la migrazione iniziale.</span><span class="sxs-lookup"><span data-stu-id="4c97f-128">Subsequent migrations will compare to the model when that initial migration was scaffolded.</span></span>

## <a name="test-the-port"></a><span data-ttu-id="4c97f-129">La porta di test</span><span class="sxs-lookup"><span data-stu-id="4c97f-129">Test the port</span></span>

<span data-ttu-id="4c97f-130">Il fatto che l'applicazione viene compilata, non significa che correttamente eseguito il porting a EF Core.</span><span class="sxs-lookup"><span data-stu-id="4c97f-130">Just because your application compiles, does not mean it is successfully ported to EF Core.</span></span> <span data-ttu-id="4c97f-131">È necessario testare tutte le aree dell'applicazione per assicurarsi che nessuna delle modifiche del comportamento hanno effetti negativi sull'applicazione.</span><span class="sxs-lookup"><span data-stu-id="4c97f-131">You will need to test all areas of your application to ensure that none of the behavior changes have adversely impacted your application.</span></span>
