---
title: Token di concorrenza - Core a Entity Framework
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: bc8b1cb0-befe-4b67-8004-26e6c5f69385
ms.technology: entity-framework-core
uid: core/modeling/concurrency
ms.openlocfilehash: 6574a9098d38c4aa525ffb4896adb01082420b5f
ms.sourcegitcommit: 860ec5d047342fbc4063a0de881c9861cc1f8813
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/05/2017
---
# <a name="concurrency-tokens"></a><span data-ttu-id="2da14-102">Token di concorrenza</span><span class="sxs-lookup"><span data-stu-id="2da14-102">Concurrency Tokens</span></span>

<span data-ttu-id="2da14-103">Se una proprietà è configurata come un token di concorrenza EF verificherà che nessun altro utente ha modificato il valore nel database durante il salvataggio delle modifiche al record specifico.</span><span class="sxs-lookup"><span data-stu-id="2da14-103">If a property is configured as a concurrency token then EF will check that no other user has modified that value in the database when saving changes to that record.</span></span> <span data-ttu-id="2da14-104">Entity Framework Usa un modello di concorrenza ottimistica, ovvero verrà si supponga che il valore non è stato modificato e tenta di salvare i dati, ma genera se rileva che è stato modificato il valore.</span><span class="sxs-lookup"><span data-stu-id="2da14-104">EF uses an optimistic concurrency pattern, meaning it will assume the value has not changed and try to save the data, but throw if it finds the value has been changed.</span></span>

<span data-ttu-id="2da14-105">Ad esempio potrebbe essere necessario configurare `LastName` su `Person` da un token di concorrenza.</span><span class="sxs-lookup"><span data-stu-id="2da14-105">For example we may want to configure `LastName` on `Person` to be a concurrency token.</span></span> <span data-ttu-id="2da14-106">Ciò significa che se un utente tenta di salvare alcune modifiche apportate a un `Person`, ma un altro utente ha modificato il `LastName` quindi verrà generata un'eccezione.</span><span class="sxs-lookup"><span data-stu-id="2da14-106">This means that if one user tries to save some changes to a `Person`, but another user has changed the `LastName` then an exception will be thrown.</span></span> <span data-ttu-id="2da14-107">Potrebbe essere auspicabile in modo che l'applicazione può richiedere all'utente di verificare che il record rappresenta comunque la stessa persona prima di salvare le modifiche.</span><span class="sxs-lookup"><span data-stu-id="2da14-107">This may be desirable so that your application can prompt the user to ensure this record still represents the same actual person before saving their changes.</span></span>

> [!NOTE]
> <span data-ttu-id="2da14-108">Questa pagina viene illustrato come configurare i token di concorrenza.</span><span class="sxs-lookup"><span data-stu-id="2da14-108">This page documents how to configure concurrency tokens.</span></span> <span data-ttu-id="2da14-109">Vedere [la gestione della concorrenza](../saving/concurrency.md) per esempi di come usare concorrenza ottimistica nell'applicazione.</span><span class="sxs-lookup"><span data-stu-id="2da14-109">See [Handling Concurrency](../saving/concurrency.md) for examples of how to use optimistic concurrency in your application.</span></span>

## <a name="how-concurrency-tokens-work-in-ef"></a><span data-ttu-id="2da14-110">Modalità di funzionamento di token di concorrenza in Entity Framework</span><span class="sxs-lookup"><span data-stu-id="2da14-110">How concurrency tokens work in EF</span></span>

<span data-ttu-id="2da14-111">Archivi dati è possono applicare i token di concorrenza verificando che tutti i record da aggiornare o eliminare ancora ha lo stesso valore per il token di concorrenza assegnato al momento il contesto di caricamento iniziale dei dati dal database.</span><span class="sxs-lookup"><span data-stu-id="2da14-111">Data stores can enforce concurrency tokens by checking that any record being updated or deleted still has the same value for the concurrency token that was assigned when the context originally loaded the data from the database.</span></span>

<span data-ttu-id="2da14-112">Ad esempio, database relazionali di ottenere questo risultato, includendo il token di concorrenza nel `WHERE` clausola di qualsiasi `UPDATE` o `DELETE` comandi e il numero di righe che sono stati interessati.</span><span class="sxs-lookup"><span data-stu-id="2da14-112">For example, relational databases achieve this by including the concurrency token in the `WHERE` clause of any `UPDATE` or `DELETE` commands and checking the number of rows that were affected.</span></span> <span data-ttu-id="2da14-113">Se il token di concorrenza corrisponde ancora verrà aggiornata una riga.</span><span class="sxs-lookup"><span data-stu-id="2da14-113">If the concurrency token still matches then one row will be updated.</span></span> <span data-ttu-id="2da14-114">Se il valore nel database è stato modificato, non le righe vengono aggiornate.</span><span class="sxs-lookup"><span data-stu-id="2da14-114">If the value in the database has changed, then no rows are updated.</span></span>

```sql
UPDATE [Person] SET [FirstName] = @p1
WHERE [PersonId] = @p0 AND [LastName] = @p2;
```

## <a name="conventions"></a><span data-ttu-id="2da14-115">Convenzioni</span><span class="sxs-lookup"><span data-stu-id="2da14-115">Conventions</span></span>

<span data-ttu-id="2da14-116">Per convenzione, le proprietà non sono configurate come token di concorrenza.</span><span class="sxs-lookup"><span data-stu-id="2da14-116">By convention, properties are never configured as concurrency tokens.</span></span>

## <a name="data-annotations"></a><span data-ttu-id="2da14-117">Annotazioni dei dati</span><span class="sxs-lookup"><span data-stu-id="2da14-117">Data Annotations</span></span>

<span data-ttu-id="2da14-118">Per configurare una proprietà come un token di concorrenza, è possibile utilizzare le annotazioni dei dati.</span><span class="sxs-lookup"><span data-stu-id="2da14-118">You can use the Data Annotations to configure a property as a concurrency token.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/Samples/Concurrency.cs#ConfigureConcurrencyAnnotations)]

## <a name="fluent-api"></a><span data-ttu-id="2da14-119">Microsoft Office Fluent API</span><span class="sxs-lookup"><span data-stu-id="2da14-119">Fluent API</span></span>

<span data-ttu-id="2da14-120">Per configurare una proprietà come un token di concorrenza, è possibile utilizzare l'API Fluent.</span><span class="sxs-lookup"><span data-stu-id="2da14-120">You can use the Fluent API to configure a property as a concurrency token.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Samples/Concurrency.cs#ConfigureConcurrencyFluent)]

## <a name="timestamprow-version"></a><span data-ttu-id="2da14-121">Versione di timestamp o la riga</span><span class="sxs-lookup"><span data-stu-id="2da14-121">Timestamp/row version</span></span>

<span data-ttu-id="2da14-122">Un timestamp è una proprietà in cui viene generato un nuovo valore dal database ogni volta che una riga viene inserita o aggiornata.</span><span class="sxs-lookup"><span data-stu-id="2da14-122">A timestamp is a property where a new value is generated by the database every time a row is inserted or updated.</span></span> <span data-ttu-id="2da14-123">La proprietà viene anche considerata come un token di concorrenza.</span><span class="sxs-lookup"><span data-stu-id="2da14-123">The property is also treated as a concurrency token.</span></span> <span data-ttu-id="2da14-124">In questo modo che si otterrà un'eccezione se qualsiasi altro utente ha modificato una riga che si sta tentando di aggiornare poiché si esegue una query per i dati.</span><span class="sxs-lookup"><span data-stu-id="2da14-124">This ensures you will get an exception if anyone else has modified a row that you are trying to update since you queried for the data.</span></span>

<span data-ttu-id="2da14-125">La procedura è compito del provider di database usato.</span><span class="sxs-lookup"><span data-stu-id="2da14-125">How this is achieved is up to the database provider being used.</span></span> <span data-ttu-id="2da14-126">Per SQL Server, i timestamp in genere è utilizzato in un *byte []* programma di installazione come proprietà, che sarà un *ROWVERSION* colonna nel database.</span><span class="sxs-lookup"><span data-stu-id="2da14-126">For SQL Server, timestamp is usually used on a *byte[]* property, which will be setup as a *ROWVERSION* column in the database.</span></span>

### <a name="conventions"></a><span data-ttu-id="2da14-127">Convenzioni</span><span class="sxs-lookup"><span data-stu-id="2da14-127">Conventions</span></span>

<span data-ttu-id="2da14-128">Per convenzione, le proprietà non sono configurate come timestamp.</span><span class="sxs-lookup"><span data-stu-id="2da14-128">By convention, properties are never configured as timestamps.</span></span>

### <a name="data-annotations"></a><span data-ttu-id="2da14-129">Annotazioni dei dati</span><span class="sxs-lookup"><span data-stu-id="2da14-129">Data Annotations</span></span>

<span data-ttu-id="2da14-130">Per configurare una proprietà come un timestamp, è possibile utilizzare le annotazioni dei dati.</span><span class="sxs-lookup"><span data-stu-id="2da14-130">You can use Data Annotations to configure a property as a timestamp.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/Samples/Timestamp.cs#ConfigureTimestampAnnotations)]

### <a name="fluent-api"></a><span data-ttu-id="2da14-131">Microsoft Office Fluent API</span><span class="sxs-lookup"><span data-stu-id="2da14-131">Fluent API</span></span>

<span data-ttu-id="2da14-132">Per configurare una proprietà come un timestamp, è possibile utilizzare l'API Fluent.</span><span class="sxs-lookup"><span data-stu-id="2da14-132">You can use the Fluent API to configure a property as a timestamp.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Samples/Timestamp.cs#ConfigureTimestampFluent)]
