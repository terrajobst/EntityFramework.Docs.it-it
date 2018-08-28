---
title: Conversione da EF6 a EF Core - conversione di un modello basato su EDMX
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 63003709-f1ec-4bdc-8083-65a60c4826d2
uid: efcore-and-ef6/porting/port-edmx
ms.openlocfilehash: 2c3336ac675a830566001a0ddb3777839f52db18
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 08/27/2018
ms.locfileid: "42997411"
---
# <a name="porting-an-ef6-edmx-based-model-to-ef-core"></a><span data-ttu-id="8ba3d-102">Conversione di un modello basato su EDMX EF6 a EF Core</span><span class="sxs-lookup"><span data-stu-id="8ba3d-102">Porting an EF6 EDMX-Based Model to EF Core</span></span>

<span data-ttu-id="8ba3d-103">EF Core non supporta il formato di file EDMX per modelli.</span><span class="sxs-lookup"><span data-stu-id="8ba3d-103">EF Core does not support the EDMX file format for models.</span></span> <span data-ttu-id="8ba3d-104">L'opzione migliore per questi modelli, la porta è generare un nuovo modello basato su codice dal database per l'applicazione.</span><span class="sxs-lookup"><span data-stu-id="8ba3d-104">The best option to port these models, is to generate a new code-based model from the database for your application.</span></span>

## <a name="install-ef-core-nuget-packages"></a><span data-ttu-id="8ba3d-105">Installare i pacchetti NuGet di Entity Framework Core</span><span class="sxs-lookup"><span data-stu-id="8ba3d-105">Install EF Core NuGet packages</span></span>

<span data-ttu-id="8ba3d-106">Installare il `Microsoft.EntityFrameworkCore.Tools` pacchetto NuGet.</span><span class="sxs-lookup"><span data-stu-id="8ba3d-106">Install the `Microsoft.EntityFrameworkCore.Tools` NuGet package.</span></span>

## <a name="regenerate-the-model"></a><span data-ttu-id="8ba3d-107">Rigenerare il modello</span><span class="sxs-lookup"><span data-stu-id="8ba3d-107">Regenerate the model</span></span>

<span data-ttu-id="8ba3d-108">È ora possibile usare la funzionalità di reverse engineering per creare un modello basato sul database esistente.</span><span class="sxs-lookup"><span data-stu-id="8ba3d-108">You can now use the reverse engineer functionality to create a model based on your existing database.</span></span>

<span data-ttu-id="8ba3d-109">Eseguire il comando seguente nella Console di gestione pacchetti (Strumenti -> Gestione pacchetti NuGet -> Console di gestione pacchetti).</span><span class="sxs-lookup"><span data-stu-id="8ba3d-109">Run the following command in Package Manager Console (Tools –> NuGet Package Manager –> Package Manager Console).</span></span> <span data-ttu-id="8ba3d-110">Visualizzare [Console di gestione pacchetti (Visual Studio)](../../core/miscellaneous/cli/powershell.md) alle opzioni di comando per eseguire lo scaffolding di un subset di tabelle e così via.</span><span class="sxs-lookup"><span data-stu-id="8ba3d-110">See [Package Manager Console (Visual Studio)](../../core/miscellaneous/cli/powershell.md) for command options to scaffold a subset of tables etc.</span></span>

``` powershell
Scaffold-DbContext "<connection string>" <database provider name>
```

<span data-ttu-id="8ba3d-111">Ad esempio, ecco il comando per eseguire lo scaffolding di un modello dal database Blogging nell'istanza di LocalDB di SQL Server.</span><span class="sxs-lookup"><span data-stu-id="8ba3d-111">For example, here is the command to scaffold a model from the Blogging database on your SQL Server LocalDB instance.</span></span>

``` powershell
Scaffold-DbContext "Server=(localdb)\mssqllocaldb;Database=Blogging;Trusted_Connection=True;" Microsoft.EntityFrameworkCore.SqlServer
```

## <a name="remove-ef6-model"></a><span data-ttu-id="8ba3d-112">Rimuovi modello EF6</span><span class="sxs-lookup"><span data-stu-id="8ba3d-112">Remove EF6 model</span></span>

<span data-ttu-id="8ba3d-113">A questo punto si annullerebbe il modello di Entity Framework 6 dall'applicazione.</span><span class="sxs-lookup"><span data-stu-id="8ba3d-113">You would now remove the EF6 model from your application.</span></span>

<span data-ttu-id="8ba3d-114">È possibile lasciare il pacchetto NuGet di Entity Framework 6 (Entity Framework) installato, come EF Core ed EF6 può essere utilizzata side-by-side nella stessa applicazione.</span><span class="sxs-lookup"><span data-stu-id="8ba3d-114">It is fine to leave the EF6 NuGet package (EntityFramework) installed, as EF Core and EF6 can be used side-by-side in the same application.</span></span> <span data-ttu-id="8ba3d-115">Tuttavia, se non si intende usare EF6 in tutte le aree dell'applicazione, quindi la disinstallazione del pacchetto documentazione offre errori di compilazione in parti di codice che richiedono attenzione.</span><span class="sxs-lookup"><span data-stu-id="8ba3d-115">However, if you aren't intending to use EF6 in any areas of your application, then uninstalling the package will help give compile errors on pieces of code that need attention.</span></span>

## <a name="update-your-code"></a><span data-ttu-id="8ba3d-116">Aggiornare il codice</span><span class="sxs-lookup"><span data-stu-id="8ba3d-116">Update your code</span></span>

<span data-ttu-id="8ba3d-117">A questo punto, è una questione di indirizzamento degli errori di compilazione e revisione codice per vedere se le modifiche di comportamento tra EF6 ed EF Core possibili conseguenze per te.</span><span class="sxs-lookup"><span data-stu-id="8ba3d-117">At this point, it's a matter of addressing compilation errors and reviewing code to see if the behavior changes between EF6 and EF Core will impact you.</span></span>

## <a name="test-the-port"></a><span data-ttu-id="8ba3d-118">La porta di test</span><span class="sxs-lookup"><span data-stu-id="8ba3d-118">Test the port</span></span>

<span data-ttu-id="8ba3d-119">Il fatto che l'applicazione viene compilata, non significa che è al termine del trasferimento in EF Core.</span><span class="sxs-lookup"><span data-stu-id="8ba3d-119">Just because your application compiles, does not mean it is successfully ported to EF Core.</span></span> <span data-ttu-id="8ba3d-120">È necessario testare tutte le aree dell'applicazione per garantire che nessuna delle modifiche di comportamento hanno effetti negativi a causa dell'applicazione.</span><span class="sxs-lookup"><span data-stu-id="8ba3d-120">You will need to test all areas of your application to ensure that none of the behavior changes have adversely impacted your application.</span></span>

> [!TIP]
> <span data-ttu-id="8ba3d-121">Visualizzare [Introduzione a EF Core in ASP.NET Core con un Database esistente](xref:core/get-started/aspnetcore/existing-db) per un riferimento aggiuntivo relativo all'utilizzo con un database esistente,</span><span class="sxs-lookup"><span data-stu-id="8ba3d-121">See [Getting Started with EF Core on ASP.NET Core with an Existing Database](xref:core/get-started/aspnetcore/existing-db) for an additional reference on how to work with an existing database,</span></span> 
