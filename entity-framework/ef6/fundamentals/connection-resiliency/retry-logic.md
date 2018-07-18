---
title: Connessione tentativi e la resilienza per la logica - Entity Framework 6
author: divega
ms.date: 2016-10-23
ms.prod: entity-framework
ms.author: divega
ms.manager: avickers
ms.technology: entity-framework-6
ms.topic: article
ms.assetid: 47d68ac1-927e-4842-ab8c-ed8c8698dff2
caps.latest.revision: 3
ms.openlocfilehash: 4c6e296ecb8b43468408d359f44fd1d1a6edf864
ms.sourcegitcommit: 390f3a37bc55105ed7cc5b0e0925b7f9c9e80ba6
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/09/2018
ms.locfileid: "39121340"
---
# <a name="connection-resiliency-and-retry-logic"></a>Logica di tentativi e la resilienza di connessione
> [!NOTE]
> **Solo EF6 e versioni successive**: funzionalità, API e altri argomenti discussi in questa pagina sono stati introdotti in Entity Framework 6. Se si usa una versione precedente, le informazioni qui riportate, o parte di esse, non sono applicabili.  

Applicazioni che si connettono a un server di database sono sempre state vulnerabili alle interruzioni di connessione a causa di errori di back-end e dall'instabilità della rete. Tuttavia, in un ambiente basato su LAN funziona con i server di database dedicato questi errori sono abbastanza rari che per la logica aggiuntiva per gestire tali errori non è spesso necessaria. Con l'avvento del cloud basati su server di database, ad esempio Database SQL di Azure e le connessioni su reti meno affidabile che è ora più comune per le interruzioni di connessione si verifichi. Probabilmente a causa di tecniche difensive che Usa database per garantire l'equità del servizio, ad esempio la limitazione delle richieste di connessione, o di instabilità della rete causando i timeout intermittenti e altri errori temporanei di cloud.  

Resilienza della connessione si intende la possibilità di ritentare automaticamente i comandi che non riescono a causa di queste interruzioni di connessione a Entity Framework.  

## <a name="execution-strategies"></a>Strategie di esecuzione  

Nuovi tentativi di connessione viene preso in considerazione da un'implementazione dell'interfaccia IDbExecutionStrategy. Le implementazioni del IDbExecutionStrategy saranno ritenuto responsabile per l'accettazione di un'operazione e, se si verifica un'eccezione, che determina se un nuovo tentativo è appropriato e nuovo tentativo in caso. Esistono quattro strategie di esecuzione forniti con Entity Framework:  

1. **DefaultExecutionStrategy**: questa strategia di esecuzione non ripeterà tutte le operazioni, è il valore predefinito per i database diversi da sql server.  
2. **DefaultSqlExecutionStrategy**: si tratta di una strategia di esecuzione interna che viene usata per impostazione predefinita. Questa strategia non tenterà affatto, tuttavia, eseguirà il wrapping di tutte le eccezioni che potrebbero essere temporanee per informare gli utenti che desiderano abilitare la resilienza di connessione.  
3. **DbExecutionStrategy**: questa classe può fungere da una classe di base per altre strategie di esecuzione, tra cui i proprio quelle personalizzate. Implementa un criterio di ripetizione esponenziale, in cui il numero di tentativi iniziale viene eseguita con zero ritardo e ritardo aumenta in misura esponenziale fino a quando non viene raggiunto il numero massimo di tentativi. Questa classe ha un metodo ShouldRetryOn astratto che può essere implementato nelle strategie di esecuzione derivata per controllare le eccezioni che devono essere riprovate.  
4. **SqlAzureExecutionStrategy**: questa strategia di esecuzione eredita da DbExecutionStrategy e ritenterà in caso di eccezioni noti per essere eventualmente temporaneo quando si lavora con Database SQL di Azure.

> [!NOTE]
> Strategie di esecuzione 2 e 4 sono inclusi nel provider di Sql Server fornito con Entity Framework, ovvero nell'assembly EntityFramework. SqlServer e sono progettate per funzionare con SQL Server.  

## <a name="enabling-an-execution-strategy"></a>Abilitazione di una strategia di esecuzione  

È il modo più semplice per indicare a Entity Framework da usare una strategia di esecuzione con il metodo SetExecutionStrategy il [DbConfiguration](~/ef6/fundamentals/configuring/code-based.md) classe:  

``` csharp
public class MyConfiguration : DbConfiguration
{
    public MyConfiguration()
    {
        SetExecutionStrategy("System.Data.SqlClient", () => new SqlAzureExecutionStrategy());
    }
}
```  

Questo codice indica a Entity Framework da usare il SqlAzureExecutionStrategy quando ci si connette a SQL Server.  

## <a name="configuring-the-execution-strategy"></a>Configurare la strategia di esecuzione  

Il costruttore di SqlAzureExecutionStrategy può accettare due parametri, MaxRetryCount e MaxDelay. Conteggio MaxRetry è il numero massimo di volte in cui la strategia eseguirà un nuovo tentativo. Il MaxDelay è un oggetto TimeSpan che rappresenta il ritardo massimo tra i tentativi che utilizzerà la strategia di esecuzione.  

Per impostare il numero massimo di tentativi per il ritardo massimo a 30 secondi e 1 si farebbe execue quanto segue:  

``` csharp
public class MyConfiguration : DbConfiguration
{
    public MyConfiguration()
    {
        SetExecutionStrategy(
            "System.Data.SqlClient",
            () => new SqlAzureExecutionStrategy(1, TimeSpan.FromSeconds(30)));
    }
}
```  

Il SqlAzureExecutionStrategy verranno effettuati tentativi immediatamente la prima volta che si verifica un errore temporaneo, ma verrà ritardata più tra i singoli tentativi fino a quando non un il numero massimo di tentativi limite viene superata o il tempo totale raggiunge il ritardo massimo.  

Le strategie di esecuzione riproverà solo un numero limitato di eccezioni che sono in genere tansient, sarà comunque necessario gestire gli altri errori, oltre a rilevare l'eccezione RetryLimitExceeded nel caso in cui un errore non è temporaneo o richiede troppo tempo risolvere se stessa.  

## <a name="limitations"></a>Limitazioni  

Esistono alcune note delle limitazioni quando si usa una strategia di esecuzione di nuovo tentativo:  

### <a name="streaming-queries-are-not-supported"></a>Non sono supportate le query di streaming  

Per impostazione predefinita, Entity Framework 6 e versioni successive nel buffer i risultati della query anziché streaming li. Se si desidera avere risultati trasmessi è possibile usare il metodo AsStreaming per modificare un LINQ in query di entità in streaming.  

``` csharp
using (var db = new BloggingContext())
{
    var query = (from b in db.Blogs
                orderby b.Url
                select b).AsStreaming();
    }
}
```  

Flusso non è supportato quando viene registrata una nuovo tentativo strategia di esecuzione. Questa limitazione esiste perché la connessione è stato possibile eliminare più tramite i risultati restituiti. In questo caso, EF deve eseguire di nuovo l'intera query ma non dispone di alcun modo affidabile di sapere quali risultati già restituiti (dati potrebbero essere cambiato dall'invio della query iniziale, i risultati potrebbero tornare in un ordine diverso, i risultati potrebbero non avere un identificatore univoco e così via.).  

### <a name="user-initiated-transactions-not-supported"></a>Le transazioni non supportate avviata dall'utente  

Dopo aver configurato una strategia di esecuzione che genera nuovi tentativi, esistono alcune limitazioni per l'utilizzo di transazioni.  

#### <a name="whats-supported-efs-default-transaction-behavior"></a>Che cos'è supportato: il comportamento della transazione di predefinito di Entity Framework  

Per impostazione predefinita, Entity Framework eseguirà gli aggiornamenti di database all'interno di una transazione. Non è necessario eseguire alcuna operazione per abilitare questa opzione, EF sempre viene eseguito automaticamente.  

Ad esempio, nel codice seguente SaveChanges viene eseguita automaticamente all'interno di una transazione. Se SaveChanges dovesse avere esito negativo dopo aver inserito tra il nuovo sito, quindi potrebbe essere eseguito il rollback della transazione e nessuna modifica applicata al database. Il contesto è anche disponibile in uno stato che consente a SaveChanges venga nuovamente chiamata per eseguire l'applicazione delle modifiche.  

``` csharp
using (var db = new BloggingContext())
{
    db.Blogs.Add(new Site { Url = "http://msdn.com/data/ef" });
    db.Blogs.Add(new Site { Url = "http://blogs.msdn.com/adonet" });
    db.SaveChanges();
}
```  

#### <a name="whats-not-supported-user-initiated-transactions"></a>Che cosa non è supportata: le transazioni avviata dall'utente  

Se non si usa una strategia di esecuzione di nuovo tentativo è possibile eseguire il wrapping di più operazioni in una singola transazione. Ad esempio, il codice seguente esegue il wrapping di due chiamate a SaveChanges in un'unica transazione. Se qualsiasi parte di questa operazione ha esito negativo, nessuna delle modifiche vengono applicate.  

``` csharp
using (var db = new BloggingContext())
{
    using (var trn = db.Database.BeginTransaction())
    {
        db.Blogs.Add(new Site { Url = "http://msdn.com/data/ef" });
        db.Blogs.Add(new Site { Url = "http://blogs.msdn.com/adonet" });
        db.SaveChanges();

        db.Blogs.Add(new Site { Url = "http://twitter.com/efmagicunicorns" });
        db.SaveChanges();

        trn.Commit();
    }
}
```  

Ciò non è supportato quando si usa una strategia di esecuzione di nuovo tentativo perché EF non tiene conto di qualsiasi operazione precedente e su come eseguire un nuovo tentativo. Ad esempio, se il secondo metodo SaveChanges, l'errore EF non è più ha le informazioni necessarie per ripetere la prima chiamata a SaveChanges.  

#### <a name="possible-workarounds"></a>Possibili soluzioni alternative  

##### <a name="suspend-execution-strategy"></a>Sospendere una strategia di esecuzione  

Una possibile soluzione alternativa consiste nel sospendere la strategia di esecuzione nuovo tentativo per il frammento di codice che deve usare un utente avviate delle transazioni. Il modo più semplice per eseguire questa operazione consiste nell'aggiungere un flag SuspendExecutionStrategy al codice in base a classe di configurazione e modifica l'espressione lambda strategia di esecuzione per restituire la strategia di esecuzione (non-retying) predefinito quando il flag è impostato.  

``` csharp
using System.Data.Entity;
using System.Data.Entity.Infrastructure;
using System.Data.Entity.SqlServer;
using System.Runtime.Remoting.Messaging;

namespace Demo
{
    public class MyConfiguration : DbConfiguration
    {
        public MyConfiguration()
        {
            this.SetExecutionStrategy("System.Data.SqlClient", () => SuspendExecutionStrategy
              ? (IDbExecutionStrategy)new DefaultExecutionStrategy()
              : new SqlAzureExecutionStrategy());
        }

        public static bool SuspendExecutionStrategy
        {
            get
            {
                return (bool?)CallContext.LogicalGetData("SuspendExecutionStrategy")  false;
            }
            set
            {
                CallContext.LogicalSetData("SuspendExecutionStrategy", value);
            }
        }
    }
}
```  

Si noti che si sta usando CallContext per archiviare il valore del flag. Questo fornisce funzionalità simili all'archiviazione thread-local, ma è possibile usare con il codice asincrono - tra cui query asincrona e risparmiare con Entity Framework.  

È ora possibile sospendere la strategia di esecuzione per la sezione di codice che utilizza una transazione avviata dall'utente.  

``` csharp
using (var db = new BloggingContext())
{
    MyConfiguration.SuspendExecutionStrategy = true;

    using (var trn = db.Database.BeginTransaction())
    {
        db.Blogs.Add(new Blog { Url = "http://msdn.com/data/ef" });
        db.Blogs.Add(new Blog { Url = "http://blogs.msdn.com/adonet" });
        db.SaveChanges();

        db.Blogs.Add(new Blog { Url = "http://twitter.com/efmagicunicorns" });
        db.SaveChanges();

        trn.Commit();
    }

    MyConfiguration.SuspendExecutionStrategy = false;
}
```  

##### <a name="manually-call-execution-strategy"></a>Chiamare manualmente strategia di esecuzione  

Un'altra possibilità è usare la strategia di esecuzione e assegnargli l'intero set di logica da eseguire, in modo che è possibile riprovare a eseguire tutto ciò che in caso di una delle operazioni manualmente. Tuttavia è necessario sospendere la strategia di esecuzione - uso della tecnica illustrato in precedenza, in modo che i contesti utilizzati all'interno del blocco di codice non irreversibile non tentano di ripetere.  

Si noti che i contesti devono essere creati all'interno del blocco di codice per effettuare altri tentativi. Ciò garantisce che sta iniziando con uno stato pulito per ogni nuovo tentativo.  

``` csharp
var executionStrategy = new SqlAzureExecutionStrategy();

MyConfiguration.SuspendExecutionStrategy = true;

executionStrategy.Execute(
    () =>
    {
        using (var db = new BloggingContext())
        {
            using (var trn = db.Database.BeginTransaction())
            {
                db.Blogs.Add(new Blog { Url = "http://msdn.com/data/ef" });
                db.Blogs.Add(new Blog { Url = "http://blogs.msdn.com/adonet" });
                db.SaveChanges();

                db.Blogs.Add(new Blog { Url = "http://twitter.com/efmagicunicorns" });
                db.SaveChanges();

                trn.Commit();
            }
        }
    });

MyConfiguration.SuspendExecutionStrategy = false;
```  
