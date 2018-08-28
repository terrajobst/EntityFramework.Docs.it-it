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
# <a name="data-seeding"></a>Seeding dei dati

> [!NOTE]  
> Questa funzionalità è stata introdotta in EF Core 2.1.

Seeding dei dati consente di specificare i dati iniziali per popolare un database. Diversamente da ef6, in EF Core, il seeding dei dati è associato un tipo di entità come parte della configurazione del modello. Quindi Entity Framework Core [migrazioni](xref:core/managing-schemas/migrations/index) può calcolare automaticamente quali inserire, aggiornare o eliminare le operazioni devono essere applicate durante l'aggiornamento del database a una nuova versione del modello.

Ad esempio, è possibile utilizzare questo per configurare i dati iniziali per un `Blog` in `OnModelCreating`:

[!code-csharp[Main](../../../samples/core/DataSeeding/DataSeedingContext.cs?name=BlogSeed)]

Per aggiungere entità contenenti una relazione di valori di chiave esterna deve essere specificato. Spesso le proprietà di chiave esterna sono nello stato shadow, pertanto per essere in grado di impostare i valori di una classe anonima deve essere utilizzata:

[!code-csharp[Main](../../../samples/core/DataSeeding/DataSeedingContext.cs?name=PostSeed)]

Dopo aver aggiunto le entità, è consigliabile usare [migrazioni](xref:core/managing-schemas/migrations/index) per applicare le modifiche. 

In alternativa, è possibile usare `context.Database.EnsureCreated()` per creare un nuovo database che contiene i dati di seeding, ad esempio per un database di test o quando si utilizza il provider in memoria. Si noti che se il database esiste già, `EnsureCreated()` né aggiornerà lo schema, né i dati di seeding del database.
