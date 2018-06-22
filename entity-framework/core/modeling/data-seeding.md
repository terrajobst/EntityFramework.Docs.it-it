---
title: Seeding dei dati - Core a Entity Framework
author: AndriySvyryd
ms.author: divega
ms.date: 02/23/2018
ms.assetid: 3154BF3C-1749-4C60-8D51-AE86773AA116
ms.technology: entity-framework-core
uid: core/modeling/data-seeding
ms.openlocfilehash: 7028e1923152b27f56721dab75aae8b9c2f5ad75
ms.sourcegitcommit: 038acd91ce2f5a28d76dcd2eab72eeba225e366d
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/14/2018
ms.locfileid: "34163200"
---
# <a name="data-seeding"></a><span data-ttu-id="d9206-102">Seeding dei dati</span><span class="sxs-lookup"><span data-stu-id="d9206-102">Data Seeding</span></span>

> [!NOTE]  
> <span data-ttu-id="d9206-103">Questa funzionalità è nuova in Entity Framework Core 2.1.</span><span class="sxs-lookup"><span data-stu-id="d9206-103">This feature is new in EF Core 2.1.</span></span>

<span data-ttu-id="d9206-104">Seeding dei dati consente di fornire dati iniziali per popolare un database.</span><span class="sxs-lookup"><span data-stu-id="d9206-104">Data seeding allows to provide initial data to populate a database.</span></span> <span data-ttu-id="d9206-105">A differenza in EF6, in Entity Framework Core, seeding dei dati è associato un tipo di entità come parte della configurazione del modello.</span><span class="sxs-lookup"><span data-stu-id="d9206-105">Unlike in EF6, in EF Core, seeding data is associated with an entity type as part of the model configuration.</span></span> <span data-ttu-id="d9206-106">Quindi Core EF [migrazioni](xref:core/managing-schemas/migrations/index) consente di calcolare automaticamente cosa inserire, aggiornare o eliminare le operazioni devono essere applicate durante l'aggiornamento del database a una nuova versione del modello.</span><span class="sxs-lookup"><span data-stu-id="d9206-106">Then EF Core [migrations](xref:core/managing-schemas/migrations/index) can automatically compute what insert, update or delete operations need to be applied when upgrading the database to a new version of the model.</span></span>

<span data-ttu-id="d9206-107">Ad esempio, è possibile utilizzare questo per configurare i dati di inizializzazione per un `Blog` in `OnModelCreating`:</span><span class="sxs-lookup"><span data-stu-id="d9206-107">As an example, you can use this to configure seed data for a `Blog` in `OnModelCreating`:</span></span>

[!code-csharp[Main](../../../samples/core/DataSeeding/DataSeedingContext.cs?name=BlogSeed)]

<span data-ttu-id="d9206-108">Per aggiungere entità che dispongono di valori di chiave esterna di una relazione deve essere specificato.</span><span class="sxs-lookup"><span data-stu-id="d9206-108">To add entities that have a relationship the foreign key values need to be specified.</span></span> <span data-ttu-id="d9206-109">Spesso le proprietà di chiave esterne sono in stato di ombreggiatura, pertanto per essere in grado di impostare i valori di una classe anonima deve essere utilizzata:</span><span class="sxs-lookup"><span data-stu-id="d9206-109">Frequently the foreign key properties are in shadow state, so to be able to set the values an anonymous class should be used:</span></span>

[!code-csharp[Main](../../../samples/core/DataSeeding/DataSeedingContext.cs?name=PostSeed)]

<span data-ttu-id="d9206-110">Dopo aver aggiunto le entità, si consiglia di usare [migrazioni](xref:core/managing-schemas/migrations/index) per applicare le modifiche.</span><span class="sxs-lookup"><span data-stu-id="d9206-110">Once entities have been added, it is recommended to use [migrations](xref:core/managing-schemas/migrations/index) to apply changes.</span></span> 

<span data-ttu-id="d9206-111">In alternativa, è possibile utilizzare `context.Database.EnsureCreated()` per creare un nuovo database che contiene i dati di valore di inizializzazione, ad esempio per un database di prova o quando si utilizza il provider in memoria.</span><span class="sxs-lookup"><span data-stu-id="d9206-111">Alternatively, you can use `context.Database.EnsureCreated()` to create a new database containing the seed data, for example for a test database or when using the in-memory provider.</span></span> <span data-ttu-id="d9206-112">Si noti che se il database esiste già, `EnsureCreated()` non aggiornerà lo schema né i dati di valore di inizializzazione nel database.</span><span class="sxs-lookup"><span data-stu-id="d9206-112">Note that if the database already exists, `EnsureCreated()` will neither update the schema nor the seed data in the database.</span></span>
