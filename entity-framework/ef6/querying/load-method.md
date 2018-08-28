---
title: Il metodo Load - Entity Framework 6
author: divega
ms.date: 2016-10-23
ms.assetid: 03c5a069-b7b4-455f-a16f-ee3b96cc4e28
ms.openlocfilehash: f7e8410b8fb8b5c3e86c51cd61868604a7566d0c
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 08/27/2018
ms.locfileid: "42996651"
---
# <a name="the-load-method"></a>Metodo Load
Esistono diversi scenari in cui si desidera caricare le entità dal database nel contesto di aumenterà immediatamente con le entità. Un buon esempio di questo è il caricamento di entità per il data binding come descritto in [i dati locali](~/ef6/querying/local-data.md). Un modo comune per eseguire questa operazione è scrivere una query LINQ, quindi chiamare ToList su di esso, solo per eliminare immediatamente l'elenco creato. Il metodo di estensione Load funziona esattamente come ToList ad eccezione del fatto che evita la creazione dell'elenco completamente.  

Le tecniche illustrate in questo argomento si applicano in modo analogo ai modelli creati con Code First ed EF Designer.  

Di seguito sono riportati due esempi di utilizzo del caricamento. Il primo viene acquisito da un'applicazione di associazione dati di Windows Form in cui viene usato carico per eseguire una query per le entità prima dell'associazione alla raccolta locale, come descritto in [i dati locali](~/ef6/querying/local-data.md):  

``` csharp
protected override void OnLoad(EventArgs e)
{
    base.OnLoad(e);

    _context = new ProductContext();

    _context.Categories.Load();
    categoryBindingSource.DataSource = _context.Categories.Local.ToBindingList();
}
```  

Nel secondo esempio Usa Load per caricare una raccolta filtrata di entità correlate, come descritto in [caricamento di entità correlate](~/ef6/querying/related-data.md):  

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
