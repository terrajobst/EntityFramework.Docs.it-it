---
title: Tipi di entità autofirmati-EF Core
description: Come configurare i tipi di entità autochiave utilizzando Entity Framework Core
author: AndriySvyryd
ms.author: ansvyryd
ms.date: 9/13/2019
uid: core/modeling/keyless-entity-types
ms.openlocfilehash: 129e24b154ba32583435aeb742dbf478350344e8
ms.sourcegitcommit: 7a709ce4f77134782393aa802df5ab2718714479
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 12/04/2019
ms.locfileid: "74824670"
---
# <a name="keyless-entity-types"></a>Tipi di entità senza chiave

> [!NOTE]
> Questa funzionalità è stata aggiunta in EF Core 2,1 sotto il nome dei tipi di query. In EF Core 3,0 il concetto è stato rinominato in tipi di entità senza chiave.

Oltre ai normali tipi di entità, un modello di EF Core può contenere _tipi di entità_senza chiave, che possono essere usati per eseguire query di database su dati che non contengono valori di chiave.

## <a name="keyless-entity-types-characteristics"></a>Caratteristiche di tipi di entità autochiave

I tipi di entità autonome supportano molte delle stesse funzionalità di mapping dei normali tipi di entità, ad esempio il mapping di ereditarietà e le proprietà di navigazione. In archivi relazionali, è possibile configurare le oggetti di database di destinazione e le colonne tramite metodi dell'API fluent o annotazioni dei dati.

Tuttavia, sono diversi dai normali tipi di entità in quanto:

- Non è possibile definire una chiave.
- Non vengono mai rilevate per le modifiche apportate in _DbContext_ e pertanto non vengono mai inserite, aggiornate o eliminate nel database.
- Non vengono mai individuati dalla convenzione.
- Supporta solo un subset di funzionalità di mapping di navigazione, in particolare:
  - Mai agiscono come entità finale principale di una relazione.
  - Potrebbero non avere spostamenti sulle entità di proprietà
  - Possono contenere solo proprietà di navigazione di riferimento che puntano a entità regolari.
  - Le entità non possono contenere proprietà di navigazione per i tipi di entità autochiave.
- Deve essere configurato con `.HasNoKey()` chiamata al metodo.
- Può essere mappato a una _query di definizione_. Una query di definizione è una query dichiarata nel modello che funge da origine dati per un tipo di entità autonome.

## <a name="usage-scenarios"></a>Scenari di utilizzo

Di seguito sono riportati alcuni degli scenari di utilizzo principali per i tipi di entità autochiave:

- Fungendo da tipo restituito per le [query SQL non elaborate](xref:core/querying/raw-sql).
- Mapping a viste di database che non contengono una chiave primaria.
- Mapping a tabelle che non è definita una chiave primaria.
- Mapping a query definite nel modello.

## <a name="mapping-to-database-objects"></a>Mapping agli oggetti di database

Il mapping di un tipo di entità autochiave a un oggetto di database viene eseguito usando l'API `ToTable` o `ToView` Fluent. Dal punto di vista di EF Core, l'oggetto di database specificato in questo metodo è un _vista_, vale a dire che viene considerato come un'origine di query di sola lettura e non può essere la destinazione dell'aggiornamento, inserire o eliminare le operazioni. Tuttavia, ciò non significa che l'oggetto di database debba essere effettivamente una vista di database. In alternativa, può essere una tabella di database che verrà considerata di sola lettura. Viceversa, per i tipi di entità regolari, EF Core presuppone che un oggetto di database specificato nel metodo `ToTable` possa essere considerato come una _tabella_, ovvero può essere utilizzato come origine della query, ma anche come destinazione da operazioni di aggiornamento, eliminazione e inserimento. In effetti, è possibile specificare il nome di una vista di database in `ToTable` e tutto dovrebbe funzionare senza problemi, purché la visualizzazione è configurata per essere aggiornabile nel database.

> [!NOTE]
> `ToView` presuppone che l'oggetto esista già nel database e non venga creato dalle migrazioni.

## <a name="example"></a>Esempio

Nell'esempio seguente viene illustrato come utilizzare i tipi di entità autochiave per eseguire una query su una vista di database.

> [!TIP]
> È possibile visualizzare l'[esempio](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/KeylessEntityTypes) di questo articolo in GitHub.

In primo luogo, definiamo un semplice modello di post di Blog e Post:

[!code-csharp[Main](../../../samples/core/KeylessEntityTypes/Program.cs#Entities)]

Successivamente, viene definita una vista di database semplici che ci consentirà di eseguire una query il numero di post associati a ciascun blog:

[!code-csharp[Main](../../../samples/core/KeylessEntityTypes/Program.cs#View)]

Successivamente, viene definita una classe che contenga il risultato della vista di database:

[!code-csharp[Main](../../../samples/core/KeylessEntityTypes/Program.cs#KeylessEntityType)]

Successivamente, si configura il tipo di entità autochiave in _OnModelCreating_ usando l'API `HasNoKey`.
Si usa l'API di configurazione Fluent per configurare il mapping per il tipo di entità autochiave:

[!code-csharp[Main](../../../samples/core/KeylessEntityTypes/Program.cs#Configuration)]

Viene quindi configurata la `DbContext` per includere l'`DbSet<T>`:

[!code-csharp[Main](../../../samples/core/KeylessEntityTypes/Program.cs#DbSet)]

Infine, è possibile eseguire una query la visualizzazione del database nella modalità standard:

[!code-csharp[Main](../../../samples/core/KeylessEntityTypes/Program.cs#Query)]

> [!TIP]
> Si noti che è stata definita anche una proprietà di query a livello di contesto (DbSet) che funge da radice per le query su questo tipo.
