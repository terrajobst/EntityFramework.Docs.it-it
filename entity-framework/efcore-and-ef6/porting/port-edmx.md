---
title: Porting da EF6 a EF Core - il Porting di un modello basato su EDMX
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: 63003709-f1ec-4bdc-8083-65a60c4826d2
uid: efcore-and-ef6/porting/port-edmx
ms.openlocfilehash: c999d2114c76ee3a7615ae897b42ee3376cff14e
ms.sourcegitcommit: 1880d5ce49d4c9cb891d0e8fb230770bb44799e5
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/23/2017
---
# <a name="porting-an-ef6-edmx-based-model-to-ef-core"></a><span data-ttu-id="ed8fd-102">Porting di un modello basato su EDMX EF6 a EF Core</span><span class="sxs-lookup"><span data-stu-id="ed8fd-102">Porting an EF6 EDMX-Based Model to EF Core</span></span>

<span data-ttu-id="ed8fd-103">Core EF non supporta il formato del file EDMX per i modelli.</span><span class="sxs-lookup"><span data-stu-id="ed8fd-103">EF Core does not support the EDMX file format for models.</span></span> <span data-ttu-id="ed8fd-104">L'opzione migliore per questi modelli, la porta consiste nel generare un nuovo modello basato su codice dal database dell'applicazione.</span><span class="sxs-lookup"><span data-stu-id="ed8fd-104">The best option to port these models, is to generate a new code-based model from the database for your application.</span></span>

## <a name="install-ef-core-nuget-packages"></a><span data-ttu-id="ed8fd-105">Installare i pacchetti NuGet Core EF</span><span class="sxs-lookup"><span data-stu-id="ed8fd-105">Install EF Core NuGet packages</span></span>

<span data-ttu-id="ed8fd-106">Installare il `Microsoft.EntityFrameworkCore.Tools` pacchetto NuGet.</span><span class="sxs-lookup"><span data-stu-id="ed8fd-106">Install the `Microsoft.EntityFrameworkCore.Tools` NuGet package.</span></span>

## <a name="regenerate-the-model"></a><span data-ttu-id="ed8fd-107">Rigenerare il modello</span><span class="sxs-lookup"><span data-stu-id="ed8fd-107">Regenerate the model</span></span>

<span data-ttu-id="ed8fd-108">È ora possibile utilizzare la funzionalità di decodifica per creare un modello basato sul database esistente.</span><span class="sxs-lookup"><span data-stu-id="ed8fd-108">You can now use the reverse engineer functionality to create a model based on your existing database.</span></span>

<span data-ttu-id="ed8fd-109">Eseguire il comando seguente nella Console di gestione pacchetti (Strumenti -> Gestione pacchetti NuGet -> Console di gestione pacchetti).</span><span class="sxs-lookup"><span data-stu-id="ed8fd-109">Run the following command in Package Manager Console (Tools –> NuGet Package Manager –> Package Manager Console).</span></span> <span data-ttu-id="ed8fd-110">Vedere [Package Manager Console (Visual Studio)](../../core/miscellaneous/cli/powershell.md) per opzioni di comando per eseguire lo scaffolding di un subset di tabelle e così via.</span><span class="sxs-lookup"><span data-stu-id="ed8fd-110">See [Package Manager Console (Visual Studio)](../../core/miscellaneous/cli/powershell.md) for command options to scaffold a subset of tables etc.</span></span>

``` powershell
Scaffold-DbContext "<connection string>" <database provider name>
```

<span data-ttu-id="ed8fd-111">Ad esempio, ecco il comando per eseguire lo scaffolding di un modello dal database di blog sull'istanza del database locale di SQL Server.</span><span class="sxs-lookup"><span data-stu-id="ed8fd-111">For example, here is the command to scaffold a model from the Blogging database on your SQL Server LocalDB instance.</span></span>

``` powershell
Scaffold-DbContext "Server=(localdb)\mssqllocaldb;Database=Blogging;Trusted_Connection=True;" Microsoft.EntityFrameworkCore.SqlServer
```

## <a name="remove-ef6-model"></a><span data-ttu-id="ed8fd-112">Rimuovere il modello EF6</span><span class="sxs-lookup"><span data-stu-id="ed8fd-112">Remove EF6 model</span></span>

<span data-ttu-id="ed8fd-113">Rimuovere questo punto il modello EF6 dall'applicazione.</span><span class="sxs-lookup"><span data-stu-id="ed8fd-113">You would now remove the EF6 model from your application.</span></span>

<span data-ttu-id="ed8fd-114">È possibile lasciare il pacchetto NuGet EF6 (EntityFramework) installato, come EF Core ed EF6 possono essere utilizzati side-by-side nella stessa applicazione.</span><span class="sxs-lookup"><span data-stu-id="ed8fd-114">It is fine to leave the EF6 NuGet package (EntityFramework) installed, as EF Core and EF6 can be used side-by-side in the same application.</span></span> <span data-ttu-id="ed8fd-115">Tuttavia, se non si intende utilizzare EF6 nelle aree dell'applicazione, quindi disinstallare il pacchetto consente di assegnare gli errori di compilazione in parti di codice che richiedono attenzione.</span><span class="sxs-lookup"><span data-stu-id="ed8fd-115">However, if you aren't intending to use EF6 in any areas of your application, then uninstalling the package will help give compile errors on pieces of code that need attention.</span></span>

## <a name="update-your-code"></a><span data-ttu-id="ed8fd-116">Aggiornare il codice</span><span class="sxs-lookup"><span data-stu-id="ed8fd-116">Update your code</span></span>

<span data-ttu-id="ed8fd-117">A questo punto, si tratta di errori di compilazione di indirizzamento e revisione codice per vedere se le modifiche di comportamento tra EF6 ed EF Core è necessario tenere conto.</span><span class="sxs-lookup"><span data-stu-id="ed8fd-117">At this point, it's a matter of addressing compilation errors and reviewing code to see if the behavior changes between EF6 and EF Core will impact you.</span></span>

## <a name="test-the-port"></a><span data-ttu-id="ed8fd-118">La porta di test</span><span class="sxs-lookup"><span data-stu-id="ed8fd-118">Test the port</span></span>

<span data-ttu-id="ed8fd-119">Il fatto che l'applicazione viene compilata, non significa che correttamente eseguito il porting a EF Core.</span><span class="sxs-lookup"><span data-stu-id="ed8fd-119">Just because your application compiles, does not mean it is successfully ported to EF Core.</span></span> <span data-ttu-id="ed8fd-120">È necessario testare tutte le aree dell'applicazione per assicurarsi che nessuna delle modifiche del comportamento hanno effetti negativi sull'applicazione.</span><span class="sxs-lookup"><span data-stu-id="ed8fd-120">You will need to test all areas of your application to ensure that none of the behavior changes have adversely impacted your application.</span></span>

> [!TIP]
> <span data-ttu-id="ed8fd-121">Vedere [Introduzione a Entity Framework Core in ASP.NET Core con un Database esistente](xref:core/get-started/aspnetcore/existing-db) per un riferimento aggiuntivo sulla modalità di utilizzo con un database esistente,</span><span class="sxs-lookup"><span data-stu-id="ed8fd-121">See [Getting Started with EF Core on ASP.NET Core with an Existing Database](xref:core/get-started/aspnetcore/existing-db) for an additional reference on how to work with an existing database,</span></span> 
