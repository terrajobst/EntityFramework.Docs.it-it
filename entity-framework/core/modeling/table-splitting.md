---
title: Suddivisione di tabelle-EF Core
description: Come configurare la suddivisione delle tabelle con Entity Framework Core
author: AndriySvyryd
ms.author: ansvyryd
ms.date: 04/10/2019
uid: core/modeling/table-splitting
ms.openlocfilehash: 0e48c516de43cdc2b54c56f1a96f5e01f9fbbbc4
ms.sourcegitcommit: 7a709ce4f77134782393aa802df5ab2718714479
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 12/04/2019
ms.locfileid: "74824565"
---
# <a name="table-splitting"></a><span data-ttu-id="7c437-103">Suddivisione di tabelle</span><span class="sxs-lookup"><span data-stu-id="7c437-103">Table Splitting</span></span>

>[!NOTE]
> <span data-ttu-id="7c437-104">Questa funzionalità è una novità di EF Core 2,0.</span><span class="sxs-lookup"><span data-stu-id="7c437-104">This feature is new in EF Core 2.0.</span></span>

<span data-ttu-id="7c437-105">EF Core consente di eseguire il mapping di due o più entità a una singola riga.</span><span class="sxs-lookup"><span data-stu-id="7c437-105">EF Core allows to map two or more entities to a single row.</span></span> <span data-ttu-id="7c437-106">Questa operazione è denominata _suddivisione tabelle_ o _condivisione tabella_.</span><span class="sxs-lookup"><span data-stu-id="7c437-106">This is called _table splitting_ or _table sharing_.</span></span>

## <a name="configuration"></a><span data-ttu-id="7c437-107">Configurazione di</span><span class="sxs-lookup"><span data-stu-id="7c437-107">Configuration</span></span>

<span data-ttu-id="7c437-108">Per utilizzare la suddivisione della tabella è necessario eseguire il mapping dei tipi di entità alla stessa tabella, disporre delle chiavi primarie mappate alle stesse colonne e almeno una relazione configurata tra la chiave primaria di un tipo di entità e un'altra nella stessa tabella.</span><span class="sxs-lookup"><span data-stu-id="7c437-108">To use table splitting the entity types need to be mapped to the same table, have the primary keys mapped to the same columns and at least one relationship configured between the primary key of one entity type and another in the same table.</span></span>

<span data-ttu-id="7c437-109">Uno scenario comune per la suddivisione delle tabelle consiste nell'utilizzare solo un subset delle colonne della tabella per ottenere un livello superiore di prestazioni o incapsulamento.</span><span class="sxs-lookup"><span data-stu-id="7c437-109">A common scenario for table splitting is using only a subset of the columns in the table for greater performance or encapsulation.</span></span>

<span data-ttu-id="7c437-110">In questo esempio `Order` rappresenta un subset di `DetailedOrder`.</span><span class="sxs-lookup"><span data-stu-id="7c437-110">In this example `Order` represents a subset of `DetailedOrder`.</span></span>

[!code-csharp[Order](../../../samples/core/Modeling/TableSplitting/Order.cs?name=Order)]

[!code-csharp[DetailedOrder](../../../samples/core/Modeling/TableSplitting/DetailedOrder.cs?name=DetailedOrder)]

<span data-ttu-id="7c437-111">Oltre alla configurazione richiesta, viene chiamato `Property(o => o.Status).HasColumnName("Status")` per eseguire il mapping `DetailedOrder.Status` alla stessa colonna `Order.Status`.</span><span class="sxs-lookup"><span data-stu-id="7c437-111">In addition to the required configuration we call `Property(o => o.Status).HasColumnName("Status")` to map `DetailedOrder.Status` to the same column as `Order.Status`.</span></span>

[!code-csharp[TableSplittingConfiguration](../../../samples/core/Modeling/TableSplitting/TableSplittingContext.cs?name=TableSplitting&highlight=3)]

> [!TIP]
> <span data-ttu-id="7c437-112">Per ulteriori informazioni sul contesto, vedere il [progetto di esempio completo](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Modeling/TableSplitting) .</span><span class="sxs-lookup"><span data-stu-id="7c437-112">See the [full sample project](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Modeling/TableSplitting) for more context.</span></span>

## <a name="usage"></a><span data-ttu-id="7c437-113">Usage</span><span class="sxs-lookup"><span data-stu-id="7c437-113">Usage</span></span>

<span data-ttu-id="7c437-114">Il salvataggio e l'esecuzione di query sulle entità tramite la suddivisione delle tabelle vengono eseguite in modo analogo alle altre entità.</span><span class="sxs-lookup"><span data-stu-id="7c437-114">Saving and querying entities using table splitting is done in the same way as other entities.</span></span> <span data-ttu-id="7c437-115">A partire da EF Core 3,0 è possibile `null`il riferimento all'entità dipendente.</span><span class="sxs-lookup"><span data-stu-id="7c437-115">And starting with EF Core 3.0 the dependent entity reference can be `null`.</span></span> <span data-ttu-id="7c437-116">Se tutte le colonne utilizzate dall'entità dipendente sono `NULL` è il database, non verrà creata alcuna istanza per la query.</span><span class="sxs-lookup"><span data-stu-id="7c437-116">If all of the columns used by the dependent entity are `NULL` is the database then no instance for it will be created when queried.</span></span> <span data-ttu-id="7c437-117">Questa situazione si verifica anche quando tutte le proprietà sono facoltative e impostate su `null`, che potrebbe non essere previsto.</span><span class="sxs-lookup"><span data-stu-id="7c437-117">This would also happen all of the properties are optional and set to `null`, which might not be expected.</span></span>

[!code-csharp[Usage](../../../samples/core/Modeling/TableSplitting/Program.cs?name=Usage)]

## <a name="concurrency-tokens"></a><span data-ttu-id="7c437-118">Token di concorrenza</span><span class="sxs-lookup"><span data-stu-id="7c437-118">Concurrency tokens</span></span>

<span data-ttu-id="7c437-119">Se uno dei tipi di entità che condividono una tabella include un token di concorrenza, è necessario che sia incluso in tutti gli altri tipi di entità per evitare un valore del token di concorrenza non aggiornato quando viene aggiornata solo una delle entità mappate alla stessa tabella.</span><span class="sxs-lookup"><span data-stu-id="7c437-119">If any of the entity types sharing a table has a concurrency token then it must be included in all other entity types to avoid a stale concurrency token value when only one of the entities mapped to the same table is updated.</span></span>

<span data-ttu-id="7c437-120">Per evitare di esporlo al codice consumer, è possibile crearne uno in stato di ombreggiatura.</span><span class="sxs-lookup"><span data-stu-id="7c437-120">To avoid exposing it to the consuming code it's possible the create one in shadow-state.</span></span>

[!code-csharp[TableSplittingConfiguration](../../../samples/core/Modeling/TableSplitting/TableSplittingContext.cs?name=ConcurrencyToken&highlight=2)]
