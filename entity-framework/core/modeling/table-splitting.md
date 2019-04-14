---
title: Table Splitting - EF Core
author: AndriySvyryd
ms.author: ansvyryd
ms.date: 04/10/2019
ms.assetid: 0EC2CCE1-BD55-45D8-9EA9-20634987F094
uid: core/modeling/table-splitting
ms.openlocfilehash: 4a0bfaf017106a0bfdff084b1c472bdc17459a89
ms.sourcegitcommit: 8f801993c9b8cd8a8fbfa7134818a8edca79e31a
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/14/2019
ms.locfileid: "59562587"
---
# <a name="table-splitting"></a><span data-ttu-id="967f9-102">Suddivisione di tabelle</span><span class="sxs-lookup"><span data-stu-id="967f9-102">Table Splitting</span></span>

>[!NOTE]
> <span data-ttu-id="967f9-103">Questa funzionalità è una novità di EF Core 2.0.</span><span class="sxs-lookup"><span data-stu-id="967f9-103">This feature is new in EF Core 2.0.</span></span>

<span data-ttu-id="967f9-104">EF Core consente di eseguire il mapping di due o più entità a una singola riga.</span><span class="sxs-lookup"><span data-stu-id="967f9-104">EF Core allows to map two or more entities to a single row.</span></span> <span data-ttu-id="967f9-105">Questa operazione viene definita _tabella suddivisione_ oppure _tabella condivisione_.</span><span class="sxs-lookup"><span data-stu-id="967f9-105">This is called _table splitting_ or _table sharing_.</span></span>

## <a name="configuration"></a><span data-ttu-id="967f9-106">Configurazione</span><span class="sxs-lookup"><span data-stu-id="967f9-106">Configuration</span></span>

<span data-ttu-id="967f9-107">Per usare la suddivisione di tabelle, che i tipi di entità devono essere mappati alla stessa tabella, sono le chiavi primarie mappate alle stesse colonne e almeno una relazione tra chiave primaria di un tipo di entità e un'altra nella stessa tabella configurata.</span><span class="sxs-lookup"><span data-stu-id="967f9-107">To use table splitting the entity types need to be mapped to the same table, have the primary keys mapped to the same columns and at least one relationship configured between the primary key of one entity type and another in the same table.</span></span>

<span data-ttu-id="967f9-108">Uno scenario comune per la suddivisione di tabelle utilizza solo un subset delle colonne nella tabella per maggiore sulle prestazioni o l'incapsulamento.</span><span class="sxs-lookup"><span data-stu-id="967f9-108">A common scenario for table splitting is using only a subset of the columns in the table for greater performance or encapsulation.</span></span>

<span data-ttu-id="967f9-109">In questo esempio `Order` rappresenta un subset di `DetailedOrder`.</span><span class="sxs-lookup"><span data-stu-id="967f9-109">In this example `Order` represents a subset of `DetailedOrder`.</span></span>

[!code-csharp[Order](../../../samples/core/Modeling/TableSplitting/Order.cs?name=Order)]

[!code-csharp[DetailedOrder](../../../samples/core/Modeling/TableSplitting/DetailedOrder.cs?name=DetailedOrder)]

<span data-ttu-id="967f9-110">Oltre alla configurazione necessaria chiamiamo `HasBaseType((string)null)` per evitare di mapping `DetailedOrder` nella stessa gerarchia `Order`.</span><span class="sxs-lookup"><span data-stu-id="967f9-110">In addition to the required configuration we call `HasBaseType((string)null)` to avoid mapping `DetailedOrder` in the same hierarchy as `Order`.</span></span>

[!code-csharp[TableSplittingConfiguration](../../../samples/core/Modeling/TableSplitting/TableSplittingContext.cs?name=TableSplitting&highlight=3)]

<span data-ttu-id="967f9-111">Vedere le [progetto di esempio completo](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Modeling/TableSplitting) per altre informazioni sul contesto.</span><span class="sxs-lookup"><span data-stu-id="967f9-111">See the [full sample project](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Modeling/TableSplitting) for more context.</span></span>

## <a name="usage"></a><span data-ttu-id="967f9-112">Utilizzo</span><span class="sxs-lookup"><span data-stu-id="967f9-112">Usage</span></span>

<span data-ttu-id="967f9-113">Salvataggio e l'esecuzione di query su entità tramite la suddivisione di tabelle viene eseguita nello stesso modo di altre entità, l'unica differenza è che è necessario tenere traccia di tutte le entità la condivisione di una riga per l'inserimento.</span><span class="sxs-lookup"><span data-stu-id="967f9-113">Saving and querying entities using table splitting is done in the same way as other entities, the only difference is that all entities sharing a row must be tracked for the insert.</span></span>

[!code-csharp[Usage](../../../samples/core/Modeling/TableSplitting/Program.cs?name=Usage)]

## <a name="concurrency-tokens"></a><span data-ttu-id="967f9-114">Token di concorrenza</span><span class="sxs-lookup"><span data-stu-id="967f9-114">Concurrency tokens</span></span>

<span data-ttu-id="967f9-115">Se uno dei tipi di entità la condivisione di una tabella contiene un token di concorrenza quindi deve essere incluso in tutti gli altri tipi di entità per evitare un valore del token di concorrenza non aggiornato quando viene aggiornata solo una delle entità mappata alla stessa tabella.</span><span class="sxs-lookup"><span data-stu-id="967f9-115">If any of the entity types sharing a table has a concurrency token then it must be included in all other entity types to avoid a stale concurrency token value when only one of the entities mapped to the same table is updated.</span></span>

<span data-ttu-id="967f9-116">Per evitare di esporli al codice consumer è possibile crearne uno in stato di ombreggiatura.</span><span class="sxs-lookup"><span data-stu-id="967f9-116">To avoid exposing it to the consuming code it's possible the create one in shadow-state.</span></span>

[!code-csharp[TableSplittingConfiguration](../../../samples/core/Modeling/TableSplitting/TableSplittingContext.cs?name=ConcurrencyToken&highlight=2)]