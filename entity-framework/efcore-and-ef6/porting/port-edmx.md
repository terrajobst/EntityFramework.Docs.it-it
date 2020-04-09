---
title: Conversione da EF6 a EF Core - Porting di un modello basato su EDMX - EF
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 63003709-f1ec-4bdc-8083-65a60c4826d2
uid: efcore-and-ef6/porting/port-edmx
ms.openlocfilehash: f0bb06dc687aaa774981d97daadc55f00fbd527e
ms.sourcegitcommit: 9b562663679854c37c05fca13d93e180213fb4aa
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/07/2020
ms.locfileid: "78416923"
---
# <a name="porting-an-ef6-edmx-based-model-to-ef-core"></a><span data-ttu-id="8b0a3-102">Conversione di un modello basato su EF6 EDMX in EF Core</span><span class="sxs-lookup"><span data-stu-id="8b0a3-102">Porting an EF6 EDMX-Based Model to EF Core</span></span>

<span data-ttu-id="8b0a3-103">EF Core non supporta il formato di file EDMX per i modelli.</span><span class="sxs-lookup"><span data-stu-id="8b0a3-103">EF Core does not support the EDMX file format for models.</span></span> <span data-ttu-id="8b0a3-104">L'opzione migliore per eseguire il porting di questi modelli consiste nel generare un nuovo modello basato su codice dal database per l'applicazione.</span><span class="sxs-lookup"><span data-stu-id="8b0a3-104">The best option to port these models, is to generate a new code-based model from the database for your application.</span></span>

## <a name="install-ef-core-nuget-packages"></a><span data-ttu-id="8b0a3-105">Installare i pacchetti NuGet di EF CoreInstall EF Core NuGet packages</span><span class="sxs-lookup"><span data-stu-id="8b0a3-105">Install EF Core NuGet packages</span></span>

<span data-ttu-id="8b0a3-106">Installare il pacchetto NuGet `Microsoft.EntityFrameworkCore.Tools`.</span><span class="sxs-lookup"><span data-stu-id="8b0a3-106">Install the `Microsoft.EntityFrameworkCore.Tools` NuGet package.</span></span>

## <a name="regenerate-the-model"></a><span data-ttu-id="8b0a3-107">Rigenerare il modello</span><span class="sxs-lookup"><span data-stu-id="8b0a3-107">Regenerate the model</span></span>

<span data-ttu-id="8b0a3-108">È ora possibile utilizzare la funzionalità di decodificazione per creare un modello basato sul database esistente.</span><span class="sxs-lookup"><span data-stu-id="8b0a3-108">You can now use the reverse engineer functionality to create a model based on your existing database.</span></span>

<span data-ttu-id="8b0a3-109">Eseguire il comando seguente nella console di gestione pacchetti (Strumenti –> Gestione pacchetti NuGet –> console di gestione pacchetti).</span><span class="sxs-lookup"><span data-stu-id="8b0a3-109">Run the following command in Package Manager Console (Tools –> NuGet Package Manager –> Package Manager Console).</span></span> <span data-ttu-id="8b0a3-110">Vedere [Console di gestione pacchetti (Visual Studio)](../../core/miscellaneous/cli/powershell.md) per le opzioni di comando per eseguire lo scaffolding di un subset di tabelle e così via.</span><span class="sxs-lookup"><span data-stu-id="8b0a3-110">See [Package Manager Console (Visual Studio)](../../core/miscellaneous/cli/powershell.md) for command options to scaffold a subset of tables etc.</span></span>

``` powershell
Scaffold-DbContext "<connection string>" <database provider name>
```

<span data-ttu-id="8b0a3-111">Ad esempio, di seguito è riportato il comando per eseguire lo scaffolding di un modello dal database Blogging nell'istanza del database locale di SQL Server.For example, here is the command to scaffold a model from the Blogging database on your SQL Server LocalDB instance.</span><span class="sxs-lookup"><span data-stu-id="8b0a3-111">For example, here is the command to scaffold a model from the Blogging database on your SQL Server LocalDB instance.</span></span>

``` powershell
Scaffold-DbContext "Server=(localdb)\mssqllocaldb;Database=Blogging;Trusted_Connection=True;" Microsoft.EntityFrameworkCore.SqlServer
```

## <a name="remove-ef6-model"></a><span data-ttu-id="8b0a3-112">Rimuovere il modello EF6Remove EF6 model</span><span class="sxs-lookup"><span data-stu-id="8b0a3-112">Remove EF6 model</span></span>

<span data-ttu-id="8b0a3-113">È ora necessario rimuovere il modello EF6 dall'applicazione.</span><span class="sxs-lookup"><span data-stu-id="8b0a3-113">You would now remove the EF6 model from your application.</span></span>

<span data-ttu-id="8b0a3-114">È bene lasciare installato il pacchetto EF6 NuGet (EntityFramework), come Entity Core ed EF6 possono essere utilizzati side-by-side nella stessa applicazione.</span><span class="sxs-lookup"><span data-stu-id="8b0a3-114">It is fine to leave the EF6 NuGet package (EntityFramework) installed, as EF Core and EF6 can be used side-by-side in the same application.</span></span> <span data-ttu-id="8b0a3-115">Tuttavia, se non si intende utilizzare EF6 in tutte le aree dell'applicazione, quindi la disinstallazione del pacchetto contribuirà a dare errori di compilazione su parti di codice che richiedono attenzione.</span><span class="sxs-lookup"><span data-stu-id="8b0a3-115">However, if you aren't intending to use EF6 in any areas of your application, then uninstalling the package will help give compile errors on pieces of code that need attention.</span></span>

## <a name="update-your-code"></a><span data-ttu-id="8b0a3-116">Aggiornare il codice</span><span class="sxs-lookup"><span data-stu-id="8b0a3-116">Update your code</span></span>

<span data-ttu-id="8b0a3-117">A questo punto, è una questione di risolvere gli errori di compilazione e la revisione del codice per vedere se il comportamento cambia tra EF6 ed EF Core avrà un impatto.</span><span class="sxs-lookup"><span data-stu-id="8b0a3-117">At this point, it's a matter of addressing compilation errors and reviewing code to see if the behavior changes between EF6 and EF Core will impact you.</span></span>

## <a name="test-the-port"></a><span data-ttu-id="8b0a3-118">Testare la porta</span><span class="sxs-lookup"><span data-stu-id="8b0a3-118">Test the port</span></span>

<span data-ttu-id="8b0a3-119">Solo perché l'applicazione viene compilata, non significa che è stato eseguito correttamente il porting a EF Core.Just because your application compiles, does not mean it is successfully ported to EF Core.</span><span class="sxs-lookup"><span data-stu-id="8b0a3-119">Just because your application compiles, does not mean it is successfully ported to EF Core.</span></span> <span data-ttu-id="8b0a3-120">Sarà necessario testare tutte le aree dell'applicazione per assicurarsi che nessuna delle modifiche di comportamento abbia influito negativamente sull'applicazione.</span><span class="sxs-lookup"><span data-stu-id="8b0a3-120">You will need to test all areas of your application to ensure that none of the behavior changes have adversely impacted your application.</span></span>
