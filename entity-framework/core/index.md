---
title: Panoramica di Entity Framework Core - EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: bc2a2676-bc46-493f-bf49-e3cc97994d57
uid: core/index
ms.openlocfilehash: e6127f775d6bbbdf81debf5519388fe252fe079d
ms.sourcegitcommit: 18ab4c349473d94b15b4ca977df12147db07b77f
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 11/06/2019
ms.locfileid: "73655617"
---
# <a name="entity-framework-core"></a><span data-ttu-id="dccc1-102">Entity Framework Core</span><span class="sxs-lookup"><span data-stu-id="dccc1-102">Entity Framework Core</span></span>

<span data-ttu-id="dccc1-103">Entity Framework (EF) Core è una versione semplice, estendibile, [open source](https://github.com/aspnet/EntityFrameworkCore) e multipiattaforma della tecnologia di accesso ai dati di grande diffusione Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="dccc1-103">Entity Framework (EF) Core is a lightweight, extensible, [open source](https://github.com/aspnet/EntityFrameworkCore) and cross-platform version of the popular Entity Framework data access technology.</span></span>

<span data-ttu-id="dccc1-104">Entity Framework Core può essere usato come mapper relazionale a oggetti (O/RM), consentendo agli sviluppatori .NET di utilizzare un database con oggetti .NET ed eliminando la necessità della maggior parte del codice di accesso ai dati che è in genere necessario scrivere.</span><span class="sxs-lookup"><span data-stu-id="dccc1-104">EF Core can serve as an object-relational mapper (O/RM), enabling .NET developers to work with a database using .NET objects, and eliminating the need for most of the data-access code they usually need to write.</span></span>

<span data-ttu-id="dccc1-105">EF Core supporta molti motori di database. Per informazioni dettagliate, vedere [Provider di Database](providers/index.md).</span><span class="sxs-lookup"><span data-stu-id="dccc1-105">EF Core supports many database engines, see [Database Providers](providers/index.md) for details.</span></span>

## <a name="the-model"></a><span data-ttu-id="dccc1-106">Il modello</span><span class="sxs-lookup"><span data-stu-id="dccc1-106">The Model</span></span>

<span data-ttu-id="dccc1-107">Con Entity Framework Core, l'accesso ai dati viene eseguito tramite un modello.</span><span class="sxs-lookup"><span data-stu-id="dccc1-107">With EF Core, data access is performed using a model.</span></span> <span data-ttu-id="dccc1-108">Un modello è costituito da classi di entità e da un contesto dell'oggetto che rappresenta una sessione con il database che consente di eseguire una query e salvare i dati.</span><span class="sxs-lookup"><span data-stu-id="dccc1-108">A model is made up of entity classes and a context object that represents a session with the database, allowing you to query and save data.</span></span> <span data-ttu-id="dccc1-109">Per altre informazioni, vedere [Creazione di un modello](modeling/index.md).</span><span class="sxs-lookup"><span data-stu-id="dccc1-109">See [Creating a Model](modeling/index.md) to learn more.</span></span>

<span data-ttu-id="dccc1-110">È possibile generare un modello da un database esistente, scrivere manualmente il codice di un modello in modo che corrisponda al database o usare [migrazioni di Entity Framework](managing-schemas/migrations/index.md) per creare un database a partire dal modello e quindi svilupparlo man mano che il modello cambia nel tempo.</span><span class="sxs-lookup"><span data-stu-id="dccc1-110">You can generate a model from an existing database, hand code a model to match your database, or use [EF Migrations](managing-schemas/migrations/index.md) to create a database from your model, and then evolve it as your model changes over time.</span></span>

[!code-csharp[Main](../../samples/core/Intro/Model.cs)]

## <a name="querying"></a><span data-ttu-id="dccc1-111">Query</span><span class="sxs-lookup"><span data-stu-id="dccc1-111">Querying</span></span>

<span data-ttu-id="dccc1-112">Le istanze di classi di entità vengono recuperate dal database tramite LINQ (Language Integrated Query).</span><span class="sxs-lookup"><span data-stu-id="dccc1-112">Instances of your entity classes are retrieved from the database using Language Integrated Query (LINQ).</span></span> <span data-ttu-id="dccc1-113">Per altre informazioni, vedere [Esecuzione di query su dati](querying/index.md).</span><span class="sxs-lookup"><span data-stu-id="dccc1-113">See [Querying Data](querying/index.md) to learn more.</span></span>

[!code-csharp[Main](../../samples/core/Intro/Program.cs#Querying)]

## <a name="saving-data"></a><span data-ttu-id="dccc1-114">Salvataggio di dati</span><span class="sxs-lookup"><span data-stu-id="dccc1-114">Saving Data</span></span>

<span data-ttu-id="dccc1-115">I dati vengano creati, eliminati e modificati nel database tramite le istanze di classi di entità.</span><span class="sxs-lookup"><span data-stu-id="dccc1-115">Data is created, deleted, and modified in the database using instances of your entity classes.</span></span> <span data-ttu-id="dccc1-116">Per altre informazioni, vedere [Salvataggio di dati](saving/index.md).</span><span class="sxs-lookup"><span data-stu-id="dccc1-116">See [Saving Data](saving/index.md) to learn more.</span></span>

[!code-csharp[Main](../../samples/core/Intro/Program.cs#SavingData)]

## <a name="next-steps"></a><span data-ttu-id="dccc1-117">Passaggi successivi</span><span class="sxs-lookup"><span data-stu-id="dccc1-117">Next steps</span></span>

<span data-ttu-id="dccc1-118">Per le esercitazioni introduttive, vedere [Introduzione a Entity Framework Core](get-started/index.md).</span><span class="sxs-lookup"><span data-stu-id="dccc1-118">For introductory tutorials, see [Getting Started with Entity Framework Core](get-started/index.md).</span></span>
