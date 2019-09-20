---
title: Suddivisione di tabelle-EF Core
author: AndriySvyryd
ms.author: ansvyryd
ms.date: 04/10/2019
ms.assetid: 0EC2CCE1-BD55-45D8-9EA9-20634987F094
uid: core/modeling/table-splitting
ms.openlocfilehash: 684fcfbb66debfd1b89e23c8aaf0a32909378c6b
ms.sourcegitcommit: cbaa6cc89bd71d5e0bcc891e55743f0e8ea3393b
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/20/2019
ms.locfileid: "71149193"
---
# <a name="table-splitting"></a><span data-ttu-id="ed48a-102">Suddivisione di tabelle</span><span class="sxs-lookup"><span data-stu-id="ed48a-102">Table Splitting</span></span>

>[!NOTE]
> <span data-ttu-id="ed48a-103">Questa funzionalità è una novità di EF Core 2,0.</span><span class="sxs-lookup"><span data-stu-id="ed48a-103">This feature is new in EF Core 2.0.</span></span>

<span data-ttu-id="ed48a-104">EF Core consente di eseguire il mapping di due o più entità a una singola riga.</span><span class="sxs-lookup"><span data-stu-id="ed48a-104">EF Core allows to map two or more entities to a single row.</span></span> <span data-ttu-id="ed48a-105">Questa operazione è denominata _suddivisione tabelle_ o _condivisione tabella_.</span><span class="sxs-lookup"><span data-stu-id="ed48a-105">This is called _table splitting_ or _table sharing_.</span></span>

## <a name="configuration"></a><span data-ttu-id="ed48a-106">Configurazione</span><span class="sxs-lookup"><span data-stu-id="ed48a-106">Configuration</span></span>

<span data-ttu-id="ed48a-107">Per utilizzare la suddivisione della tabella è necessario eseguire il mapping dei tipi di entità alla stessa tabella, disporre delle chiavi primarie mappate alle stesse colonne e almeno una relazione configurata tra la chiave primaria di un tipo di entità e un'altra nella stessa tabella.</span><span class="sxs-lookup"><span data-stu-id="ed48a-107">To use table splitting the entity types need to be mapped to the same table, have the primary keys mapped to the same columns and at least one relationship configured between the primary key of one entity type and another in the same table.</span></span>

<span data-ttu-id="ed48a-108">Uno scenario comune per la suddivisione delle tabelle consiste nell'utilizzare solo un subset delle colonne della tabella per ottenere un livello superiore di prestazioni o incapsulamento.</span><span class="sxs-lookup"><span data-stu-id="ed48a-108">A common scenario for table splitting is using only a subset of the columns in the table for greater performance or encapsulation.</span></span>

<span data-ttu-id="ed48a-109">In questo esempio `Order` rappresenta un subset di `DetailedOrder`.</span><span class="sxs-lookup"><span data-stu-id="ed48a-109">In this example `Order` represents a subset of `DetailedOrder`.</span></span>

[!code-csharp[Order](../../../samples/core/Modeling/TableSplitting/Order.cs?name=Order)]

[!code-csharp[DetailedOrder](../../../samples/core/Modeling/TableSplitting/DetailedOrder.cs?name=DetailedOrder)]

<span data-ttu-id="ed48a-110">Oltre alla configurazione richiesta, `Property(o => o.Status).HasColumnName("Status")` `Order.Status`viene chiamato per eseguire il `DetailedOrder.Status` mapping alla stessa colonna di.</span><span class="sxs-lookup"><span data-stu-id="ed48a-110">In addition to the required configuration we call `Property(o => o.Status).HasColumnName("Status")` to map `DetailedOrder.Status` to the same column as `Order.Status`.</span></span>

[!code-csharp[TableSplittingConfiguration](../../../samples/core/Modeling/TableSplitting/TableSplittingContext.cs?name=TableSplitting&highlight=3)]

> [!TIP]
> <span data-ttu-id="ed48a-111">Per ulteriori informazioni sul contesto, vedere il [progetto di esempio completo](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Modeling/TableSplitting) .</span><span class="sxs-lookup"><span data-stu-id="ed48a-111">See the [full sample project](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Modeling/TableSplitting) for more context.</span></span>

## <a name="usage"></a><span data-ttu-id="ed48a-112">Utilizzo</span><span class="sxs-lookup"><span data-stu-id="ed48a-112">Usage</span></span>

<span data-ttu-id="ed48a-113">Il salvataggio e l'esecuzione di query sulle entità tramite la suddivisione delle tabelle vengono eseguite in modo analogo alle altre entità.</span><span class="sxs-lookup"><span data-stu-id="ed48a-113">Saving and querying entities using table splitting is done in the same way as other entities.</span></span> <span data-ttu-id="ed48a-114">A partire da EF Core 3,0, il riferimento all'entità dipendente `null`può essere.</span><span class="sxs-lookup"><span data-stu-id="ed48a-114">And starting with EF Core 3.0 the dependent entity reference can be `null`.</span></span> <span data-ttu-id="ed48a-115">Se tutte le colonne utilizzate dall'entità dipendente sono `NULL` rappresentate dal database, non verrà creata alcuna istanza per la query.</span><span class="sxs-lookup"><span data-stu-id="ed48a-115">If all of the columns used by the dependent entity are `NULL` is the database then no instance for it will be created when queried.</span></span> <span data-ttu-id="ed48a-116">Questa situazione si verifica anche quando tutte le proprietà sono facoltative e `null`impostate su, che potrebbe non essere previsto.</span><span class="sxs-lookup"><span data-stu-id="ed48a-116">This would also happen all of the properties are optional and set to `null`, which might not be expected.</span></span>

[!code-csharp[Usage](../../../samples/core/Modeling/TableSplitting/Program.cs?name=Usage)]

## <a name="concurrency-tokens"></a><span data-ttu-id="ed48a-117">Token di concorrenza</span><span class="sxs-lookup"><span data-stu-id="ed48a-117">Concurrency tokens</span></span>

<span data-ttu-id="ed48a-118">Se uno dei tipi di entità che condividono una tabella include un token di concorrenza, è necessario che sia incluso in tutti gli altri tipi di entità per evitare un valore del token di concorrenza non aggiornato quando viene aggiornata solo una delle entità mappate alla stessa tabella.</span><span class="sxs-lookup"><span data-stu-id="ed48a-118">If any of the entity types sharing a table has a concurrency token then it must be included in all other entity types to avoid a stale concurrency token value when only one of the entities mapped to the same table is updated.</span></span>

<span data-ttu-id="ed48a-119">Per evitare di esporlo al codice consumer, è possibile crearne uno in stato di ombreggiatura.</span><span class="sxs-lookup"><span data-stu-id="ed48a-119">To avoid exposing it to the consuming code it's possible the create one in shadow-state.</span></span>

[!code-csharp[TableSplittingConfiguration](../../../samples/core/Modeling/TableSplitting/TableSplittingContext.cs?name=ConcurrencyToken&highlight=2)]