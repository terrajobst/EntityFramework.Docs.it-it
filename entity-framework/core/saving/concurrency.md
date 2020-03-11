---
title: Gestione dei conflitti di concorrenza - EF Core
author: rowanmiller
ms.date: 03/03/2018
uid: core/saving/concurrency
ms.openlocfilehash: a1d1a5a11d482f9104691aa3c072dbd1c548e9f1
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78417590"
---
# <a name="handling-concurrency-conflicts"></a><span data-ttu-id="51873-102">Gestione dei conflitti di concorrenza</span><span class="sxs-lookup"><span data-stu-id="51873-102">Handling Concurrency Conflicts</span></span>

> [!NOTE]
> <span data-ttu-id="51873-103">Questa pagina illustra il funzionamento della concorrenza in EF Core e come gestire i conflitti di concorrenza nell'applicazione.</span><span class="sxs-lookup"><span data-stu-id="51873-103">This page documents how concurrency works in EF Core and how to handle concurrency conflicts in your application.</span></span> <span data-ttu-id="51873-104">Vedere [Token di concorrenza](xref:core/modeling/concurrency) per informazioni dettagliate su come configurare i token di concorrenza nel modello.</span><span class="sxs-lookup"><span data-stu-id="51873-104">See [Concurrency Tokens](xref:core/modeling/concurrency) for details on how to configure concurrency tokens in your model.</span></span>

> [!TIP]
> <span data-ttu-id="51873-105">È possibile visualizzare l'[esempio](https://github.com/dotnet/EntityFramework.Docs/tree/master/samples/core/Saving/Concurrency/) di questo articolo in GitHub.</span><span class="sxs-lookup"><span data-stu-id="51873-105">You can view this article's [sample](https://github.com/dotnet/EntityFramework.Docs/tree/master/samples/core/Saving/Concurrency/) on GitHub.</span></span>

<span data-ttu-id="51873-106">Con _concorrenza del database_ si fa riferimento alle situazioni in cui più processi o utenti accedono agli stessi dati in un database o li modificano nello stesso momento.</span><span class="sxs-lookup"><span data-stu-id="51873-106">_Database concurrency_ refers to situations in which multiple processes or users access or change the same data in a database at the same time.</span></span> <span data-ttu-id="51873-107">Con _controllo della concorrenza_ si fa riferimento ai meccanismi specifici usati per garantire la coerenza dei dati in presenza di modifiche simultanee.</span><span class="sxs-lookup"><span data-stu-id="51873-107">_Concurrency control_ refers to specific mechanisms used to ensure data consistency in presence of concurrent changes.</span></span>

<span data-ttu-id="51873-108">EF Core implementa il _controllo della concorrenza ottimistica_, ovvero consentirà a più processi o utenti di apportare modifiche in modo indipendente senza l'overhead di sincronizzazione o blocco.</span><span class="sxs-lookup"><span data-stu-id="51873-108">EF Core implements _optimistic concurrency control_, meaning that it will let multiple processes or users make changes independently without the overhead of synchronization or locking.</span></span> <span data-ttu-id="51873-109">In una situazione ideale, queste modifiche non interferiranno l'una con l'altra e quindi potranno essere completate.</span><span class="sxs-lookup"><span data-stu-id="51873-109">In the ideal situation, these changes will not interfere with each other and therefore will be able to succeed.</span></span> <span data-ttu-id="51873-110">Nel caso peggiore, due o più processi tenteranno di apportare modifiche in conflitto e solo uno di essi avrà esito positivo.</span><span class="sxs-lookup"><span data-stu-id="51873-110">In the worst case scenario, two or more processes will attempt to make conflicting changes, and only one of them should succeed.</span></span>

## <a name="how-concurrency-control-works-in-ef-core"></a><span data-ttu-id="51873-111">Funzionamento del controllo della concorrenza in EF Core</span><span class="sxs-lookup"><span data-stu-id="51873-111">How concurrency control works in EF Core</span></span>

<span data-ttu-id="51873-112">Le proprietà configurate come token di concorrenza vengono usate per implementare il controllo della concorrenza ottimistica: ogni volta che un'operazione di aggiornamento o eliminazione viene eseguita durante `SaveChanges`, il valore del token di concorrenza nel database viene confrontato con il valore originale letto da EF Core.</span><span class="sxs-lookup"><span data-stu-id="51873-112">Properties configured as concurrency tokens are used to implement optimistic concurrency control: whenever an update or delete operation is performed during `SaveChanges`, the value of the concurrency token on the database is compared against the original value read by EF Core.</span></span>

- <span data-ttu-id="51873-113">Se i valori corrispondono, l'operazione può essere completata.</span><span class="sxs-lookup"><span data-stu-id="51873-113">If the values match, the operation can complete.</span></span>
- <span data-ttu-id="51873-114">Se i valori non corrispondono, EF Core presuppone che un altro utente abbia eseguito un'operazione in conflitto e interrompe la transazione corrente.</span><span class="sxs-lookup"><span data-stu-id="51873-114">If the values do not match, EF Core assumes that another user has performed a conflicting operation and aborts the current transaction.</span></span>

<span data-ttu-id="51873-115">La situazione in cui un altro utente ha eseguito un'operazione in conflitto con l'operazione corrente è nota come _conflitto di concorrenza_.</span><span class="sxs-lookup"><span data-stu-id="51873-115">The situation when another user has performed an operation that conflicts with the current operation is known as _concurrency conflict_.</span></span>

<span data-ttu-id="51873-116">I provider di database sono responsabili dell'implementazione del confronto dei valori dei token di concorrenza.</span><span class="sxs-lookup"><span data-stu-id="51873-116">Database providers are responsible for implementing the comparison of concurrency token values.</span></span>

<span data-ttu-id="51873-117">Nei database relazionali EF Core include un controllo per il valore del token di concorrenza nella clausola `WHERE` di qualsiasi istruzione `UPDATE` o `DELETE`.</span><span class="sxs-lookup"><span data-stu-id="51873-117">On relational databases EF Core includes a check for the value of the concurrency token in the `WHERE` clause of any `UPDATE` or `DELETE` statements.</span></span> <span data-ttu-id="51873-118">Dopo avere eseguito le istruzioni, EF Core legge il numero di righe interessate.</span><span class="sxs-lookup"><span data-stu-id="51873-118">After executing the statements, EF Core reads the number of rows that were affected.</span></span>

<span data-ttu-id="51873-119">Se nessuna riga è interessata, viene rilevato un conflitto di concorrenza ed EF Core genera `DbUpdateConcurrencyException`.</span><span class="sxs-lookup"><span data-stu-id="51873-119">If no rows are affected, a concurrency conflict is detected, and EF Core throws `DbUpdateConcurrencyException`.</span></span>

<span data-ttu-id="51873-120">Ad esempio, si può decidere di configurare `LastName` per `Person` come token di concorrenza.</span><span class="sxs-lookup"><span data-stu-id="51873-120">For example, we may want to configure `LastName` on `Person` to be a concurrency token.</span></span> <span data-ttu-id="51873-121">Qualsiasi operazione di aggiornamento su Person includerà il controllo della concorrenza nella clausola `WHERE`:</span><span class="sxs-lookup"><span data-stu-id="51873-121">Then any update operation on Person will include the concurrency check in the `WHERE` clause:</span></span>

``` sql
UPDATE [Person] SET [FirstName] = @p1
WHERE [PersonId] = @p0 AND [LastName] = @p2;
```

## <a name="resolving-concurrency-conflicts"></a><span data-ttu-id="51873-122">Risoluzione dei conflitti di concorrenza</span><span class="sxs-lookup"><span data-stu-id="51873-122">Resolving concurrency conflicts</span></span>

<span data-ttu-id="51873-123">Continuando con l'esempio precedente, se un utente tenta di salvare alcune modifiche apportate a `Person`, ma un altro utente ha già cambiato `LastName` verrà generata un'eccezione.</span><span class="sxs-lookup"><span data-stu-id="51873-123">Continuing with the previous example, if one user tries to save some changes to a `Person`, but another user has already changed the `LastName`, then an exception will be thrown.</span></span>

<span data-ttu-id="51873-124">A questo punto, l'applicazione potrebbe semplicemente informare l'utente che l'aggiornamento non è riuscito a causa di modifiche in conflitto e passare oltre.</span><span class="sxs-lookup"><span data-stu-id="51873-124">At this point, the application could simply inform the user that the update was not successful due to conflicting changes and move on.</span></span> <span data-ttu-id="51873-125">Potrebbe però essere opportuno richiedere all'utente di verificare che il record rappresenti ancora la stessa persona effettiva e ripetere l'operazione.</span><span class="sxs-lookup"><span data-stu-id="51873-125">But it may be desirable to prompt the user to ensure this record still represents the same actual person and to retry the operation.</span></span>

<span data-ttu-id="51873-126">Questo processo è un esempio di _risoluzione di un conflitto di concorrenza_.</span><span class="sxs-lookup"><span data-stu-id="51873-126">This process is an example of _resolving a concurrency conflict_.</span></span>

<span data-ttu-id="51873-127">La risoluzione di un conflitto di concorrenza comporta l'unione delle modifiche in sospeso dal `DbContext` corrente con i valori nel database.</span><span class="sxs-lookup"><span data-stu-id="51873-127">Resolving a concurrency conflict involves merging the pending changes from the current `DbContext` with the values in the database.</span></span> <span data-ttu-id="51873-128">I valori che vengono uniti varieranno in base all'applicazione e potrebbero dipendere dall'input dell'utente.</span><span class="sxs-lookup"><span data-stu-id="51873-128">What values get merged will vary based on the application and may be directed by user input.</span></span>

<span data-ttu-id="51873-129">**Sono disponibili tre set di valori per risolvere un conflitto di concorrenza:**</span><span class="sxs-lookup"><span data-stu-id="51873-129">**There are three sets of values available to help resolve a concurrency conflict:**</span></span>

- <span data-ttu-id="51873-130">I **valori correnti** sono i valori che l'applicazione stava tentando di scrivere nel database.</span><span class="sxs-lookup"><span data-stu-id="51873-130">**Current values** are the values that the application was attempting to write to the database.</span></span>
- <span data-ttu-id="51873-131">I **valori originali** sono i valori che sono stati originariamente recuperati dal database, prima di apportare eventuali modifiche.</span><span class="sxs-lookup"><span data-stu-id="51873-131">**Original values** are the values that were originally retrieved from the database, before any edits were made.</span></span>
- <span data-ttu-id="51873-132">I **valori del database** sono i valori attualmente archiviati nel database.</span><span class="sxs-lookup"><span data-stu-id="51873-132">**Database values** are the values currently stored in the database.</span></span>

<span data-ttu-id="51873-133">L'approccio generale per gestire i conflitti di concorrenza è il seguente:</span><span class="sxs-lookup"><span data-stu-id="51873-133">The general approach to handle a concurrency conflicts is:</span></span>

1. <span data-ttu-id="51873-134">Intercettare `DbUpdateConcurrencyException` durante `SaveChanges`.</span><span class="sxs-lookup"><span data-stu-id="51873-134">Catch `DbUpdateConcurrencyException` during `SaveChanges`.</span></span>
2. <span data-ttu-id="51873-135">Usare `DbUpdateConcurrencyException.Entries` per preparare un nuovo set di modifiche per le entità interessate.</span><span class="sxs-lookup"><span data-stu-id="51873-135">Use `DbUpdateConcurrencyException.Entries` to prepare a new set of changes for the affected entities.</span></span>
3. <span data-ttu-id="51873-136">Aggiornare i valori originali del token di concorrenza in modo da riflettere i valori correnti nel database.</span><span class="sxs-lookup"><span data-stu-id="51873-136">Refresh the original values of the concurrency token to reflect the current values in the database.</span></span>
4. <span data-ttu-id="51873-137">Ripetere il processo fino a quando non si verificano conflitti.</span><span class="sxs-lookup"><span data-stu-id="51873-137">Retry the process until no conflicts occur.</span></span>

<span data-ttu-id="51873-138">Nell'esempio seguente `Person.FirstName` e `Person.LastName` sono configurati come token di concorrenza.</span><span class="sxs-lookup"><span data-stu-id="51873-138">In the following example, `Person.FirstName` and `Person.LastName` are set up as concurrency tokens.</span></span> <span data-ttu-id="51873-139">È presente un commento `// TODO:` nella posizione in cui viene inclusa la logica specifica dell'applicazione per scegliere il valore da salvare.</span><span class="sxs-lookup"><span data-stu-id="51873-139">There is a `// TODO:` comment in the location where you include application specific logic to choose the value to be saved.</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Concurrency/Sample.cs?name=ConcurrencyHandlingCode&highlight=34-35)]
