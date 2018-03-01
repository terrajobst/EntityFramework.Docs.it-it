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
# <a name="data-seeding"></a>Seeding dei dati

> [!NOTE]  
> Questa funzionalità è nuova in Entity Framework Core 2.1.

Seeding dei dati consente di fornire dati iniziali per popolare un database. A differenza in EF6, in Entity Framework Core, seeding dei dati è associato un tipo di entità come parte della configurazione del modello. Migrazioni di EF Core possono calcolare automaticamente cosa inserire, aggiornare o eliminare le operazioni devono essere applicate durante l'aggiornamento del database a una nuova versione del modello.

Ad esempio, è possibile utilizzare questo per configurare i dati di inizializzazione per un `Blog` in `OnModelCreating`:

[!code-csharp[Main](../../../samples/core/DataSeeding/DataSeedingContext.cs?name=BlogSeed)]

Per aggiungere entità che dispongono di valori di chiave esterna di una relazione deve essere specificato. Spesso le proprietà di chiave esterne sono in stato di ombreggiatura, pertanto per essere in grado di impostare i valori di una classe anonima deve essere utilizzata:

[!code-csharp[Main](../../../samples/core/DataSeeding/DataSeedingContext.cs?name=PostSeed)]
