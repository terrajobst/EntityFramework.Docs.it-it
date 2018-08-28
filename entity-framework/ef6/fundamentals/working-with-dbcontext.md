---
title: Utilizzo di DbContext - Entity Framework 6
author: divega
ms.date: 2016-10-23
ms.assetid: b0e6bddc-8a87-4d51-b1cb-7756df938c23
ms.openlocfilehash: f95f503c4e40e65503d5af0c1b686d0055728bfe
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 08/27/2018
ms.locfileid: "42998146"
---
# <a name="working-with-dbcontext"></a>Utilizzo di DbContext

Per usare Entity Framework per eseguire una query, inserire, aggiornare ed eliminare i dati con gli oggetti .NET, è necessario innanzitutto [creare un modello](~/ef6/modeling/index.md) che esegue il mapping di entità e relazioni definite nel modello per le tabelle in un database.

Dopo aver creato un modello, la classe primaria l'applicazione interagisca con è `System.Data.Entity.DbContext` (noto anche come la classe del contesto). È possibile usare un oggetto DbContext associati a un modello per:
- Scrivere ed eseguire query   
- Materializzare i risultati della query come oggetti entità
- Tenere traccia delle modifiche apportate agli oggetti
- Salvare in modo permanente delle modifiche degli oggetti nel database
- Associare gli oggetti in memoria ai controlli dell'interfaccia utente

Questa pagina offre alcune indicazioni su come gestire la classe del contesto.  

## <a name="defining-a-dbcontext-derived-class"></a>Definizione di una classe DbContext derivata  

Il metodo consigliato per lavorare con contesto consiste nel definire una classe che deriva da DbContext e ne espone proprietà DbSet che rappresentano raccolte di entità nel contesto specificata. Se si lavora con la finestra di progettazione di Entity Framework, il contesto verrà generato automaticamente. Se si lavora con Code First, è in genere scriverà il contesto di se stessi.  

``` csharp
public class ProductContext : DbContext
{
    public DbSet<Category> Categories { get; set; }
    public DbSet<Product> Products { get; set; }
}
```  

Dopo aver creato un contesto, è necessario eseguire una query per, aggiungere (mediante `Add` oppure `Attach` metodi) o rimuovere (usando `Remove`) entità nel contesto tramite queste proprietà. L'accesso a un `DbSet` proprietà su un oggetto di contesto rappresenta una query iniziale che restituisce tutte le entità del tipo specificato. Si noti che l'accesso solo a una proprietà non eseguirà la query. Una query viene eseguita quando:  

- Enumerata da un'istruzione `foreach` (C#) o `For Each` (Visual Basic).  
- Enumerata da un'operazione di raccolta, ad esempio `ToArray`, `ToDictionary`, o `ToList`.  
- Gli operatori LINQ come `First` o `Any` vengono specificati nella parte più esterna della query.  
- Viene chiamato uno dei metodi seguenti: il `Load` metodo di estensione `DbEntityEntry.Reload`, `Database.ExecuteSqlCommand`, e `DbSet<T>.Find`, se un'entità con la chiave specificata non è presente già caricate nel contesto.  

## <a name="lifetime"></a>Durata  

La durata del contesto inizia quando l'istanza viene creata e termina quando l'istanza viene eliminata o sottoposto a garbage collection. Uso **usando** se si desidera che tutte le risorse controllate dal contesto siano eliminate alla fine del blocco. Quando si usa **usando**, il compilatore crea automaticamente un blocco try/finally e chiama dispose nel **infine** blocco.  

``` csharp
public void UseProducts()
{
    using (var context = new ProductContext())
    {     
        // Perform data access using the context
    }
}
```  

Di seguito sono riportate alcune linee guida generali prima di decidere sulla durata del contesto:  

- Quando si lavora con le applicazioni Web, usare un'istanza del contesto per ogni richiesta.  
- Quando si lavora con Windows Presentation Foundation (WPF) o Windows Form, usare un'istanza del contesto per ogni modulo. Ciò consente di usare la funzionalità di rilevamento delle modifiche fornisce tale contesto.  
- Se l'istanza del contesto viene creato da un contenitore di inserimento delle dipendenze, è in genere la responsabilità del contenitore da eliminare il contesto.
- Se il contesto viene creato nel codice dell'applicazione, ricordarsi di eliminare il contesto quando non è più necessaria.  
- Quando si lavora sul contesto con esecuzione prolungata tenere presente quanto segue:  
    - Come si caricano più oggetti e i relativi riferimenti in memoria, il consumo di memoria del contesto può aumentare rapidamente. È possibile pertanto che si verifichino problemi di prestazioni.  
    - Il contesto non è thread-safe, pertanto non deve essere condivise tra più thread lavorare contemporaneamente su di esso.
    - Se un'eccezione provoca uno stato irreversibile per il contesto, l'intera applicazione può terminare.  
    - Le possibilità che si verifichino problemi correlati alla concorrenza aumentano quando l'intervallo tra il momento in cui viene eseguita una query sui dati e quello in cui i dati vengono aggiornati aumenta.  

## <a name="connections"></a>Connessioni  

Per impostazione predefinita, il contesto gestisce le connessioni al database. Il contesto apre e chiude le connessioni in base alle esigenze. Ad esempio, il contesto viene aperta una connessione per eseguire una query e quindi chiude la connessione quando sono stati elaborati tutti i set di risultati.  

In alcuni casi, tuttavia, può essere necessario disporre di maggiore controllo sull'apertura e la chiusura della connessione. Ad esempio, quando si usa SQL Server Compact, è spesso consigliabile mantenere una connessione aperta separata per il database per la durata dell'applicazione per migliorare le prestazioni. È possibile gestire manualmente questo processo tramite la proprietà `Connection`.  
