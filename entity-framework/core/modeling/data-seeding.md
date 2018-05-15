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
---
# <a name="data-seeding"></a>Seeding dei dati

> [!NOTE]  
> Questa funzionalità è nuova in Entity Framework Core 2.1.

Seeding dei dati consente di fornire dati iniziali per popolare un database. A differenza in EF6, in Entity Framework Core, seeding dei dati è associato un tipo di entità come parte della configurazione del modello. Quindi Core EF [migrazioni](xref:core/managing-schemas/migrations/index) consente di calcolare automaticamente cosa inserire, aggiornare o eliminare le operazioni devono essere applicate durante l'aggiornamento del database a una nuova versione del modello.

Ad esempio, è possibile utilizzare questo per configurare i dati di inizializzazione per un `Blog` in `OnModelCreating`:

[!code-csharp[Main](../../../samples/core/DataSeeding/DataSeedingContext.cs?name=BlogSeed)]

Per aggiungere entità che dispongono di valori di chiave esterna di una relazione deve essere specificato. Spesso le proprietà di chiave esterne sono in stato di ombreggiatura, pertanto per essere in grado di impostare i valori di una classe anonima deve essere utilizzata:

[!code-csharp[Main](../../../samples/core/DataSeeding/DataSeedingContext.cs?name=PostSeed)]

Dopo aver aggiunto le entità, si consiglia di usare [migrazioni](xref:core/managing-schemas/migrations/index) per applicare le modifiche. 

In alternativa, è possibile utilizzare `context.Database.EnsureCreated()` per creare un nuovo database che contiene i dati di valore di inizializzazione, ad esempio per un database di prova o quando si utilizza il provider in memoria. Si noti che se il database esiste già, `EnsureCreated()` non aggiornerà lo schema né i dati di valore di inizializzazione nel database.
