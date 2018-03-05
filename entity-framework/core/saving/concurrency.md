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
# <a name="handling-concurrency-conflicts"></a><span data-ttu-id="7f29f-102">Gestione dei conflitti di concorrenza</span><span class="sxs-lookup"><span data-stu-id="7f29f-102">Handling Concurrency Conflicts</span></span>

> [!NOTE]
> <span data-ttu-id="7f29f-103">Questa pagina illustra il funzionamento di concorrenza in EF Core e come gestire i conflitti di concorrenza nell'applicazione.</span><span class="sxs-lookup"><span data-stu-id="7f29f-103">This page documents how concurrency works in EF Core and how to handle concurrency conflicts in your application.</span></span> <span data-ttu-id="7f29f-104">Vedere [i token di concorrenza](xref:core/modeling/concurrency) per informazioni dettagliate su come configurare i token di concorrenza nel modello.</span><span class="sxs-lookup"><span data-stu-id="7f29f-104">See [Concurrency Tokens](xref:core/modeling/concurrency) for details on how to configure concurrency tokens in your model.</span></span>

> [!TIP]
> <span data-ttu-id="7f29f-105">È possibile visualizzare l'[esempio](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Saving/Saving/Concurrency/) di questo articolo in GitHub.</span><span class="sxs-lookup"><span data-stu-id="7f29f-105">You can view this article's [sample](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Saving/Saving/Concurrency/) on GitHub.</span></span>

<span data-ttu-id="7f29f-106">_Concorrenza del database_ fa riferimento alle situazioni in cui più processi o gli utenti accedano o modificano gli stessi dati in un database nello stesso momento.</span><span class="sxs-lookup"><span data-stu-id="7f29f-106">_Database concurrency_ refers to situations in which multiple processes or users access or change the same data in a database at the same time.</span></span> <span data-ttu-id="7f29f-107">_Controllo della concorrenza_ fa riferimento a meccanismi specifici per garantire la coerenza dei dati in presenza di modifiche simultanee.</span><span class="sxs-lookup"><span data-stu-id="7f29f-107">_Concurrency control_ refers to specific mechanisms used to ensure data consistency in presence of concurrent changes.</span></span>

<span data-ttu-id="7f29f-108">Componenti di base di Entity Framework implementa _controllo della concorrenza ottimistica_, vale a dire che sarà possibile più processi o gli utenti di apportare modifiche in modo indipendente senza l'overhead di sincronizzazione o di blocco.</span><span class="sxs-lookup"><span data-stu-id="7f29f-108">EF Core implements _optimistic concurrency control_, meaning that it will let multiple processes or users make changes independently without the overhead of synchronization or locking.</span></span> <span data-ttu-id="7f29f-109">Nella situazione ideale, queste modifiche non interferisce con l'altro e pertanto saranno possibile abbia esito positivo.</span><span class="sxs-lookup"><span data-stu-id="7f29f-109">In the ideal situation, these changes will not interfere with each other and therefore will be able to succeed.</span></span> <span data-ttu-id="7f29f-110">Nello scenario del caso peggiore, due o più processi verrà eseguito un tentativo di apportare modifiche in conflitto e solo uno di essi dovrebbe avere esito positivo.</span><span class="sxs-lookup"><span data-stu-id="7f29f-110">In the worst case scenario, two or more processes will attempt to make conflicting changes, and only one of them should succeed.</span></span>

## <a name="how-concurrency-control-works-in-ef-core"></a><span data-ttu-id="7f29f-111">Funzionamento del controllo della concorrenza in EF Core</span><span class="sxs-lookup"><span data-stu-id="7f29f-111">How concurrency control works in EF Core</span></span>

<span data-ttu-id="7f29f-112">Proprietà configurata come token di concorrenza vengono utilizzate per implementare il controllo della concorrenza ottimistica: ogni volta che un'operazione update o delete viene eseguita durante `SaveChanges`, il valore del token di concorrenza nel database viene confrontato con originale valore letto da Entity Framework Core.</span><span class="sxs-lookup"><span data-stu-id="7f29f-112">Properties configured as concurrency tokens are used to implement optimistic concurrency control: whenever an update or delete operation is performed during `SaveChanges`, the value of the concurrency token on the database is compared against the original value read by EF Core.</span></span>

- <span data-ttu-id="7f29f-113">Se i valori corrispondono, è possibile completare l'operazione.</span><span class="sxs-lookup"><span data-stu-id="7f29f-113">If the values match, the operation can complete.</span></span>
- <span data-ttu-id="7f29f-114">Se i valori non corrispondono, Core EF presuppone che un altro utente ha eseguito un'operazione in conflitto e interrompe la transazione corrente.</span><span class="sxs-lookup"><span data-stu-id="7f29f-114">If the values do not match, EF Core assumes that another user has performed a conflicting operation and aborts the current transaction.</span></span>

<span data-ttu-id="7f29f-115">La situazione quando un altro utente ha eseguito un'operazione che è in conflitto con l'operazione corrente è noto come _conflitto di concorrenza_.</span><span class="sxs-lookup"><span data-stu-id="7f29f-115">The situation when another user has performed an operation that conflicts with the current operation is known as _concurrency conflict_.</span></span>

<span data-ttu-id="7f29f-116">Provider di database sono responsabili dell'implementazione il confronto di valori dei token di concorrenza.</span><span class="sxs-lookup"><span data-stu-id="7f29f-116">Database providers are responsible for implementing the comparison of concurrency token values.</span></span>

<span data-ttu-id="7f29f-117">Nei database relazionali EF Core include un controllo per il valore del token di concorrenza nel `WHERE` clausola di qualsiasi `UPDATE` o `DELETE` istruzioni.</span><span class="sxs-lookup"><span data-stu-id="7f29f-117">On relational databases EF Core includes a check for the value of the concurrency token in the `WHERE` clause of any `UPDATE` or `DELETE` statements.</span></span> <span data-ttu-id="7f29f-118">Dopo avere eseguito le istruzioni, Core EF legge il numero di righe interessate.</span><span class="sxs-lookup"><span data-stu-id="7f29f-118">After executing the statements, EF Core reads the number of rows that were affected.</span></span>

<span data-ttu-id="7f29f-119">Se nessuna riga è interessata, viene rilevato un conflitto di concorrenza e genera un'eccezione Core EF `DbUpdateConcurrencyException`.</span><span class="sxs-lookup"><span data-stu-id="7f29f-119">If no rows are affected, a concurrency conflict is detected, and EF Core throws `DbUpdateConcurrencyException`.</span></span>

<span data-ttu-id="7f29f-120">Ad esempio, si può decidere di configurare `LastName` su `Person` da un token di concorrenza.</span><span class="sxs-lookup"><span data-stu-id="7f29f-120">For example, we may want to configure `LastName` on `Person` to be a concurrency token.</span></span> <span data-ttu-id="7f29f-121">Qualsiasi operazione di aggiornamento sulla persona includerà il controllo della concorrenza nel `WHERE` clausola:</span><span class="sxs-lookup"><span data-stu-id="7f29f-121">Then any update operation on Person will include the concurrency check in the `WHERE` clause:</span></span>

``` sql
UPDATE [Person] SET [FirstName] = @p1
WHERE [PersonId] = @p0 AND [LastName] = @p2;
```

## <a name="resolving-concurrency-conflicts"></a><span data-ttu-id="7f29f-122">Risoluzione dei conflitti di concorrenza</span><span class="sxs-lookup"><span data-stu-id="7f29f-122">Resolving concurrency conflicts</span></span>

<span data-ttu-id="7f29f-123">Continuando con l'esempio precedente, se un utente tenta di salvare alcune modifiche apportate a un `Person`, ma un altro utente ha cambiato il `LastName` il verrà generata un'eccezione.</span><span class="sxs-lookup"><span data-stu-id="7f29f-123">Continuing with the previous example, if one user tries to save some changes to a `Person`, but another user has already changed the `LastName` the an exception will be thrown.</span></span>

<span data-ttu-id="7f29f-124">A questo punto, l'applicazione potrebbe semplicemente informare l'utente che l'aggiornamento non è riuscito a causa di modifiche in conflitto e passare.</span><span class="sxs-lookup"><span data-stu-id="7f29f-124">At this point, the application could simply inform the user that the update was not successful due to conflicting changes and move on.</span></span> <span data-ttu-id="7f29f-125">Ma può essere opportuno per richiedere all'utente per garantire che questo record rappresenta comunque la stessa persona e ripetere l'operazione.</span><span class="sxs-lookup"><span data-stu-id="7f29f-125">But it may be desirable to prompt the user to ensure this record still represents the same actual person and to retry the operation.</span></span>

<span data-ttu-id="7f29f-126">Questo processo è un esempio di _risolve un conflitto di concorrenza_.</span><span class="sxs-lookup"><span data-stu-id="7f29f-126">This process is an example of _resolving a concurrency conflict_.</span></span>

<span data-ttu-id="7f29f-127">Risoluzione di un conflitto di concorrenza comporta l'unione delle modifiche in sospeso da corrente `DbContext` con i valori nel database.</span><span class="sxs-lookup"><span data-stu-id="7f29f-127">Resolving a concurrency conflict involves merging the pending changes from the current `DbContext` with the values in the database.</span></span> <span data-ttu-id="7f29f-128">I valori che viene uniti variano in base all'applicazione e potrebbero essere diretti dall'input dell'utente.</span><span class="sxs-lookup"><span data-stu-id="7f29f-128">What values get merged will vary based on the application and may be directed by user input.</span></span>

<span data-ttu-id="7f29f-129">**Sono disponibili tre set di valori disponibili per risolvere un conflitto di concorrenza:**</span><span class="sxs-lookup"><span data-stu-id="7f29f-129">**There are three sets of values available to help resolve a concurrency conflict:**</span></span>

* <span data-ttu-id="7f29f-130">**I valori correnti** sono i valori che l'applicazione ha tentato di scrivere nel database.</span><span class="sxs-lookup"><span data-stu-id="7f29f-130">**Current values** are the values that the application was attempting to write to the database.</span></span>

* <span data-ttu-id="7f29f-131">**I valori originali** sono i valori che sono stati originariamente recuperati dal database, prima di apportare eventuali modifiche.</span><span class="sxs-lookup"><span data-stu-id="7f29f-131">**Original values** are the values that were originally retrieved from the database, before any edits were made.</span></span>

* <span data-ttu-id="7f29f-132">**I valori del database** sono i valori attualmente archiviati nel database.</span><span class="sxs-lookup"><span data-stu-id="7f29f-132">**Database values** are the values currently stored in the database.</span></span>

<span data-ttu-id="7f29f-133">L'approccio generale per gestire i conflitti di concorrenza è:</span><span class="sxs-lookup"><span data-stu-id="7f29f-133">The general approach to handle a concurrency conflicts is:</span></span>

1. <span data-ttu-id="7f29f-134">Catch `DbUpdateConcurrencyException` durante `SaveChanges`.</span><span class="sxs-lookup"><span data-stu-id="7f29f-134">Catch `DbUpdateConcurrencyException` during `SaveChanges`.</span></span>
2. <span data-ttu-id="7f29f-135">Utilizzare `DbUpdateConcurrencyException.Entries` per preparare un nuovo set di modifiche per le entità interessate.</span><span class="sxs-lookup"><span data-stu-id="7f29f-135">Use `DbUpdateConcurrencyException.Entries` to prepare a new set of changes for the affected entities.</span></span>
3. <span data-ttu-id="7f29f-136">Aggiornare i valori originali del token di concorrenza in modo da riflettere i valori correnti nel database.</span><span class="sxs-lookup"><span data-stu-id="7f29f-136">Refresh the original values of the concurrency token to reflect the current values in the database.</span></span>
4. <span data-ttu-id="7f29f-137">Ripetere il processo fino a quando non si verifichino conflitti.</span><span class="sxs-lookup"><span data-stu-id="7f29f-137">Retry the process until no conflicts occur.</span></span>

<span data-ttu-id="7f29f-138">Nell'esempio seguente, `Person.FirstName` e `Person.LastName` siano impostati come token di concorrenza.</span><span class="sxs-lookup"><span data-stu-id="7f29f-138">In the following example, `Person.FirstName` and `Person.LastName` are setup as concurrency tokens.</span></span> <span data-ttu-id="7f29f-139">È presente un `// TODO:` commento nella posizione in cui includere la logica specifica dell'applicazione per scegliere il valore deve essere salvato.</span><span class="sxs-lookup"><span data-stu-id="7f29f-139">There is a `// TODO:` comment in the location where you include application specific logic to choose the value to be saved.</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Saving/Concurrency/Sample.cs?name=ConcurrencyHandlingCode&highlight=34-35)]
