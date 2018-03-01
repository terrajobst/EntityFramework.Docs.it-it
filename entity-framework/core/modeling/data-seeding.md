---
title: Seeding dei dati - Core a Entity Framework
author: AndriySvyryd
ms.author: divega
ms.date: 02/23/2018
ms.assetid: 3154BF3C-1749-4C60-8D51-AE86773AA116
ms.technology: entity-framework-core
uid: core/modeling/data-seeding
ms.openlocfilehash: 693ffe44e247a79e01ac7c98a36472bf2c68d37f
ms.sourcegitcommit: b2d94cebdc32edad4fecb07e53fece66437d1b04
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/28/2018
---
# <a name="data-seeding"></a><span data-ttu-id="99c17-102">Seeding dei dati</span><span class="sxs-lookup"><span data-stu-id="99c17-102">Data Seeding</span></span>

> [!NOTE]  
> <span data-ttu-id="99c17-103">Questa funzionalità è nuova in Entity Framework Core 2.1.</span><span class="sxs-lookup"><span data-stu-id="99c17-103">This feature is new in EF Core 2.1.</span></span>

<span data-ttu-id="99c17-104">Seeding dei dati consente di fornire dati iniziali per popolare un database.</span><span class="sxs-lookup"><span data-stu-id="99c17-104">Data seeding allows to provide initial data to populate a database.</span></span> <span data-ttu-id="99c17-105">A differenza in EF6, in Entity Framework Core, seeding dei dati è associato un tipo di entità come parte della configurazione del modello.</span><span class="sxs-lookup"><span data-stu-id="99c17-105">Unlike in EF6, in EF Core, seeding data is associated with an entity type as part of the model configuration.</span></span> <span data-ttu-id="99c17-106">Migrazioni di EF Core possono calcolare automaticamente cosa inserire, aggiornare o eliminare le operazioni devono essere applicate durante l'aggiornamento del database a una nuova versione del modello.</span><span class="sxs-lookup"><span data-stu-id="99c17-106">Then EF Core migrations can automatically compute what insert, update or delete operations need to be applied when upgrading the database to a new version of the model.</span></span>

<span data-ttu-id="99c17-107">Ad esempio, è possibile utilizzare questo per configurare i dati di inizializzazione per un `Blog` in `OnModelCreating`:</span><span class="sxs-lookup"><span data-stu-id="99c17-107">As an example, you can use this to configure seed data for a `Blog` in `OnModelCreating`:</span></span>

[!code-csharp[Main](../../../samples/core/DataSeeding/DataSeedingContext.cs?name=BlogSeed)]

<span data-ttu-id="99c17-108">Per aggiungere entità che dispongono di valori di chiave esterna di una relazione deve essere specificato.</span><span class="sxs-lookup"><span data-stu-id="99c17-108">To add entities that have a relationship the foreign key values need to be specified.</span></span> <span data-ttu-id="99c17-109">Spesso le proprietà di chiave esterne sono in stato di ombreggiatura, pertanto per essere in grado di impostare i valori di una classe anonima deve essere utilizzata:</span><span class="sxs-lookup"><span data-stu-id="99c17-109">Frequently the foreign key properties are in shadow state, so to be able to set the values an anonymous class should be used:</span></span>

[!code-csharp[Main](../../../samples/core/DataSeeding/DataSeedingContext.cs?name=PostSeed)]
