---
title: Creazione e configurazione di un modello - EF Core
author: rowanmiller
ms.date: 11/05/2019
ms.assetid: 88253ff3-174e-485c-b3f8-768243d01ee1
uid: core/modeling/index
ms.openlocfilehash: 58be4a45473c6292790da341e360b3340de27be7
ms.sourcegitcommit: 7a709ce4f77134782393aa802df5ab2718714479
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 12/04/2019
ms.locfileid: "74824691"
---
# <a name="creating-and-configuring-a-model"></a><span data-ttu-id="84bcf-102">Creazione e configurazione di un modello</span><span class="sxs-lookup"><span data-stu-id="84bcf-102">Creating and configuring a model</span></span>

<span data-ttu-id="84bcf-103">Entity Framework usa un insieme di convenzioni per compilare un modello in base alla forma delle classi di entità.</span><span class="sxs-lookup"><span data-stu-id="84bcf-103">Entity Framework uses a set of conventions to build a model based on the shape of your entity classes.</span></span> <span data-ttu-id="84bcf-104">È possibile specificare una configurazione aggiuntiva per integrare e/o sostituire gli elementi individuati dalla convenzione.</span><span class="sxs-lookup"><span data-stu-id="84bcf-104">You can specify additional configuration to supplement and/or override what was discovered by convention.</span></span>

<span data-ttu-id="84bcf-105">Questo articolo descrive la configurazione che può essere applicata a un modello destinato a qualsiasi archivio dati e che può essere applicata per qualsiasi database relazionale.</span><span class="sxs-lookup"><span data-stu-id="84bcf-105">This article covers configuration that can be applied to a model targeting any data store and that which can be applied when targeting any relational database.</span></span> <span data-ttu-id="84bcf-106">I provider possono anche abilitare una configurazione specifica di un archivio dati.</span><span class="sxs-lookup"><span data-stu-id="84bcf-106">Providers may also enable configuration that is specific to a particular data store.</span></span> <span data-ttu-id="84bcf-107">Per informazioni sulla configurazione specifica del provider, vedere la sezione  [Provider di database](../providers/index.md) .</span><span class="sxs-lookup"><span data-stu-id="84bcf-107">For documentation on provider specific configuration see the [Database Providers](../providers/index.md) section.</span></span>

> [!TIP]  
> <span data-ttu-id="84bcf-108">È possibile visualizzare l' [esempio](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples)  di questo articolo in GitHub.</span><span class="sxs-lookup"><span data-stu-id="84bcf-108">You can view this article’s [sample](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples) on GitHub.</span></span>

## <a name="use-fluent-api-to-configure-a-model"></a><span data-ttu-id="84bcf-109">Usare l'API Fluent per configurare un modello</span><span class="sxs-lookup"><span data-stu-id="84bcf-109">Use fluent API to configure a model</span></span>

<span data-ttu-id="84bcf-110">È possibile eseguire l'override del metodo  `OnModelCreating`  nel contesto derivato e usare  `ModelBuilder API` per configurare il modello.</span><span class="sxs-lookup"><span data-stu-id="84bcf-110">You can override the `OnModelCreating` method in your derived context and use the `ModelBuilder API` to configure your model.</span></span> <span data-ttu-id="84bcf-111">Questo è il metodo di configurazione più efficace e consente di specificare la configurazione senza modificare le classi di entità.</span><span class="sxs-lookup"><span data-stu-id="84bcf-111">This is the most powerful method of configuration and allows configuration to be specified without modifying your entity classes.</span></span> <span data-ttu-id="84bcf-112">La configurazione dell'API Fluent ha la precedenza più elevata e sostituisce le convenzioni e le annotazioni dei dati.</span><span class="sxs-lookup"><span data-stu-id="84bcf-112">Fluent API configuration has the highest precedence and will override conventions and data annotations.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Required.cs?highlight=11-13)]

## <a name="use-data-annotations-to-configure-a-model"></a><span data-ttu-id="84bcf-113">Usare le annotazioni dei dati per configurare un modello</span><span class="sxs-lookup"><span data-stu-id="84bcf-113">Use data annotations to configure a model</span></span>

<span data-ttu-id="84bcf-114">È anche possibile applicare attributi (chiamati annotazioni dei dati) alle classi e alle proprietà.</span><span class="sxs-lookup"><span data-stu-id="84bcf-114">You can also apply attributes (known as Data Annotations) to your classes and properties.</span></span> <span data-ttu-id="84bcf-115">Le annotazioni dei dati sostituiranno le convenzioni, ma saranno a loro volta ignorate dalla configurazione dell'API Fluent.</span><span class="sxs-lookup"><span data-stu-id="84bcf-115">Data annotations will override conventions, but will be overridden by Fluent API configuration.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/Required.cs?highlight=14)]
