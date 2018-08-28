---
title: Ereditarietà - EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 754be334-dd21-450e-9d22-2591e80012a2
uid: core/modeling/inheritance
ms.openlocfilehash: c5fa9d13dec8cfc3e1cac69e471f509cbbb9e4c5
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 08/27/2018
ms.locfileid: "42995896"
---
# <a name="inheritance"></a><span data-ttu-id="9edbc-102">Ereditarietà</span><span class="sxs-lookup"><span data-stu-id="9edbc-102">Inheritance</span></span>

<span data-ttu-id="9edbc-103">Ereditarietà nel modello di Entity Framework viene utilizzato per controllare la modalità di rappresentazione di ereditarietà nelle classi di entità nel database.</span><span class="sxs-lookup"><span data-stu-id="9edbc-103">Inheritance in the EF model is used to control how inheritance in the entity classes is represented in the database.</span></span>

## <a name="conventions"></a><span data-ttu-id="9edbc-104">Convenzioni</span><span class="sxs-lookup"><span data-stu-id="9edbc-104">Conventions</span></span>

<span data-ttu-id="9edbc-105">Per convenzione, spetta il provider di database per determinare come ereditarietà verrà rappresentata nel database.</span><span class="sxs-lookup"><span data-stu-id="9edbc-105">By convention, it is up to the database provider to determine how inheritance will be represented in the database.</span></span> <span data-ttu-id="9edbc-106">Visualizzare [ereditarietà (Database relazionale)](relational/inheritance.md) per come questa operazione viene gestita con un provider di database relazionale.</span><span class="sxs-lookup"><span data-stu-id="9edbc-106">See [Inheritance (Relational Database)](relational/inheritance.md) for how this is handled with a relational database provider.</span></span>

<span data-ttu-id="9edbc-107">EF installerà solo l'ereditarietà se due o più tipi ereditati sono inclusi in modo esplicito nel modello.</span><span class="sxs-lookup"><span data-stu-id="9edbc-107">EF will only setup inheritance if two or more inherited types are explicitly included in the model.</span></span> <span data-ttu-id="9edbc-108">EF non analizza per i tipi di base o derivati in caso contrario, non incluse nel modello.</span><span class="sxs-lookup"><span data-stu-id="9edbc-108">EF will not scan for base or derived types that were not otherwise included in the model.</span></span> <span data-ttu-id="9edbc-109">È possibile includere i tipi nel modello tramite l'esposizione di un *DbSet<TEntity>*  per ogni tipo nella gerarchia di ereditarietà.</span><span class="sxs-lookup"><span data-stu-id="9edbc-109">You can include types in the model by exposing a *DbSet<TEntity>* for each type in the inheritance hierarchy.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/Conventions/Samples/InheritanceDbSets.cs?highlight=3-4&name=Model)]

<span data-ttu-id="9edbc-110">Se non si vuole esporre una *DbSet<TEntity>*  per una o più entità nella gerarchia, è possibile usare l'API Fluent per assicurare che siano incluse nel modello.</span><span class="sxs-lookup"><span data-stu-id="9edbc-110">If you don't want to expose a *DbSet<TEntity>* for one or more entities in the hierarchy, you can use the Fluent API to ensure they are included in the model.</span></span>
<span data-ttu-id="9edbc-111">E se non si basano sulle convenzioni, è possibile specificare il tipo di base in modo esplicito usando `HasBaseType`.</span><span class="sxs-lookup"><span data-stu-id="9edbc-111">And if you don't rely on conventions you can specify the base type explicitly using `HasBaseType`.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/Conventions/Samples/InheritanceModelBuilder.cs?highlight=7&name=Context)]

> [!NOTE]
> <span data-ttu-id="9edbc-112">È possibile usare `.HasBaseType((Type)null)` per rimuovere un tipo di entità dalla gerarchia.</span><span class="sxs-lookup"><span data-stu-id="9edbc-112">You can use `.HasBaseType((Type)null)` to remove an entity type from the hierarchy.</span></span>

## <a name="data-annotations"></a><span data-ttu-id="9edbc-113">Annotazioni dei dati</span><span class="sxs-lookup"><span data-stu-id="9edbc-113">Data Annotations</span></span>

<span data-ttu-id="9edbc-114">È possibile usare le annotazioni dei dati per configurare l'ereditarietà.</span><span class="sxs-lookup"><span data-stu-id="9edbc-114">You cannot use Data Annotations to configure inheritance.</span></span>

## <a name="fluent-api"></a><span data-ttu-id="9edbc-115">API Fluent</span><span class="sxs-lookup"><span data-stu-id="9edbc-115">Fluent API</span></span>

<span data-ttu-id="9edbc-116">L'API Fluent per ereditarietà dipende il provider di database in che uso.</span><span class="sxs-lookup"><span data-stu-id="9edbc-116">The Fluent API for inheritance depends on the database provider you are using.</span></span> <span data-ttu-id="9edbc-117">Visualizzare [ereditarietà (Database relazionale)](relational/inheritance.md) per la configurazione è possibile eseguire per un provider di database relazionale.</span><span class="sxs-lookup"><span data-stu-id="9edbc-117">See [Inheritance (Relational Database)](relational/inheritance.md) for the configuration you can perform for a relational database provider.</span></span>
