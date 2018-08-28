---
title: Seeding dei dati - EF Core
author: AndriySvyryd
ms.date: 02/23/2018
ms.assetid: 3154BF3C-1749-4C60-8D51-AE86773AA116
uid: core/modeling/data-seeding
ms.openlocfilehash: 48ba2389de4b57dbe4c2b2124911c71440d45556
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 08/27/2018
ms.locfileid: "42994479"
---
# <a name="data-seeding"></a><span data-ttu-id="1325c-102">Seeding dei dati</span><span class="sxs-lookup"><span data-stu-id="1325c-102">Data Seeding</span></span>

> [!NOTE]  
> <span data-ttu-id="1325c-103">Questa funzionalità è stata introdotta in EF Core 2.1.</span><span class="sxs-lookup"><span data-stu-id="1325c-103">This feature is new in EF Core 2.1.</span></span>

<span data-ttu-id="1325c-104">Seeding dei dati consente di specificare i dati iniziali per popolare un database.</span><span class="sxs-lookup"><span data-stu-id="1325c-104">Data seeding allows to provide initial data to populate a database.</span></span> <span data-ttu-id="1325c-105">Diversamente da ef6, in EF Core, il seeding dei dati è associato un tipo di entità come parte della configurazione del modello.</span><span class="sxs-lookup"><span data-stu-id="1325c-105">Unlike in EF6, in EF Core, seeding data is associated with an entity type as part of the model configuration.</span></span> <span data-ttu-id="1325c-106">Quindi Entity Framework Core [migrazioni](xref:core/managing-schemas/migrations/index) può calcolare automaticamente quali inserire, aggiornare o eliminare le operazioni devono essere applicate durante l'aggiornamento del database a una nuova versione del modello.</span><span class="sxs-lookup"><span data-stu-id="1325c-106">Then EF Core [migrations](xref:core/managing-schemas/migrations/index) can automatically compute what insert, update or delete operations need to be applied when upgrading the database to a new version of the model.</span></span>

<span data-ttu-id="1325c-107">Ad esempio, è possibile utilizzare questo per configurare i dati iniziali per un `Blog` in `OnModelCreating`:</span><span class="sxs-lookup"><span data-stu-id="1325c-107">As an example, you can use this to configure seed data for a `Blog` in `OnModelCreating`:</span></span>

[!code-csharp[Main](../../../samples/core/DataSeeding/DataSeedingContext.cs?name=BlogSeed)]

<span data-ttu-id="1325c-108">Per aggiungere entità contenenti una relazione di valori di chiave esterna deve essere specificato.</span><span class="sxs-lookup"><span data-stu-id="1325c-108">To add entities that have a relationship the foreign key values need to be specified.</span></span> <span data-ttu-id="1325c-109">Spesso le proprietà di chiave esterna sono nello stato shadow, pertanto per essere in grado di impostare i valori di una classe anonima deve essere utilizzata:</span><span class="sxs-lookup"><span data-stu-id="1325c-109">Frequently the foreign key properties are in shadow state, so to be able to set the values an anonymous class should be used:</span></span>

[!code-csharp[Main](../../../samples/core/DataSeeding/DataSeedingContext.cs?name=PostSeed)]

<span data-ttu-id="1325c-110">Dopo aver aggiunto le entità, è consigliabile usare [migrazioni](xref:core/managing-schemas/migrations/index) per applicare le modifiche.</span><span class="sxs-lookup"><span data-stu-id="1325c-110">Once entities have been added, it is recommended to use [migrations](xref:core/managing-schemas/migrations/index) to apply changes.</span></span> 

<span data-ttu-id="1325c-111">In alternativa, è possibile usare `context.Database.EnsureCreated()` per creare un nuovo database che contiene i dati di seeding, ad esempio per un database di test o quando si utilizza il provider in memoria.</span><span class="sxs-lookup"><span data-stu-id="1325c-111">Alternatively, you can use `context.Database.EnsureCreated()` to create a new database containing the seed data, for example for a test database or when using the in-memory provider.</span></span> <span data-ttu-id="1325c-112">Si noti che se il database esiste già, `EnsureCreated()` né aggiornerà lo schema, né i dati di seeding del database.</span><span class="sxs-lookup"><span data-stu-id="1325c-112">Note that if the database already exists, `EnsureCreated()` will neither update the schema nor the seed data in the database.</span></span>
