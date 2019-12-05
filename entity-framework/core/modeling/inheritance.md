---
title: Ereditarietà-EF Core
description: Come configurare l'ereditarietà del tipo di entità usando Entity Framework Core
author: AndriySvyryd
ms.author: ansvyryd
ms.date: 10/27/2016
uid: core/modeling/inheritance
ms.openlocfilehash: 4d43a432174c92ab7f3f9d78a234aefb0a4a17e8
ms.sourcegitcommit: 7a709ce4f77134782393aa802df5ab2718714479
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 12/04/2019
ms.locfileid: "74824672"
---
# <a name="inheritance"></a><span data-ttu-id="ade7c-103">Ereditarietà</span><span class="sxs-lookup"><span data-stu-id="ade7c-103">Inheritance</span></span>

<span data-ttu-id="ade7c-104">L'ereditarietà nel modello EF viene utilizzata per controllare il modo in cui l'ereditarietà nelle classi di entità viene rappresentata nel database.</span><span class="sxs-lookup"><span data-stu-id="ade7c-104">Inheritance in the EF model is used to control how inheritance in the entity classes is represented in the database.</span></span>

## <a name="conventions"></a><span data-ttu-id="ade7c-105">Convenzioni</span><span class="sxs-lookup"><span data-stu-id="ade7c-105">Conventions</span></span>

<span data-ttu-id="ade7c-106">Per impostazione predefinita, spetta al provider di database determinare il modo in cui l'ereditarietà verrà rappresentata nel database.</span><span class="sxs-lookup"><span data-stu-id="ade7c-106">By default, it is up to the database provider to determine how inheritance will be represented in the database.</span></span> <span data-ttu-id="ade7c-107">Vedere [ereditarietà (database relazionale)](relational/inheritance.md) per il modo in cui viene gestita con un provider di database relazionale.</span><span class="sxs-lookup"><span data-stu-id="ade7c-107">See [Inheritance (Relational Database)](relational/inheritance.md) for how this is handled with a relational database provider.</span></span>

<span data-ttu-id="ade7c-108">EF imposterà l'ereditarietà solo se due o più tipi ereditati vengono inclusi in modo esplicito nel modello.</span><span class="sxs-lookup"><span data-stu-id="ade7c-108">EF will only setup inheritance if two or more inherited types are explicitly included in the model.</span></span> <span data-ttu-id="ade7c-109">EF non analizza i tipi di base o derivati che non sono stati altrimenti inclusi nel modello.</span><span class="sxs-lookup"><span data-stu-id="ade7c-109">EF will not scan for base or derived types that were not otherwise included in the model.</span></span> <span data-ttu-id="ade7c-110">È possibile includere tipi nel modello esponendo un *DbSet\<tentity >* per ogni tipo nella gerarchia di ereditarietà.</span><span class="sxs-lookup"><span data-stu-id="ade7c-110">You can include types in the model by exposing a *DbSet\<TEntity>* for each type in the inheritance hierarchy.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/Conventions/InheritanceDbSets.cs?highlight=3-4&name=Model)]

<span data-ttu-id="ade7c-111">Se non si vuole esporre un *DbSet\<tentity >* per una o più entità nella gerarchia, è possibile usare l'API Fluent per assicurarsi che siano incluse nel modello.</span><span class="sxs-lookup"><span data-stu-id="ade7c-111">If you don't want to expose a *DbSet\<TEntity>* for one or more entities in the hierarchy, you can use the Fluent API to ensure they are included in the model.</span></span>
<span data-ttu-id="ade7c-112">Se non si basano sulle convenzioni, è possibile specificare il tipo di base in modo esplicito utilizzando `HasBaseType`.</span><span class="sxs-lookup"><span data-stu-id="ade7c-112">And if you don't rely on conventions, you can specify the base type explicitly using `HasBaseType`.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/Conventions/InheritanceModelBuilder.cs?highlight=7&name=Context)]

> [!NOTE]
> <span data-ttu-id="ade7c-113">È possibile utilizzare `.HasBaseType((Type)null)` per rimuovere un tipo di entità dalla gerarchia.</span><span class="sxs-lookup"><span data-stu-id="ade7c-113">You can use `.HasBaseType((Type)null)` to remove an entity type from the hierarchy.</span></span>

## <a name="data-annotations"></a><span data-ttu-id="ade7c-114">Annotazioni dei dati</span><span class="sxs-lookup"><span data-stu-id="ade7c-114">Data Annotations</span></span>

<span data-ttu-id="ade7c-115">Non è possibile utilizzare le annotazioni dei dati per configurare l'ereditarietà.</span><span class="sxs-lookup"><span data-stu-id="ade7c-115">You cannot use Data Annotations to configure inheritance.</span></span>

## <a name="fluent-api"></a><span data-ttu-id="ade7c-116">API Fluent</span><span class="sxs-lookup"><span data-stu-id="ade7c-116">Fluent API</span></span>

<span data-ttu-id="ade7c-117">L'API Fluent per l'ereditarietà dipende dal provider di database in uso.</span><span class="sxs-lookup"><span data-stu-id="ade7c-117">The Fluent API for inheritance depends on the database provider you are using.</span></span> <span data-ttu-id="ade7c-118">Vedere [ereditarietà (database relazionale)](relational/inheritance.md) per la configurazione che è possibile eseguire per un provider di database relazionale.</span><span class="sxs-lookup"><span data-stu-id="ade7c-118">See [Inheritance (Relational Database)](relational/inheritance.md) for the configuration you can perform for a relational database provider.</span></span>
