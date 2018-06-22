---
title: Creazione di un modello - EF Core
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: 88253ff3-174e-485c-b3f8-768243d01ee1
ms.technology: entity-framework-core
uid: core/modeling/index
ms.openlocfilehash: 1ad0f6891fbc8ba2e4d102cc9997f053a9dddb66
ms.sourcegitcommit: 507a40ed050fee957bcf8cf05f6e0ec8a3b1a363
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 04/26/2018
ms.locfileid: "31812437"
---
# <a name="creating-a-model"></a><span data-ttu-id="fdc9c-102">Creazione di un modello</span><span class="sxs-lookup"><span data-stu-id="fdc9c-102">Creating a Model</span></span>

<span data-ttu-id="fdc9c-103">Entity Framework usa un insieme di convenzioni per compilare un modello in base alla forma delle classi di entità.</span><span class="sxs-lookup"><span data-stu-id="fdc9c-103">Entity Framework uses a set of conventions to build a model based on the shape of your entity classes.</span></span> <span data-ttu-id="fdc9c-104">È possibile specificare una configurazione aggiuntiva per integrare e/o sostituire gli elementi individuati dalla convenzione.</span><span class="sxs-lookup"><span data-stu-id="fdc9c-104">You can specify additional configuration to supplement and/or override what was discovered by convention.</span></span>

<span data-ttu-id="fdc9c-105">Questo articolo descrive la configurazione che può essere applicata a un modello destinato a qualsiasi archivio dati e che può essere applicata per qualsiasi database relazionale.</span><span class="sxs-lookup"><span data-stu-id="fdc9c-105">This article covers configuration that can be applied to a model targeting any data store and that which can be applied when targeting any relational database.</span></span> <span data-ttu-id="fdc9c-106">I provider possono anche abilitare una configurazione specifica di un archivio dati.</span><span class="sxs-lookup"><span data-stu-id="fdc9c-106">Providers may also enable configuration that is specific to a particular data store.</span></span> <span data-ttu-id="fdc9c-107">Per informazioni sulla configurazione specifica del provider, vedere la sezione [Provider di database](../providers/index.md).</span><span class="sxs-lookup"><span data-stu-id="fdc9c-107">For documentation on provider specific configuration see the [Database Providers](../providers/index.md) section.</span></span>

> [!TIP]  
> <span data-ttu-id="fdc9c-108">L'[esempio](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples) di questo articolo è disponibile in GitHub.</span><span class="sxs-lookup"><span data-stu-id="fdc9c-108">You can view this article’s [sample](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples) on GitHub.</span></span>

## <a name="methods-of-configuration"></a><span data-ttu-id="fdc9c-109">Metodi di configurazione</span><span class="sxs-lookup"><span data-stu-id="fdc9c-109">Methods of configuration</span></span>

### <a name="fluent-api"></a><span data-ttu-id="fdc9c-110">API Fluent</span><span class="sxs-lookup"><span data-stu-id="fdc9c-110">Fluent API</span></span>

<span data-ttu-id="fdc9c-111">È possibile eseguire l'override del metodo `OnModelCreating` nel contesto derivato e usare `ModelBuilder API` per configurare il modello.</span><span class="sxs-lookup"><span data-stu-id="fdc9c-111">You can override the `OnModelCreating` method in your derived context and use the `ModelBuilder API` to configure your model.</span></span> <span data-ttu-id="fdc9c-112">Questo è il metodo di configurazione più efficace e consente di specificare la configurazione senza modificare le classi di entità.</span><span class="sxs-lookup"><span data-stu-id="fdc9c-112">This is the most powerful method of configuration and allows configuration to be specified without modifying your entity classes.</span></span> <span data-ttu-id="fdc9c-113">La configurazione dell'API Fluent ha la precedenza più elevata e sostituisce le convenzioni e le annotazioni dei dati.</span><span class="sxs-lookup"><span data-stu-id="fdc9c-113">Fluent API configuration has the highest precedence and will override conventions and data annotations.</span></span>

<!-- [!code-csharp[Main](samples/core/Modeling/FluentAPI/Samples/Required.cs?range=5-15&highlight=5-10)] -->

``` csharp
    class MyContext : DbContext
    {
        public DbSet<Blog> Blogs { get; set; }

        protected override void OnModelCreating(ModelBuilder modelBuilder)
        {
            modelBuilder.Entity<Blog>()
                .Property(b => b.Url)
                .IsRequired();
        }
    }
```

### <a name="data-annotations"></a><span data-ttu-id="fdc9c-114">Annotazioni dei dati</span><span class="sxs-lookup"><span data-stu-id="fdc9c-114">Data Annotations</span></span>

<span data-ttu-id="fdc9c-115">È anche possibile applicare attributi (chiamati annotazioni dei dati) alle classi e alle proprietà.</span><span class="sxs-lookup"><span data-stu-id="fdc9c-115">You can also apply attributes (known as Data Annotations) to your classes and properties.</span></span> <span data-ttu-id="fdc9c-116">Le annotazioni dei dati sostituiscono le convenzioni, ma vengono sovrascritte dalla configurazione dell'API Fluent.</span><span class="sxs-lookup"><span data-stu-id="fdc9c-116">Data annotations will override conventions, but will be overwritten by Fluent API configuration.</span></span>

<!-- [!code-csharp[Main](samples/core/Modeling/DataAnnotations/Samples/Required.cs?range=11-16&highlight=4)] -->
``` csharp
    public class Blog
    {
        public int BlogId { get; set; }
        [Required]
        public string Url { get; set; }
    }
```
