---
title: Conversione da EF6 a EF Core - conversione di un modello basato su codice
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 2dce1a50-7d84-4856-abf6-2763dd9be99d
uid: efcore-and-ef6/porting/port-code
ms.openlocfilehash: 2484b681d71ae8711b1b3a59bc274a0b2e403294
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 08/27/2018
ms.locfileid: "42997049"
---
# <a name="porting-an-ef6-code-based-model-to-ef-core"></a><span data-ttu-id="a33c0-102">Porting di un modello basato su codice EF6 a EF Core</span><span class="sxs-lookup"><span data-stu-id="a33c0-102">Porting an EF6 Code-Based Model to EF Core</span></span>

<span data-ttu-id="a33c0-103">Se sei arrivato a leggere tutti i problemi e si è pronti a porta, ecco alcune linee guida che consentono di iniziare.</span><span class="sxs-lookup"><span data-stu-id="a33c0-103">If you've read all the caveats and you are ready to port, then here are some guidelines to help you get started.</span></span>

## <a name="install-ef-core-nuget-packages"></a><span data-ttu-id="a33c0-104">Installare i pacchetti NuGet di Entity Framework Core</span><span class="sxs-lookup"><span data-stu-id="a33c0-104">Install EF Core NuGet packages</span></span>

<span data-ttu-id="a33c0-105">Per usare EF Core, si installa il pacchetto NuGet per il provider di database da usare.</span><span class="sxs-lookup"><span data-stu-id="a33c0-105">To use EF Core, you install the NuGet package for the database provider you want to use.</span></span> <span data-ttu-id="a33c0-106">Ad esempio, quando la destinazione SQL Server, è preferibile installare `Microsoft.EntityFrameworkCore.SqlServer`.</span><span class="sxs-lookup"><span data-stu-id="a33c0-106">For example, when targeting SQL Server, you would install `Microsoft.EntityFrameworkCore.SqlServer`.</span></span> <span data-ttu-id="a33c0-107">Visualizzare [provider di Database](../../core/providers/index.md) per informazioni dettagliate.</span><span class="sxs-lookup"><span data-stu-id="a33c0-107">See [Database Providers](../../core/providers/index.md) for details.</span></span>

<span data-ttu-id="a33c0-108">Se si prevede di usare le migrazioni, quindi è consigliabile installare anche il `Microsoft.EntityFrameworkCore.Tools` pacchetto.</span><span class="sxs-lookup"><span data-stu-id="a33c0-108">If you are planning to use migrations, then you should also install the `Microsoft.EntityFrameworkCore.Tools` package.</span></span>

<span data-ttu-id="a33c0-109">È possibile lasciare il pacchetto NuGet di Entity Framework 6 (Entity Framework) installato, come EF Core ed EF6 può essere utilizzata side-by-side nella stessa applicazione.</span><span class="sxs-lookup"><span data-stu-id="a33c0-109">It is fine to leave the EF6 NuGet package (EntityFramework) installed, as EF Core and EF6 can be used side-by-side in the same application.</span></span> <span data-ttu-id="a33c0-110">Tuttavia, se non si intende usare EF6 in tutte le aree dell'applicazione, quindi la disinstallazione del pacchetto documentazione offre errori di compilazione in parti di codice che richiedono attenzione.</span><span class="sxs-lookup"><span data-stu-id="a33c0-110">However, if you aren't intending to use EF6 in any areas of your application, then uninstalling the package will help give compile errors on pieces of code that need attention.</span></span>

## <a name="swap-namespaces"></a><span data-ttu-id="a33c0-111">Scambia gli spazi dei nomi</span><span class="sxs-lookup"><span data-stu-id="a33c0-111">Swap namespaces</span></span>

<span data-ttu-id="a33c0-112">La maggior parte delle API che usano in EF6 si trovano nel `System.Data.Entity` dello spazio dei nomi (e relativi spazi dei nomi secondari).</span><span class="sxs-lookup"><span data-stu-id="a33c0-112">Most APIs that you use in EF6 are in the `System.Data.Entity` namespace (and related sub-namespaces).</span></span> <span data-ttu-id="a33c0-113">La prima modifica di codice consiste nell'invertire al `Microsoft.EntityFrameworkCore` dello spazio dei nomi.</span><span class="sxs-lookup"><span data-stu-id="a33c0-113">The first code change is to swap to the `Microsoft.EntityFrameworkCore` namespace.</span></span> <span data-ttu-id="a33c0-114">In genere iniziare con il file di codice del contesto derivato e quindi scoprire da qui, risolvere gli errori di compilazione appena si verificano.</span><span class="sxs-lookup"><span data-stu-id="a33c0-114">You would typically start with your derived context code file and then work out from there, addressing compilation errors as they occur.</span></span>

## <a name="context-configuration-connection-etc"></a><span data-ttu-id="a33c0-115">Configurazione del contesto (connessione e così via).</span><span class="sxs-lookup"><span data-stu-id="a33c0-115">Context configuration (connection etc.)</span></span>

<span data-ttu-id="a33c0-116">Come descritto in [assicurarsi che EF Core verrà operazioni dell'applicazione](ensure-requirements.md), EF Core ha meno magic intorno a rilevare il database a cui connettersi.</span><span class="sxs-lookup"><span data-stu-id="a33c0-116">As described in [Ensure EF Core Will Work for Your Application](ensure-requirements.md), EF Core has less magic around detecting the database to connect to.</span></span> <span data-ttu-id="a33c0-117">È necessario eseguire l'override di `OnConfiguring` (metodo) nel contesto derivato e usare l'API specifiche del provider di database per configurare la connessione al database.</span><span class="sxs-lookup"><span data-stu-id="a33c0-117">You will need to override the `OnConfiguring` method on your derived context, and use the database provider specific API to setup the connection to the database.</span></span>

<span data-ttu-id="a33c0-118">La maggior parte delle applicazioni EF6 archiviano la stringa di connessione nelle applicazioni `App/Web.config` file.</span><span class="sxs-lookup"><span data-stu-id="a33c0-118">Most EF6 applications store the connection string in the applications `App/Web.config` file.</span></span> <span data-ttu-id="a33c0-119">In EF Core, leggere questa stringa di connessione usando il `ConfigurationManager` API.</span><span class="sxs-lookup"><span data-stu-id="a33c0-119">In EF Core, you read this connection string using the `ConfigurationManager` API.</span></span> <span data-ttu-id="a33c0-120">Potrebbe essere necessario aggiungere un riferimento al `System.Configuration` assembly del framework per poter usare questa API.</span><span class="sxs-lookup"><span data-stu-id="a33c0-120">You may need to add a reference to the `System.Configuration` framework assembly to be able to use this API.</span></span>

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

## <a name="update-your-code"></a><span data-ttu-id="a33c0-121">Aggiornare il codice</span><span class="sxs-lookup"><span data-stu-id="a33c0-121">Update your code</span></span>

<span data-ttu-id="a33c0-122">A questo punto, è una questione di indirizzamento errori di compilazione e revisione del codice per vedere se le modifiche del comportamento possibili conseguenze per te.</span><span class="sxs-lookup"><span data-stu-id="a33c0-122">At this point, it's a matter of addressing compilation errors and reviewing code to see if the behavior changes will impact you.</span></span>

## <a name="existing-migrations"></a><span data-ttu-id="a33c0-123">Migrazioni esistenti</span><span class="sxs-lookup"><span data-stu-id="a33c0-123">Existing migrations</span></span>

<span data-ttu-id="a33c0-124">Non c'è davvero una modalità fattibile per trasferire le migrazioni esistenti EF6 a EF Core.</span><span class="sxs-lookup"><span data-stu-id="a33c0-124">There isn't really a feasible way to port existing EF6 migrations to EF Core.</span></span>

<span data-ttu-id="a33c0-125">Se possibile, è consigliabile presupporre che tutti i precedenti migrazioni da EF6 sono state applicate al database e quindi avvia la migrazione dello schema da che punto di usando Entity Framework Core.</span><span class="sxs-lookup"><span data-stu-id="a33c0-125">If possible, it is best to assume that all previous migrations from EF6 have been applied to the database and then start migrating the schema from that point using EF Core.</span></span> <span data-ttu-id="a33c0-126">A tale scopo, si utilizzerebbe il `Add-Migration` comando per aggiungere una migrazione una volta il modello viene trasferito a EF Core.</span><span class="sxs-lookup"><span data-stu-id="a33c0-126">To do this, you would use the `Add-Migration` command to add a migration once the model is ported to EF Core.</span></span> <span data-ttu-id="a33c0-127">Quindi rimuovere tutto il codice dal `Up` e `Down` metodi della migrazione con scaffolding.</span><span class="sxs-lookup"><span data-stu-id="a33c0-127">You would then remove all code from the `Up` and `Down` methods of the scaffolded migration.</span></span> <span data-ttu-id="a33c0-128">Migrazioni successive confronterà al modello quando è stato eseguito lo scaffolding che la migrazione iniziale.</span><span class="sxs-lookup"><span data-stu-id="a33c0-128">Subsequent migrations will compare to the model when that initial migration was scaffolded.</span></span>

## <a name="test-the-port"></a><span data-ttu-id="a33c0-129">La porta di test</span><span class="sxs-lookup"><span data-stu-id="a33c0-129">Test the port</span></span>

<span data-ttu-id="a33c0-130">Il fatto che l'applicazione viene compilata, non significa che è al termine del trasferimento in EF Core.</span><span class="sxs-lookup"><span data-stu-id="a33c0-130">Just because your application compiles, does not mean it is successfully ported to EF Core.</span></span> <span data-ttu-id="a33c0-131">È necessario testare tutte le aree dell'applicazione per garantire che nessuna delle modifiche di comportamento hanno effetti negativi a causa dell'applicazione.</span><span class="sxs-lookup"><span data-stu-id="a33c0-131">You will need to test all areas of your application to ensure that none of the behavior changes have adversely impacted your application.</span></span>
