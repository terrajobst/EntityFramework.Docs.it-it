---
title: Gestione dei conflitti di concorrenza - Core EF
author: rowanmiller
ms.author: divega
ms.date: 03/03/2018
ms.technology: entity-framework-core
uid: core/saving/concurrency
ms.openlocfilehash: 288d9c6fced5ebbaa2c366248c68547502c3698e
ms.sourcegitcommit: 8f3be0a2a394253efb653388ec66bda964e5ee1b
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/05/2018
---
# <a name="handling-concurrency-conflicts"></a>Gestione dei conflitti di concorrenza

> [!NOTE]
> Questa pagina illustra il funzionamento di concorrenza in EF Core e come gestire i conflitti di concorrenza nell'applicazione. Vedere [i token di concorrenza](xref:core/modeling/concurrency) per informazioni dettagliate su come configurare i token di concorrenza nel modello.

> [!TIP]
> È possibile visualizzare l'[esempio](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Saving/Saving/Concurrency/) di questo articolo in GitHub.

_Concorrenza del database_ fa riferimento alle situazioni in cui più processi o gli utenti accedano o modificano gli stessi dati in un database nello stesso momento. _Controllo della concorrenza_ fa riferimento a meccanismi specifici per garantire la coerenza dei dati in presenza di modifiche simultanee.

Componenti di base di Entity Framework implementa _controllo della concorrenza ottimistica_, vale a dire che sarà possibile più processi o gli utenti di apportare modifiche in modo indipendente senza l'overhead di sincronizzazione o di blocco. Nella situazione ideale, queste modifiche non interferisce con l'altro e pertanto saranno possibile abbia esito positivo. Nello scenario del caso peggiore, due o più processi verrà eseguito un tentativo di apportare modifiche in conflitto e solo uno di essi dovrebbe avere esito positivo.

## <a name="how-concurrency-control-works-in-ef-core"></a>Funzionamento del controllo della concorrenza in EF Core

Proprietà configurata come token di concorrenza vengono utilizzate per implementare il controllo della concorrenza ottimistica: ogni volta che un'operazione update o delete viene eseguita durante `SaveChanges`, il valore del token di concorrenza nel database viene confrontato con originale valore letto da Entity Framework Core.

- Se i valori corrispondono, è possibile completare l'operazione.
- Se i valori non corrispondono, Core EF presuppone che un altro utente ha eseguito un'operazione in conflitto e interrompe la transazione corrente.

La situazione quando un altro utente ha eseguito un'operazione che è in conflitto con l'operazione corrente è noto come _conflitto di concorrenza_.

Provider di database sono responsabili dell'implementazione il confronto di valori dei token di concorrenza.

Nei database relazionali EF Core include un controllo per il valore del token di concorrenza nel `WHERE` clausola di qualsiasi `UPDATE` o `DELETE` istruzioni. Dopo avere eseguito le istruzioni, Core EF legge il numero di righe interessate.

Se nessuna riga è interessata, viene rilevato un conflitto di concorrenza e genera un'eccezione Core EF `DbUpdateConcurrencyException`.

Ad esempio, si può decidere di configurare `LastName` su `Person` da un token di concorrenza. Qualsiasi operazione di aggiornamento sulla persona includerà il controllo della concorrenza nel `WHERE` clausola:

``` sql
UPDATE [Person] SET [FirstName] = @p1
WHERE [PersonId] = @p0 AND [LastName] = @p2;
```

## <a name="resolving-concurrency-conflicts"></a>Risoluzione dei conflitti di concorrenza

Continuando con l'esempio precedente, se un utente tenta di salvare alcune modifiche apportate a un `Person`, ma un altro utente ha cambiato il `LastName` il verrà generata un'eccezione.

A questo punto, l'applicazione potrebbe semplicemente informare l'utente che l'aggiornamento non è riuscito a causa di modifiche in conflitto e passare. Ma può essere opportuno per richiedere all'utente per garantire che questo record rappresenta comunque la stessa persona e ripetere l'operazione.

Questo processo è un esempio di _risolve un conflitto di concorrenza_.

Risoluzione di un conflitto di concorrenza comporta l'unione delle modifiche in sospeso da corrente `DbContext` con i valori nel database. I valori che viene uniti variano in base all'applicazione e potrebbero essere diretti dall'input dell'utente.

**Sono disponibili tre set di valori disponibili per risolvere un conflitto di concorrenza:**

* **I valori correnti** sono i valori che l'applicazione ha tentato di scrivere nel database.

* **I valori originali** sono i valori che sono stati originariamente recuperati dal database, prima di apportare eventuali modifiche.

* **I valori del database** sono i valori attualmente archiviati nel database.

L'approccio generale per gestire i conflitti di concorrenza è:

1. Catch `DbUpdateConcurrencyException` durante `SaveChanges`.
2. Utilizzare `DbUpdateConcurrencyException.Entries` per preparare un nuovo set di modifiche per le entità interessate.
3. Aggiornare i valori originali del token di concorrenza in modo da riflettere i valori correnti nel database.
4. Ripetere il processo fino a quando non si verifichino conflitti.

Nell'esempio seguente, `Person.FirstName` e `Person.LastName` siano impostati come token di concorrenza. È presente un `// TODO:` commento nella posizione in cui includere la logica specifica dell'applicazione per scegliere il valore deve essere salvato.

[!code-csharp[Main](../../../samples/core/Saving/Saving/Concurrency/Sample.cs?name=ConcurrencyHandlingCode&highlight=34-35)]
