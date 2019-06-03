---
title: Informazioni di riferimento sugli strumenti di Entity Framework Core - EF Core
author: bricelam
ms.author: bricelam
ms.date: 09/19/2018
uid: core/miscellaneous/cli/index
ms.openlocfilehash: 13e80f740bc5ce3404e8dba40b65ec872c5e3f90
ms.sourcegitcommit: ea1cdec0b982b922a59b9d9301d3ed2b94baca0f
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 05/31/2019
ms.locfileid: "66452262"
---
# <a name="entity-framework-core-tools-reference"></a><span data-ttu-id="d74d4-102">Informazioni di riferimento sugli strumenti di Entity Framework Core</span><span class="sxs-lookup"><span data-stu-id="d74d4-102">Entity Framework Core tools reference</span></span>

<span data-ttu-id="d74d4-103">Gli strumenti di Entity Framework Core semplificano le attività di sviluppo in fase di progettazione.</span><span class="sxs-lookup"><span data-stu-id="d74d4-103">The Entity Framework Core tools help with design-time development tasks.</span></span> <span data-ttu-id="d74d4-104">Vengono usati principalmente per gestire le migrazioni ed eseguire lo scaffolding di un elemento `DbContext` e dei tipi di entità decompilando lo schema di un database.</span><span class="sxs-lookup"><span data-stu-id="d74d4-104">They're primarily used to manage Migrations and to scaffold a `DbContext` and entity types by reverse engineering the schema of a database.</span></span>

* <span data-ttu-id="d74d4-105">Gli [strumenti della console di Gestione pacchetti di EF Core](powershell.md) vengono eseguiti nella [console di Gestione pacchetti](https://docs.microsoft.com/nuget/tools/package-manager-console) in Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="d74d4-105">The [EF Core Package Manager Console tools](powershell.md) run in the [Package Manager Console](https://docs.microsoft.com/nuget/tools/package-manager-console) in Visual Studio.</span></span> <span data-ttu-id="d74d4-106">Questi strumenti funzionano sia con i progetti .NET Framework che con i progetti .NET Core.</span><span class="sxs-lookup"><span data-stu-id="d74d4-106">These tools work with both .NET Framework and .NET Core projects.</span></span>

* <span data-ttu-id="d74d4-107">Gli [strumenti dell'interfaccia della riga di comando di .NET EF Core](dotnet.md) sono un'estensione per gli [strumenti dell'interfaccia della riga di comando di .NET Core](https://docs.microsoft.com/dotnet/core/tools/) multipiattaforma.</span><span class="sxs-lookup"><span data-stu-id="d74d4-107">The [EF Core .NET command-line interface (CLI) tools](dotnet.md) are an extension to the cross-platform [.NET Core CLI tools](https://docs.microsoft.com/dotnet/core/tools/).</span></span> <span data-ttu-id="d74d4-108">Tali strumenti richiedono un progetto .NET Core SDK (uno con `Sdk="Microsoft.NET.Sdk"` o simili nel file di progetto).</span><span class="sxs-lookup"><span data-stu-id="d74d4-108">These tools require a .NET Core SDK project (one with `Sdk="Microsoft.NET.Sdk"` or similar in the project file).</span></span>

<span data-ttu-id="d74d4-109">Entrambi gli strumenti espongono la stessa funzionalità.</span><span class="sxs-lookup"><span data-stu-id="d74d4-109">Both tools expose the same functionality.</span></span> <span data-ttu-id="d74d4-110">Se sviluppa in Visual Studio, è consigliabile usare gli strumenti della **console di Gestione pacchetti** poiché offrono un'esperienza più integrata.</span><span class="sxs-lookup"><span data-stu-id="d74d4-110">If you're developing in Visual Studio, we recommend using the **Package Manager Console** tools since they provide a more integrated experience.</span></span>

## <a name="next-steps"></a><span data-ttu-id="d74d4-111">Passaggi successivi</span><span class="sxs-lookup"><span data-stu-id="d74d4-111">Next steps</span></span>

* [<span data-ttu-id="d74d4-112">Informazioni di riferimento sugli strumenti della console di Gestione pacchetti di EF Core</span><span class="sxs-lookup"><span data-stu-id="d74d4-112">EF Core Package Manager Console tools reference</span></span>](powershell.md)
* [<span data-ttu-id="d74d4-113">Informazioni di riferimento sugli strumenti dell'interfaccia della riga di comando di .NET di EF Core</span><span class="sxs-lookup"><span data-stu-id="d74d4-113">EF Core .NET CLI tools reference</span></span>](dotnet.md)
