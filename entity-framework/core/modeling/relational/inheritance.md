---
title: Ereditarietà (database relazionale)-EF Core
description: Come configurare l'ereditarietà del tipo di entità in un database relazionale tramite Entity Framework Core
author: AndriySvyryd
ms.author: ansvyryd
ms.date: 11/06/2019
uid: core/modeling/relational/inheritance
ms.openlocfilehash: 30e25aa2968ceab03404baddb46e0ae59fc3ea6b
ms.sourcegitcommit: 7a709ce4f77134782393aa802df5ab2718714479
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 12/04/2019
ms.locfileid: "74824752"
---
# <a name="inheritance-relational-database"></a><span data-ttu-id="6d862-103">Ereditarietà (database relazionale)</span><span class="sxs-lookup"><span data-stu-id="6d862-103">Inheritance (Relational Database)</span></span>

> [!NOTE]  
> <span data-ttu-id="6d862-104">La configurazione di questa sezione è applicabile in generale ai database relazionali.</span><span class="sxs-lookup"><span data-stu-id="6d862-104">The configuration in this section is applicable to relational databases in general.</span></span> <span data-ttu-id="6d862-105">I metodi di estensione descritti diventano disponibili quando si installa un provider di database relazionali (a causa del pacchetto *Microsoft.EntityFrameworkCore.Relational* condiviso).</span><span class="sxs-lookup"><span data-stu-id="6d862-105">The extension methods shown here will become available when you install a relational database provider (due to the shared *Microsoft.EntityFrameworkCore.Relational* package).</span></span>

<span data-ttu-id="6d862-106">L'ereditarietà nel modello EF viene utilizzata per controllare il modo in cui l'ereditarietà nelle classi di entità viene rappresentata nel database.</span><span class="sxs-lookup"><span data-stu-id="6d862-106">Inheritance in the EF model is used to control how inheritance in the entity classes is represented in the database.</span></span>

> [!NOTE]  
> <span data-ttu-id="6d862-107">Attualmente, in EF Core viene implementato solo il modello tabella per gerarchia (TPH).</span><span class="sxs-lookup"><span data-stu-id="6d862-107">Currently, only the table-per-hierarchy (TPH) pattern is implemented in EF Core.</span></span> <span data-ttu-id="6d862-108">Altri modelli comuni, ad esempio tabella per tipo (TPT) e tabella per tipo concreto (TPC), non sono ancora disponibili.</span><span class="sxs-lookup"><span data-stu-id="6d862-108">Other common patterns like table-per-type (TPT) and table-per-concrete-type (TPC) are not yet available.</span></span>

## <a name="conventions"></a><span data-ttu-id="6d862-109">Convenzioni</span><span class="sxs-lookup"><span data-stu-id="6d862-109">Conventions</span></span>

<span data-ttu-id="6d862-110">Per impostazione predefinita, viene eseguito il mapping dell'ereditarietà utilizzando il modello tabella per gerarchia (TPH).</span><span class="sxs-lookup"><span data-stu-id="6d862-110">By default, inheritance will be mapped using the table-per-hierarchy (TPH) pattern.</span></span> <span data-ttu-id="6d862-111">TPH usa una singola tabella per archiviare i dati per tutti i tipi nella gerarchia.</span><span class="sxs-lookup"><span data-stu-id="6d862-111">TPH uses a single table to store the data for all types in the hierarchy.</span></span> <span data-ttu-id="6d862-112">Una colonna discriminatore viene utilizzata per identificare il tipo rappresentato da ogni riga.</span><span class="sxs-lookup"><span data-stu-id="6d862-112">A discriminator column is used to identify which type each row represents.</span></span>

<span data-ttu-id="6d862-113">EF Core configurerà l'ereditarietà solo se due o più tipi ereditati vengono inclusi in modo esplicito nel modello. per ulteriori informazioni, vedere [ereditarietà](../inheritance.md) .</span><span class="sxs-lookup"><span data-stu-id="6d862-113">EF Core will only setup inheritance if two or more inherited types are explicitly included in the model (see [Inheritance](../inheritance.md) for more details).</span></span>

<span data-ttu-id="6d862-114">Di seguito è riportato un esempio che mostra uno scenario di ereditarietà semplice e i dati archiviati in una tabella di database relazionale usando il modello TPH.</span><span class="sxs-lookup"><span data-stu-id="6d862-114">Below is an example showing a simple inheritance scenario and the data stored in a relational database table using the TPH pattern.</span></span> <span data-ttu-id="6d862-115">La colonna *discriminatore* identifica il tipo di *Blog* archiviato in ogni riga.</span><span class="sxs-lookup"><span data-stu-id="6d862-115">The *Discriminator* column identifies which type of *Blog* is stored in each row.</span></span>

[!code-csharp[Main](../../../../samples/core/Modeling/Conventions/InheritanceDbSets.cs#Model)]

![image](_static/inheritance-tph-data.png)

>[!NOTE]
> <span data-ttu-id="6d862-117">Le colonne del database vengono rese automaticamente Nullable quando necessario quando si usa il mapping di TPH.</span><span class="sxs-lookup"><span data-stu-id="6d862-117">Database columns are automatically made nullable as necessary when using TPH mapping.</span></span>

## <a name="data-annotations"></a><span data-ttu-id="6d862-118">Annotazioni dei dati</span><span class="sxs-lookup"><span data-stu-id="6d862-118">Data Annotations</span></span>

<span data-ttu-id="6d862-119">Non è possibile utilizzare le annotazioni dei dati per configurare l'ereditarietà.</span><span class="sxs-lookup"><span data-stu-id="6d862-119">You cannot use Data Annotations to configure inheritance.</span></span>

## <a name="fluent-api"></a><span data-ttu-id="6d862-120">API Fluent</span><span class="sxs-lookup"><span data-stu-id="6d862-120">Fluent API</span></span>

<span data-ttu-id="6d862-121">È possibile utilizzare l'API Fluent per configurare il nome e il tipo della colonna discriminatore e i valori utilizzati per identificare ogni tipo nella gerarchia.</span><span class="sxs-lookup"><span data-stu-id="6d862-121">You can use the Fluent API to configure the name and type of the discriminator column and the values that are used to identify each type in the hierarchy.</span></span>

[!code-csharp[Main](../../../../samples/core/Modeling/FluentAPI/InheritanceTPHDiscriminator.cs#Inheritance)]

## <a name="configuring-the-discriminator-property"></a><span data-ttu-id="6d862-122">Configurazione della proprietà Discriminator</span><span class="sxs-lookup"><span data-stu-id="6d862-122">Configuring the discriminator property</span></span>

<span data-ttu-id="6d862-123">Negli esempi precedenti, il discriminatore viene creato come [proprietà shadow](xref:core/modeling/shadow-properties) nell'entità di base della gerarchia.</span><span class="sxs-lookup"><span data-stu-id="6d862-123">In the examples above, the discriminator is created as a [shadow property](xref:core/modeling/shadow-properties) on the base entity of the hierarchy.</span></span> <span data-ttu-id="6d862-124">Poiché si tratta di una proprietà nel modello, può essere configurata esattamente come le altre proprietà.</span><span class="sxs-lookup"><span data-stu-id="6d862-124">Since it is a property in the model, it can be configured just like other properties.</span></span> <span data-ttu-id="6d862-125">Ad esempio, per impostare la lunghezza massima quando viene usato il discriminatore predefinito per convenzione:</span><span class="sxs-lookup"><span data-stu-id="6d862-125">For example, to set the max length when the default, by-convention discriminator is being used:</span></span>

[!code-csharp[Main](../../../../samples/core/Modeling/FluentAPI/DefaultDiscriminator.cs#DiscriminatorConfiguration)]

<span data-ttu-id="6d862-126">È anche possibile eseguire il mapping del discriminatore a una proprietà .NET nell'entità e configurarla.</span><span class="sxs-lookup"><span data-stu-id="6d862-126">The discriminator can also be mapped to a .NET property in your entity and configure it.</span></span> <span data-ttu-id="6d862-127">Ad esempio:</span><span class="sxs-lookup"><span data-stu-id="6d862-127">For example:</span></span>

[!code-csharp[Main](../../../../samples/core/Modeling/FluentAPI/NonShadowDiscriminator.cs#NonShadowDiscriminator)]

## <a name="shared-columns"></a><span data-ttu-id="6d862-128">Colonne condivise</span><span class="sxs-lookup"><span data-stu-id="6d862-128">Shared columns</span></span>

<span data-ttu-id="6d862-129">Quando due tipi di entità di pari livello hanno una proprietà con lo stesso nome, per impostazione predefinita verrà eseguito il mapping a due colonne separate.</span><span class="sxs-lookup"><span data-stu-id="6d862-129">When two sibling entity types have a property with the same name they will be mapped to two separate columns by default.</span></span> <span data-ttu-id="6d862-130">Tuttavia, se sono compatibili, è possibile eseguirne il mapping alla stessa colonna:</span><span class="sxs-lookup"><span data-stu-id="6d862-130">But if they are compatible they can be mapped to the same column:</span></span>

[!code-csharp[Main](../../../../samples/core/Modeling/FluentAPI/SharedTPHColumns.cs#SharedTPHColumns)]