---
title: Automatica rilevare le modifiche apportate - Entity Framework 6
author: divega
ms.date: 2016-10-23
ms.prod: entity-framework
ms.author: divega
ms.manager: avickers
ms.technology: entity-framework-6
ms.topic: article
ms.assetid: a8d1488d-9a54-4623-a76b-e81329ff2756
caps.latest.revision: 3
ms.openlocfilehash: 62f2f026426346fc1230a2f5743c8cb7d232ec7f
ms.sourcegitcommit: f05e7b62584cf228f17390bb086a61d505712e1b
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/08/2018
ms.locfileid: "39120777"
---
# <a name="automatic-detect-changes"></a>Automatica rilevare le modifiche
Quando si utilizza la maggior parte delle entità POCO la determinazione di come è stata modificata un'entità (e pertanto gli aggiornamenti devono essere inviati al database) viene gestita dall'algoritmo di rilevamento delle modifiche. Rilevare works modifiche rilevando le differenze tra i valori correnti delle proprietà dell'entità e i valori originali delle proprietà che vengono archiviati in uno snapshot quando l'entità è stata eseguita una query o collegato. Le tecniche illustrate in questo argomento si applicano in modo analogo ai modelli creati con Code First ed EF Designer.  

Per impostazione predefinita, Entity Framework esegue automaticamente di rilevare le modifiche quando vengono chiamati i metodi seguenti:  

- DbSet.Find  
- DbSet.Local  
- DbSet.Add  
- DbSet.AddRange
- DbSet.Remove  
- DbSet.RemoveRange
- DbSet.Attach  
- DbContext.SaveChanges  
- DbContext.GetValidationErrors  
- DbContext.Entry  
- DbChangeTracker.Entries  

## <a name="disabling-automatic-detection-of-changes"></a>Disabilitare il rilevamento automatico delle modifiche  

Se si stanno registrando una grande quantità di entità nel contesto e si chiama uno di questi metodi molte volte in un ciclo, è possibile ottenere miglioramenti significativi delle prestazioni disattivando il rilevamento delle modifiche per la durata del ciclo. Ad esempio:  

``` csharp
using (var context = new BloggingContext())
{
    try
    {
        context.Configuration.AutoDetectChangesEnabled = false;

        // Make many calls in a loop
        foreach (var blog in aLotOfBlogs)
        {
            context.Blogs.Add(blog);
        }
    }
    finally
    {
        context.Configuration.AutoDetectChangesEnabled = true;
    }
}
```  

Non dimenticare di riabilitare il rilevamento delle modifiche dopo il ciclo, abbiamo utilizzato una blocco try/finally per garantire sempre viene nuovamente abilitata anche se il codice nel ciclo genera un'eccezione.  

Un'alternativa alla disabilitazione e riabilitazione di campo viene lasciato il rilevamento automatico delle modifiche disabilitato in qualsiasi momento e di contesto di chiamata. ChangeTracker.DetectChanges in modo esplicito oppure usare attentamente il proxy di rilevamento delle modifiche. Entrambe queste opzioni sono avanzate e può facilmente introdurre bug nell'applicazione quindi utilizzarli con cautela.  

Se è necessario aggiungere o rimuovere molti oggetti da un contesto, è consigliabile usare DbSet.AddRange e DbSet.RemoveRange. Questi metodi rilevano automaticamente le modifiche una sola volta dopo il completamento di operazioni add o remove. 
