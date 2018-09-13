---
title: Il metodo Load - Entity Framework 6
author: divega
ms.date: 10/23/2016
ms.assetid: 03c5a069-b7b4-455f-a16f-ee3b96cc4e28
ms.openlocfilehash: 3a0d11552b6bfd8b83f15c58c6cb9f945d9d4536
ms.sourcegitcommit: 2b787009fd5be5627f1189ee396e708cd130e07b
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/13/2018
ms.locfileid: "45490896"
---
# <a name="the-load-method"></a><span data-ttu-id="ef202-102">Metodo Load</span><span class="sxs-lookup"><span data-stu-id="ef202-102">The Load Method</span></span>
<span data-ttu-id="ef202-103">Esistono diversi scenari in cui si desidera caricare le entità dal database nel contesto di aumenterà immediatamente con le entità.</span><span class="sxs-lookup"><span data-stu-id="ef202-103">There are several scenarios where you may want to load entities from the database into the context without immediately doing anything with those entities.</span></span> <span data-ttu-id="ef202-104">Un buon esempio di questo è il caricamento di entità per il data binding come descritto in [i dati locali](~/ef6/querying/local-data.md).</span><span class="sxs-lookup"><span data-stu-id="ef202-104">A good example of this is loading entities for data binding as described in [Local Data](~/ef6/querying/local-data.md).</span></span> <span data-ttu-id="ef202-105">Un modo comune per eseguire questa operazione è scrivere una query LINQ, quindi chiamare ToList su di esso, solo per eliminare immediatamente l'elenco creato.</span><span class="sxs-lookup"><span data-stu-id="ef202-105">One common way to do this is to write a LINQ query and then call ToList on it, only to immediately discard the created list.</span></span> <span data-ttu-id="ef202-106">Il metodo di estensione Load funziona esattamente come ToList ad eccezione del fatto che evita la creazione dell'elenco completamente.</span><span class="sxs-lookup"><span data-stu-id="ef202-106">The Load extension method works just like ToList except that it avoids the creation of the list altogether.</span></span>  

<span data-ttu-id="ef202-107">Le tecniche illustrate in questo argomento si applicano in modo analogo ai modelli creati con Code First ed EF Designer.</span><span class="sxs-lookup"><span data-stu-id="ef202-107">The techniques shown in this topic apply equally to models created with Code First and the EF Designer.</span></span>  

<span data-ttu-id="ef202-108">Di seguito sono riportati due esempi di utilizzo del caricamento.</span><span class="sxs-lookup"><span data-stu-id="ef202-108">Here are two examples of using Load.</span></span> <span data-ttu-id="ef202-109">Il primo viene acquisito da un'applicazione di associazione dati di Windows Form in cui viene usato carico per eseguire una query per le entità prima dell'associazione alla raccolta locale, come descritto in [i dati locali](~/ef6/querying/local-data.md):</span><span class="sxs-lookup"><span data-stu-id="ef202-109">The first is taken from a Windows Forms data binding application where Load is used to query for entities before binding to the local collection, as described in [Local Data](~/ef6/querying/local-data.md):</span></span>  

``` csharp
protected override void OnLoad(EventArgs e)
{
    base.OnLoad(e);

    _context = new ProductContext();

    _context.Categories.Load();
    categoryBindingSource.DataSource = _context.Categories.Local.ToBindingList();
}
```  

<span data-ttu-id="ef202-110">Nel secondo esempio Usa Load per caricare una raccolta filtrata di entità correlate, come descritto in [caricamento di entità correlate](~/ef6/querying/related-data.md):</span><span class="sxs-lookup"><span data-stu-id="ef202-110">The second example shows using Load to load a filtered collection of related entities, as described in [Loading Related Entities](~/ef6/querying/related-data.md):</span></span>  

``` csharp
using (var context = new BloggingContext())
{
    var blog = context.Blogs.Find(1);

    // Load the posts with the 'entity-framework' tag related to a given blog
    context.Entry(blog)
        .Collection(b => b.Posts)
        .Query()
        .Where(p => p.Tags.Contains("entity-framework")
        .Load();
}
```  
