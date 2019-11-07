---
title: Ereditarietà-EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 754be334-dd21-450e-9d22-2591e80012a2
uid: core/modeling/inheritance
ms.openlocfilehash: abc1caa4d3839b7cdb52b316bcfc8f648b609b70
ms.sourcegitcommit: 18ab4c349473d94b15b4ca977df12147db07b77f
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/06/2019
ms.locfileid: "73655680"
---
# <a name="inheritance"></a><span data-ttu-id="b40ab-102">Ereditarietà</span><span class="sxs-lookup"><span data-stu-id="b40ab-102">Inheritance</span></span>

<span data-ttu-id="b40ab-103">L'ereditarietà nel modello EF viene utilizzata per controllare il modo in cui l'ereditarietà nelle classi di entità viene rappresentata nel database.</span><span class="sxs-lookup"><span data-stu-id="b40ab-103">Inheritance in the EF model is used to control how inheritance in the entity classes is represented in the database.</span></span>

## <a name="conventions"></a><span data-ttu-id="b40ab-104">Convenzioni</span><span class="sxs-lookup"><span data-stu-id="b40ab-104">Conventions</span></span>

<span data-ttu-id="b40ab-105">Per convenzione, spetta al provider di database determinare il modo in cui l'ereditarietà verrà rappresentata nel database.</span><span class="sxs-lookup"><span data-stu-id="b40ab-105">By convention, it is up to the database provider to determine how inheritance will be represented in the database.</span></span> <span data-ttu-id="b40ab-106">Vedere [ereditarietà (database relazionale)](relational/inheritance.md) per il modo in cui viene gestita con un provider di database relazionale.</span><span class="sxs-lookup"><span data-stu-id="b40ab-106">See [Inheritance (Relational Database)](relational/inheritance.md) for how this is handled with a relational database provider.</span></span>

<span data-ttu-id="b40ab-107">EF imposterà l'ereditarietà solo se due o più tipi ereditati vengono inclusi in modo esplicito nel modello.</span><span class="sxs-lookup"><span data-stu-id="b40ab-107">EF will only setup inheritance if two or more inherited types are explicitly included in the model.</span></span> <span data-ttu-id="b40ab-108">EF non analizza i tipi di base o derivati che non sono stati altrimenti inclusi nel modello.</span><span class="sxs-lookup"><span data-stu-id="b40ab-108">EF will not scan for base or derived types that were not otherwise included in the model.</span></span> <span data-ttu-id="b40ab-109">È possibile includere tipi nel modello esponendo un *DbSet\<tentity >* per ogni tipo nella gerarchia di ereditarietà.</span><span class="sxs-lookup"><span data-stu-id="b40ab-109">You can include types in the model by exposing a *DbSet\<TEntity>* for each type in the inheritance hierarchy.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/Conventions/InheritanceDbSets.cs?highlight=3-4&name=Model)]

<span data-ttu-id="b40ab-110">Se non si vuole esporre un *DbSet\<tentity >* per una o più entità nella gerarchia, è possibile usare l'API Fluent per assicurarsi che siano incluse nel modello.</span><span class="sxs-lookup"><span data-stu-id="b40ab-110">If you don't want to expose a *DbSet\<TEntity>* for one or more entities in the hierarchy, you can use the Fluent API to ensure they are included in the model.</span></span>
<span data-ttu-id="b40ab-111">Se non si basano sulle convenzioni, è possibile specificare il tipo di base in modo esplicito utilizzando `HasBaseType`.</span><span class="sxs-lookup"><span data-stu-id="b40ab-111">And if you don't rely on conventions, you can specify the base type explicitly using `HasBaseType`.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/Conventions/InheritanceModelBuilder.cs?highlight=7&name=Context)]

> [!NOTE]
> <span data-ttu-id="b40ab-112">È possibile utilizzare `.HasBaseType((Type)null)` per rimuovere un tipo di entità dalla gerarchia.</span><span class="sxs-lookup"><span data-stu-id="b40ab-112">You can use `.HasBaseType((Type)null)` to remove an entity type from the hierarchy.</span></span>

## <a name="data-annotations"></a><span data-ttu-id="b40ab-113">Annotazioni dei dati</span><span class="sxs-lookup"><span data-stu-id="b40ab-113">Data Annotations</span></span>

<span data-ttu-id="b40ab-114">Non è possibile utilizzare le annotazioni dei dati per configurare l'ereditarietà.</span><span class="sxs-lookup"><span data-stu-id="b40ab-114">You cannot use Data Annotations to configure inheritance.</span></span>

## <a name="fluent-api"></a><span data-ttu-id="b40ab-115">API Fluent</span><span class="sxs-lookup"><span data-stu-id="b40ab-115">Fluent API</span></span>

<span data-ttu-id="b40ab-116">L'API Fluent per l'ereditarietà dipende dal provider di database in uso.</span><span class="sxs-lookup"><span data-stu-id="b40ab-116">The Fluent API for inheritance depends on the database provider you are using.</span></span> <span data-ttu-id="b40ab-117">Vedere [ereditarietà (database relazionale)](relational/inheritance.md) per la configurazione che è possibile eseguire per un provider di database relazionale.</span><span class="sxs-lookup"><span data-stu-id="b40ab-117">See [Inheritance (Relational Database)](relational/inheritance.md) for the configuration you can perform for a relational database provider.</span></span>
