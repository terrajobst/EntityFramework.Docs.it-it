---
title: Tipi di query - EF Core
author: anpete
ms.author: anpete
ms.date: 2/26/2018
ms.assetid: 9F4450C5-1A3F-4BB6-AC19-9FAC64292AAD
ms.technology: entity-framework-core
uid: core/modeling/query-types
ms.openlocfilehash: 5a2cd451da8833daf2c315419559eb4a2c705b13
ms.sourcegitcommit: 4467032fd6ca223e5965b59912d74cf88a1dd77f
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 08/01/2018
ms.locfileid: "39388481"
---
# <a name="query-types"></a>Tipi di query
> [!NOTE]
> Questa funzionalità è una novità di EF Core 2.1

Oltre ai tipi di entità, un modello di EF Core può contenere _tipi di query_, che può essere usato per eseguire query sui dati che non sono stato eseguito il mapping ai tipi di entità di database.

Tipi di query presentano molte analogie con tipi di entità:

- Possono anche essere aggiunti al modello sia nel `OnModelCreating`, o tramite una proprietà "set" su un oggetto derivato _DbContext_.
- Supportano molte delle stesse funzionalità di mapping, come l'ereditarietà di mapping, le proprietà di navigazione (vedere le limitazioni seguenti) e, in archivi relazionali, la possibilità di configurare le oggetti di database di destinazione e le colonne tramite metodi dell'API fluent o annotazioni dei dati.

Tuttavia sono diversi da entità tipi che sono:

- Non richiedono una chiave da definire.
- Non verranno mai rilevati per le modifiche nel _DbContext_ e pertanto mai inserite, aggiornate o eliminate nel database.
- Non vengono mai individuati dalla convenzione.
- Supportano solo un subset di funzioni di mapping di spostamento, in particolare:
  - Mai agiscono come entità finale principale di una relazione.
  - Possono contenere solo le proprietà di navigazione di riferimento che punta all'entità.
  - Le entità non possono contenere le proprietà di navigazione ai tipi di query.
- Vengano affrontati nel _ModelBuilder_ usando la `Query` metodo anziché la `Entity` (metodo).
- Viene eseguito il mapping sul _DbContext_ tramite le proprietà di tipo `DbQuery<T>` anziché `DbSet<T>`
- Viene eseguito il mapping agli oggetti di database usando il `ToView` metodo, anziché `ToTable`.
- Può eseguire il mapping a un _definizione di query_ : una definizione di query è una query secondaria dichiarata nel modello che funge da un'origine dati per un tipo di query.

Alcuni degli scenari di utilizzo principali per i tipi di query sono:

- Che funge da tipo restituito per ad hoc `FromSql()` le query.
- Mapping a viste di database.
- Mapping a tabelle che non è definita una chiave primaria.
- Mapping a query definite nel modello.

> [!TIP]
> Mapping di un tipo di query a un oggetto di database viene eseguito mediante il `ToView` API fluent. Dal punto di vista di EF Core, l'oggetto di database specificato in questo metodo è un _vista_, vale a dire che viene considerato come un'origine di query di sola lettura e non può essere la destinazione dell'aggiornamento, inserire o eliminare le operazioni. Tuttavia, ciò non significa che l'oggetto di database è effettivamente necessario per essere una vista di database, in alternativa può essere una tabella di database che verrà considerata di sola lettura. Viceversa, per i tipi di entità, EF Core si presuppone che un oggetto di database specificato nella `ToTable` metodo può essere considerato come un _tabella_, vale a dire che può essere utilizzato come origine query ma anche di destinazione da aggiornare, eliminare e inserire operazioni. In effetti, è possibile specificare il nome di una vista di database in `ToTable` e tutto dovrebbe funzionare senza problemi, purché la visualizzazione è configurata per essere aggiornabile nel database.

## <a name="example"></a>Esempio

Nell'esempio seguente viene illustrato come utilizzare il tipo di Query per eseguire query di una vista di database.

> [!TIP]
> È possibile visualizzare l'[esempio](https://github.com/aspnet/EntityFrameworkCore/tree/master/samples/QueryTypes) di questo articolo in GitHub.

In primo luogo, definiamo un semplice modello di post di Blog e Post:

[!code-csharp[Main](../../../efcore-repo/samples/QueryTypes/Program.cs#Entities)]

Successivamente, viene definita una vista di database semplici che ci consentirà di eseguire una query il numero di post associati a ciascun blog:

[!code-csharp[Main](../../../efcore-repo/samples/QueryTypes/Program.cs#View)]

Successivamente, viene definita una classe che contenga il risultato della vista di database:

[!code-csharp[Main](../../../efcore-repo/samples/QueryTypes/Program.cs#QueryType)]

Successivamente, si configura il tipo di query nel _OnModelCreating_ usando il `modelBuilder.Query<T>` API.
Utilizziamo standard configurazione fluent API per configurare il mapping per il tipo di Query:

[!code-csharp[Main](../../../efcore-repo/samples/QueryTypes/Program.cs#Configuration)]

Infine, è possibile eseguire una query la visualizzazione del database nella modalità standard:

[!code-csharp[Main](../../../efcore-repo/samples/QueryTypes/Program.cs#Query)]

> [!TIP]
> Si noti che inoltre abbiamo definito una proprietà di query a livello di contesto (DbQuery) di agire come un utente root per le query su questo tipo.
