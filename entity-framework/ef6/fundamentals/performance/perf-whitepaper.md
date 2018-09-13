---
title: Considerazioni sulle prestazioni per EF4 EF5 ed EF6
author: divega
ms.date: 10/23/2016
ms.assetid: d6d5a465-6434-45fa-855d-5eb48c61a2ea
ms.openlocfilehash: a58461a6d18d9d53c002b5d45cecbff7b0cdf81e
ms.sourcegitcommit: 2b787009fd5be5627f1189ee396e708cd130e07b
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/13/2018
ms.locfileid: "45490259"
---
# <a name="performance-considerations-for-ef-4-5-and-6"></a>Considerazioni sulle prestazioni per Entity Framework 4, 5 e 6
David Obando, Eric Dettinger e ad altri utenti

Data di pubblicazione: Aprile 2012

Ultimo aggiornamento: maggio 2014

------------------------------------------------------------------------

## <a name="1-introduction"></a>1. Introduzione

Framework di Mapping relazionale a oggetti sono un modo pratico per fornire un'astrazione per l'accesso ai dati in un'applicazione orientata agli oggetti. Per le applicazioni .NET, consigliata da Microsoft CHE O/RM è Entity Framework. Tuttavia, con qualsiasi livello di astrazione delle prestazioni possono costituire un problema.

Questo white paper è stato scritto per mostrare le considerazioni sulle prestazioni durante lo sviluppo di applicazioni con Entity Framework, per offrire agli sviluppatori un'idea degli algoritmi interni di Entity Framework che possono influire sulle prestazioni e fornire suggerimenti per l'analisi e miglioramento delle prestazioni nelle applicazioni che usano Entity Framework. Esistono alcuni argomenti buona sulle prestazioni già disponibili sul web e abbiamo provato anche che puntano a queste risorse dove possibile.

Le prestazioni sono una questione spinosa. Questo white paper è concepito come una risorsa per aiutare si apportano le prestazioni relative decisioni per le applicazioni che usano Entity Framework. Abbiamo incluso alcune metriche di test per illustrare le prestazioni, ma queste metriche non sono da intendersi come assoluto degli indicatori delle prestazioni che verrà visualizzato nell'applicazione.

Ai fini pratici, questo documento si presuppone che Entity Framework 4 viene eseguita in .NET 4.0 ed Entity Framework 5 e 6 vengono eseguite in .NET 4.5. Molti dei miglioramenti delle prestazioni per Entity Framework 5 si trovano all'interno di componenti di base fornite con .NET 4.5.

Entity Framework 6 è un rilascio fuori banda fuori e non dipende dai componenti di Entity Framework forniti con .NET. Entity Framework 6 si cimentano sia .NET 4.0 e .NET 4.5 e può offrire un vantaggio significativo delle prestazioni per gli utenti non hanno eseguito l'aggiornamento da .NET 4.0, ma vuole che i bit più recenti di Entity Framework nella propria applicazione. Quando questo documento vengono citate Entity Framework 6, fa riferimento alla versione più recente disponibile al momento della stesura di questo articolo: versione 6.1.0.

## <a name="2-cold-vs-warm-query-execution"></a>2. Visual Studio ad accesso sporadico. Esecuzione di Query a caldo

La prima volta che le query viene eseguita in un determinato modello, Entity Framework esegue molte operazioni in background per caricare e convalidare il modello. Abbiamo fanno riferimento a questa prima query come query "fredda".  Altre query in base a un modello già caricato sono note come query "calda" e sono molto più veloci.

È possibile richiedere una visualizzazione generale di dove viene impiegato il tempo quando si esegue una query mediante Entity Framework e vedere in cui le cose stanno migliorando in Entity Framework 6.

**Prima esecuzione di Query, query ad accesso sporadico**

| Operazioni di scrittura di codice utente                                                                                     | Operazione                    | EF4 Impatto sulle prestazioni                                                                                                                                                                                                                                                                                                                                                                                                        | EF5 Impatto sulle prestazioni                                                                                                                                                                                                                                                                                                                                                                                                                                                    | Entity Framework 6 Impatto sulle prestazioni                                                                                                                                                                                                                                                                                                                                                                                                                                                       |
|:-----------------------------------------------------------------------------------------------------|:--------------------------|:------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|:--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|:-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| `using(var db = new MyContext())` <br/> `{`                                                          | Creazione rapida          | Medio                                                                                                                                                                                                                                                                                                                                                                                                                        | Medio                                                                                                                                                                                                                                                                                                                                                                                                                                                                    | Bassa                                                                                                                                                                                                                                                                                                                                                                                                                                                                          |
| `  var q1 = ` <br/> `    from c in db.Customers` <br/> `    where c.Id == id1` <br/> `    select c;` | Creazione di espressioni di query | Bassa                                                                                                                                                                                                                                                                                                                                                                                                                           | Bassa                                                                                                                                                                                                                                                                                                                                                                                                                                                                       | Bassa                                                                                                                                                                                                                                                                                                                                                                                                                                                                          |
| `  var c1 = q1.First();`                                                                             | Esecuzione di query LINQ      | -Caricamento Metadata: elevata ma memorizzati nella cache <br/> -Visualizzare generazione: potenzialmente molto alta ma memorizzati nella cache <br/> -Valutazione parametro: Medium <br/> -Conversione di query: Medium <br/> -Generazione materializzatore: Media ma memorizzati nella cache <br/> -Esecuzione di query del database: potenzialmente elevato <br/> + Venga <br/> + Command. ExecuteReader <br/> + DataReader. Read <br/> La materializzazione dell'oggetto: Medium <br/> -Ricerca identity: Medium | -Caricamento Metadata: elevata ma memorizzati nella cache <br/> -Visualizzare generazione: potenzialmente molto alta ma memorizzati nella cache <br/> -Valutazione parametro: bassa <br/> -Conversione di query: Media ma memorizzati nella cache <br/> -Generazione materializzatore: Media ma memorizzati nella cache <br/> -Esecuzione di query del database: potenzialmente elevato (più query in alcune situazioni) <br/> + Venga <br/> + Command. ExecuteReader <br/> + DataReader. Read <br/> La materializzazione dell'oggetto: Medium <br/> -Ricerca identity: Medium | -Caricamento Metadata: elevata ma memorizzati nella cache <br/> -Visualizzare generazione: Media ma memorizzati nella cache <br/> -Valutazione parametro: bassa <br/> -Conversione di query: Media ma memorizzati nella cache <br/> -Generazione materializzatore: Media ma memorizzati nella cache <br/> -Esecuzione di query del database: potenzialmente elevato (più query in alcune situazioni) <br/> + Venga <br/> + Command. ExecuteReader <br/> + DataReader. Read <br/> La materializzazione dell'oggetto: Medium (più veloce rispetto a EF5) <br/> -Ricerca identity: Medium |
| `}`                                                                                                  | Connection          | Bassa                                                                                                                                                                                                                                                                                                                                                                                                                           | Bassa                                                                                                                                                                                                                                                                                                                                                                                                                                                                       | Bassa                                                                                                                                                                                                                                                                                                                                                                                                                                                                          |


**Esecuzione della seconda Query – query a caldo**

| Operazioni di scrittura di codice utente                                                                                     | Operazione                    | EF4 Impatto sulle prestazioni                                                                                                                                                                                                                                                                                                                                                                                                                                                                            | EF5 Impatto sulle prestazioni                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                | Entity Framework 6 Impatto sulle prestazioni                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                   |
|:-----------------------------------------------------------------------------------------------------|:--------------------------|:--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|:------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|:---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| `using(var db = new MyContext())` <br/> `{`                                                          | Creazione rapida          | Medio                                                                                                                                                                                                                                                                                                                                                                                                                                                                                            | Medio                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                | Bassa                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                      |
| `  var q1 = ` <br/> `    from c in db.Customers` <br/> `    where c.Id == id1` <br/> `    select c;` | Creazione di espressioni di query | Bassa                                                                                                                                                                                                                                                                                                                                                                                                                                                                                               | Bassa                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                   | Bassa                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                      |
| `  var c1 = q1.First();`                                                                             | Esecuzione di query LINQ      | -Metadata ~~caricamento~~ lookup: ~~elevata ma memorizzati nella cache~~ bassa <br/> -Visualizzare ~~generazione~~ lookup: ~~potenzialmente molto alta ma memorizzati nella cache~~ bassa <br/> -Valutazione parametro: Medium <br/> : Esegue una query ~~traduzione~~ ricerca: Medium <br/> -Materializzatore ~~generazione~~ lookup: ~~Medium ma memorizzati nella cache~~ bassa <br/> -Esecuzione di query del database: potenzialmente elevato <br/> + Venga <br/> + Command. ExecuteReader <br/> + DataReader. Read <br/> La materializzazione dell'oggetto: Medium <br/> -Ricerca identity: Medium | -Metadata ~~caricamento~~ lookup: ~~elevata ma memorizzati nella cache~~ bassa <br/> -Visualizzare ~~generazione~~ lookup: ~~potenzialmente molto alta ma memorizzati nella cache~~ bassa <br/> -Valutazione parametro: bassa <br/> : Esegue una query ~~translation~~ lookup: ~~Medium ma memorizzati nella cache~~ bassa <br/> -Materializzatore ~~generazione~~ lookup: ~~Medium ma memorizzati nella cache~~ bassa <br/> -Esecuzione di query del database: potenzialmente elevato (più query in alcune situazioni) <br/> + Venga <br/> + Command. ExecuteReader <br/> + DataReader. Read <br/> La materializzazione dell'oggetto: Medium <br/> -Ricerca identity: Medium | -Metadata ~~caricamento~~ lookup: ~~elevata ma memorizzati nella cache~~ bassa <br/> -Visualizzare ~~generazione~~ lookup: ~~Medium ma memorizzati nella cache~~ bassa <br/> -Valutazione parametro: bassa <br/> : Esegue una query ~~translation~~ lookup: ~~Medium ma memorizzati nella cache~~ bassa <br/> -Materializzatore ~~generazione~~ lookup: ~~Medium ma memorizzati nella cache~~ bassa <br/> -Esecuzione di query del database: potenzialmente elevato (più query in alcune situazioni) <br/> + Venga <br/> + Command. ExecuteReader <br/> + DataReader. Read <br/> La materializzazione dell'oggetto: Medium (più veloce rispetto a EF5) <br/> -Ricerca identity: Medium |
| `}`                                                                                                  | Connection          | Bassa                                                                                                                                                                                                                                                                                                                                                                                                                                                                                               | Bassa                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                   | Bassa                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                      |


Esistono diversi modi per ridurre l'impatto sulle prestazioni delle query sia ad accesso frequente e meno attivi e prenderemo in esame tali nella sezione seguente. In particolare, esamineremo riducendo il costo del modello il caricamento nelle query ad accesso sporadico usando le visualizzazioni pregenerate, che consentono di risolvere problematiche delle prestazioni esperti durante la generazione di visualizzazioni. Per le query a caldo, illustreremo la memorizzazione nella cache piano di query, nessuna query di rilevamento e le opzioni di esecuzione di query diversi.

### <a name="21-what-is-view-generation"></a>2.1 What ' s generazione di viste?

Per comprendere quale visualizzazione viene generazione, è innanzitutto necessario conoscere quali sono "Mapping di viste". Le visualizzazioni di mapping sono rappresentazioni eseguibili delle trasformazioni specificate nel mapping per tutti i set di entità e associazioni. Internamente, queste visualizzazioni mapping assumere la forma di CQTs (strutture ad albero query canoniche). Esistono due tipi di visualizzazioni dei mapping:

-   Eseguire query sulle viste: rappresentano le trasformazioni necessarie per passare dallo schema del database al modello concettuale.
-   Aggiornare le viste: rappresentano le trasformazioni necessarie per passare dal modello concettuale allo schema del database.

Tenere presente che il modello concettuale potrebbe essere diverso dallo schema del database in vari modi. Ad esempio, una singola tabella potrebbe essere utilizzata per archiviare i dati per due tipi di entità diversi. Ereditarietà e i mapping non semplice svolgono un ruolo in base alla complessità delle viste di mapping.

Il processo di elaborazione di tali visualizzazioni in base alla specifica del mapping è ciò che chiamiamo generazione di visualizzazioni. Generazione di visualizzazioni può sia avvenire in modo dinamico quando viene caricato un modello o in fase di compilazione usando "visualizzazioni pregenerate"; quest'ultimo viene serializzato sotto forma di istruzioni Entity SQL a C\# o file di Visual Basic.

Quando vengono generate le visualizzazioni, essi vengono anche convalidati. Dal punto di vista delle prestazioni, la maggior parte del costo di generazione di visualizzazioni è effettivamente la convalida delle visualizzazioni che assicura che le connessioni tra le entità ha senso e avere la cardinalità corretta per tutte le operazioni supportate.

Quando viene eseguita una query su un set di entità, la query viene combinata con la visualizzazione di query corrispondente, e il risultato di questa composizione di viene eseguito tramite il compilatore di piano per creare la rappresentazione della query che può comprendere l'archivio di backup. Per SQL Server, il risultato finale di questa compilazione è un'istruzione T-SQL SELECT. La prima volta che viene eseguito un aggiornamento su un set di entità, la visualizzazione di aggiornamento viene eseguita tramite una procedura simile per trasformarlo in istruzioni DML per il database di destinazione.

### <a name="22-factors-that-affect-view-generation-performance"></a>2.2 fattori che influiscono sulle prestazioni di generazione di visualizzazioni

Le prestazioni del passaggio di generazione di visualizzazione dipende non solo la dimensione del modello ma anche il modello è la modalità interconnesse. Se due entità sono connessi tramite una catena di ereditarietà o un'associazione, essi sono chiamati gruppi connessi. Allo stesso modo se le due tabelle sono connesse tramite una chiave esterna, sono connessi. Come aumenta il numero di connessa entità e tabelle negli schemi, la generazione di visualizzazioni costo aumenta.

L'algoritmo che viene usato per generare e convalidare le viste è esponenziale nel peggiore dei casi, anche se usiamo alcune ottimizzazioni per migliorare questa soglia. Di seguito sono riportati i fattori principali che sembrano influire negativamente sulle prestazioni:

-   Dimensione del modello, che fa riferimento al numero di entità e la quantità di associazioni tra queste entità.
-   Complessità del modello, in particolare dell'ereditarietà che include un numero elevato di tipi.
-   Utilizzo di associazioni indipendenti, invece di associazioni di chiavi esterne.

Per i modelli di piccole dimensioni, semplici il costo può essere sufficientemente ridotto per evitare di non dover usando le visualizzazioni pregenerate. Come aumentare la dimensione del modello e la complessità, esistono diverse opzioni disponibili per ridurre il costo di convalida e generazione di visualizzazioni.

### <a name="23-using-pre-generated-views-to-decrease-model-load-time"></a>Il tempo di caricamento 2,3 mediante Pre-Generated le viste per ridurre i modelli

#### <a name="231-pre-generated-views-using-the-entity-framework-power-tools"></a>2.3.1 le visualizzazioni pregenerate tramite Entity Framework Power Tools

È anche possibile utilizzare Entity Framework Power Tools per generare visualizzazioni di file EDMX e Code First modelli facendo clic sul file della classe modello e utilizzare il menu di Entity Framework per selezionare "Genera visualizzazioni". Entity Framework Power Tools funzionano solo sui contesti di DbContext derivata ed è reperibile in \< http://visualstudiogallery.msdn.microsoft.com/72a60b14-1581-4b9b-89f2-846072eff19d>.

Per altre informazioni su come usare le visualizzazioni pregenerate su Entity Framework 6 visita [Pre-Generated Mapping viste](~/ef6/fundamentals/performance/pre-generated-views.md).

#### <a name="232-how-to-use-pre-generated-views-with-a-model-created-by-edmgen"></a>2.3.2 come usare le visualizzazioni pregenerate con un modello creato da EDMGen

EDMGen è un'utilità che viene fornito con .NET e funziona con Entity Framework 4 e 5, ma non con Entity Framework 6. EDMGen consente di generare un file di modello, il livello di oggetti e le viste dalla riga di comando. Uno degli output sarà un file di visualizzazioni nel tuo linguaggio preferito, Visual Basic o C\#. Si tratta di un file di codice contenente frammenti Entity SQL per ogni set di entità. Per abilitare le visualizzazioni pregenerate, è sufficiente includere il file nel progetto.

Se si apportano manualmente le modifiche ai file di schema per il modello, è necessario generare di nuovo il file di viste. È possibile farlo eseguendo EDMGen con il **/mode:ViewGeneration** flag.

Per altre informazioni, vedere [procedura: Pre-Generate viste per migliorare le prestazioni delle Query](https://msdn.microsoft.com/library/bb896240.aspx).

#### <a name="233-how-to-use-pre-generated-views-with-an-edmx-file"></a>2.3.3 come usare le visualizzazioni Pre-Generated con un file EDMX

È inoltre possibile utilizzare EDMGen per generare visualizzazioni per un file EDMX: l'argomento di cui viene fatto riferimento in precedenza MSDN viene descritto come aggiungere un evento di pre-compilazione per eseguire questa operazione - ma questo è complicato e vi sono casi in cui non è possibile. È in genere più semplice usare un modello T4 per generare le viste quando il modello è in un file edmx.

Blog del team di ADO.NET è una richiesta post che descrive come usare un modello T4 per la generazione di visualizzazioni ( \< http://blogs.msdn.com/b/adonet/archive/2008/06/20/how-to-use-a-t4-template-for-view-generation.aspx>). Questo post include un modello che può essere scaricato e aggiunti al progetto. Il modello è stato scritto per la prima versione di Entity Framework, in modo che non è garantite per lavorare con le versioni più recenti di Entity Framework. Tuttavia, è possibile scaricare un set più aggiornato dei modelli di generazione di visualizzazione per Entity Framework 4 e 5from Visual Studio Gallery:

-   VB.NET: \<http://visualstudiogallery.msdn.microsoft.com/118b44f2-1b91-4de2-a584-7a680418941d>
-   C\#: \<http://visualstudiogallery.msdn.microsoft.com/ae7730ce-ddab-470f-8456-1b313cd2c44d>

Se si usa Entity Framework 6 è possibile ottenere la visualizzazione generazione T4 i modelli Visual Studio Gallery all'indirizzo \< http://visualstudiogallery.msdn.microsoft.com/18a7db90-6705-4d19-9dd1-0a6c23d0751f>.

#### <a name="234-how-to-use-pre-generated-views-with-a-code-first-model"></a>2.3.4 come usare Pre-Generated viste con un modello Code First

È anche possibile usare le visualizzazioni pregenerate con un progetto di Code First. Entity Framework Power Tools è in grado di generare un file di visualizzazioni per il progetto Code First. È possibile trovare Entity Framework Power Tools in Visual Studio Gallery al \< http://visualstudiogallery.msdn.microsoft.com/72a60b14-1581-4b9b-89f2-846072eff19d/>.

### <a name="24-reducing-the-cost-of-view-generation"></a>2.4 riducendo i costi di generazione di visualizzazioni

Usando le visualizzazioni pregenerate sposta il costo della generazione di visualizzazioni dal caricamento del modello (fase di esecuzione) in fase di compilazione. Anche se ciò migliora le prestazioni di avvio in fase di esecuzione, si continuerà a verificarsi i problemi della generazione di visualizzazioni mentre si sta sviluppando. Esistono alcuni suggerimenti aggiuntivi che consentono di ridurre il costo della generazione di visualizzazioni, sia in fase di compilazione e fase di esecuzione.

#### <a name="241-using-foreign-key-associations-to-reduce-view-generation-cost"></a>2.4.1 utilizzando associazioni di chiavi esterne per ridurre i costi di generazione di visualizzazione

Abbiamo visto un numero di case in cambio le associazioni nel modello da associazioni indipendenti per associazioni di chiavi esterne notevolmente migliorato il tempo impiegato nella generazione di visualizzazioni.

Per illustrare questo miglioramento, viene generato due versioni del modello Navision utilizzando EDMGen. *Nota: seeappendix Cfor una descrizione del modello Navision.* Il modello Navision è interessante per questo esercizio a causa di un relativo quantità molto elevata di entità e relazioni tra di essi.

È stata generata una versione di questo modello di dimensioni molto grande con associazioni di chiavi esterne e l'altro è stato generato con associazioni indipendenti. Abbiamo quindi programmato il tempo impiegato per generare le visualizzazioni per ogni modello. Test Framework5 entità utilizzato il metodo GenerateViews() dalla classe EntityViewGenerator per generare le visualizzazioni, mentre il test di Entity Framework 6 usato il metodo GenerateViews() dalla classe StorageMappingItemCollection sia. Ciò a causa di un codice ristrutturazione che si sono verificati nella codebase Entity Framework 6.

Usa Entity Framework 5, generazione di visualizzazioni per il modello con le chiavi esterne sono necessari 65 minuti in un computer di laboratorio. Non è noto quanto tempo ci sarebbero voluti per generare le visualizzazioni per il modello utilizzato associazioni indipendenti. È stato lasciato il test in esecuzione per più di un mese prima che il computer è stato riavviato nel nostro laboratorio per installare gli aggiornamenti mensili.

Con Entity Framework 6, generazione di visualizzazioni per il modello con le chiavi esterne ha impiegato 28 secondi nello stesso computer lab. Generazione di visualizzazioni per il modello che utilizza associazioni indipendenti impiegato 58 secondi. I miglioramenti di fine per Entity Framework 6 nel relativo codice di generazione di visualizzazione significa che molti progetti non sarà necessario le visualizzazioni pregenerate per ottenere tempi di avvio.

È importante osservazione pregenerazione di visualizzazioni in Entity Framework 4 e 5 possono essere eseguite con EDMGen o Entity Framework Power Tools. Per la visualizzazione di Entity Framework 6 generazione può essere eseguita tramite Entity Framework Power Tools o a livello di programmazione come descritto in [Pre-Generated Mapping viste](~/ef6/fundamentals/performance/pre-generated-views.md).

##### <a name="2411-how-to-use-foreign-keys-instead-of-independent-associations"></a>2.4.1.1 come usare le chiavi esterne anziché associazioni indipendenti

Quando si utilizza EDMGen o Entity Designer in Visual Studio, si ottengono le chiavi esterne per impostazione predefinita, e sono necessari solo un singolo flag casella di controllo o della riga di comando per passare tra le chiavi esterne e IAs.

Se si dispone di un modello Code First di grandi dimensioni, con associazioni indipendenti avrà lo stesso effetto sulla generazione di visualizzazione. Per evitare questo impatto, incluse le proprietà di chiave esterna per le classi per gli oggetti dipendenti, anche se alcuni sviluppatori prenderà in considerazione questa opzione per essere possano contaminare relativo modello a oggetti. È possibile trovare altre informazioni su questo argomento in \< http://blog.oneunicorn.com/2011/12/11/whats-the-deal-with-mapping-foreign-keys-using-the-entity-framework/>.

| Quando si usa      | Eseguire questa operazione                                                                                                                                                                                                                                                                                                                              |
|:----------------|:-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Finestra di progettazione entità | Dopo l'aggiunta di un'associazione tra due entità, assicurarsi di avere un vincolo referenziale. I vincoli referenziali indicano a Entity Framework di usare le chiavi esterne anziché associazioni indipendenti. Per altri dettagli, visitare \< http://blogs.msdn.com/b/efdesign/archive/2009/03/16/foreign-keys-in-the-entity-framework.aspx>. |
| EDMGen          | Quando si utilizza EDMGen per generare i file dal database, verrà rispettate e aggiunte al modello di conseguenza le chiavi esterne. Per altre informazioni sulle differenti opzioni esposte da EDMGen visita [ http://msdn.microsoft.com/library/bb387165.aspx ](https://msdn.microsoft.com/library/bb387165.aspx).                           |
| Code First      | Vedere la sezione "Relazione convenzione" i [convenzioni del primo codice](~/ef6/modeling/code-first/conventions/built-in.md) argomento per informazioni su come includere proprietà di chiave esterna per oggetti dipendenti quando si utilizza Code First.                                                                                              |

#### <a name="242-moving-your-model-to-a-separate-assembly"></a>2.4.2 lo spostamento del modello in un assembly separato

Quando il modello è inclusa direttamente nel progetto dell'applicazione e si generano visualizzazioni tramite un evento di pre-compilazione o un modello T4, convalida e generazione di visualizzazioni avrà luogo ogni volta che il progetto viene ricompilato, anche se il modello non è stato modificato. Se si sposta il modello in un assembly separato e farvi riferimento dal progetto dell'applicazione, è possibile apportare altre modifiche all'applicazione senza la necessità di ricompilare il progetto contenente il modello.

*Nota:* quando si passa il modello per separare gli assembly ricordare di copiare le stringhe di connessione per il modello nel file di configurazione dell'applicazione del progetto client.

#### <a name="243-disable-validation-of-an-edmx-based-model"></a>2.4.3 disabilitare la convalida di un modello basato su edmx

Modelli EDMX vengono convalidati in fase di compilazione, anche se il modello rimane invariato. Se il modello è già stato convalidato, è possibile eliminare la convalida in fase di compilazione impostando la proprietà "Convalidare nella Build" su false nella finestra Proprietà. Quando si modifica il mapping o un modello, è possibile temporaneamente abilitare nuovamente la convalida per verificare le modifiche.

Si noti che sono stati apportati miglioramenti delle prestazioni per la finestra di progettazione di Entity Framework per Entity Framework 6 e il costo della "convalida su Build" è molto minore rispetto a versioni precedenti della finestra di progettazione.

## <a name="3-caching-in-the-entity-framework"></a>La memorizzazione nella cache 3 di Entity Framework

Entity Framework presenta le seguenti forme di memorizzazione nella cache predefinite:

1.  La memorizzazione nella cache dell'oggetto: ObjectStateManager incorporato in un'istanza di ObjectContext tiene traccia in memoria degli oggetti che sono state recuperate mediante tale istanza. Ciò è noto anche come cache di primo livello.
2.  Caching di piano di query - riutilizzando il comando di archiviazione generato quando una query viene eseguita più volte.
3.  Metadati di memorizzazione nella cache, i metadati per un modello di condivisione tra le diverse connessioni allo stesso modello.

Oltre alla cache che Entity Framework fornisce per impostazione predefinita, un tipo speciale di provider di dati ADO.NET, noto come un provider di ritorno a capo può essere usato anche per estendere Entity Framework con una cache per i risultati recuperati dal database, noto anche come la memorizzazione nella cache di secondo livello.

### <a name="31-object-caching"></a>3.1 oggetti nella cache

Per impostazione predefinita quando nei risultati di una query, viene restituita un'entità poco prima che Entity Framework consente di materializzare, ObjectContext controllerà se un'entità con la stessa chiave è già stata caricata nel relativo ObjectStateManager. Se è già presente un'entità con le stesse chiavi EF verranno includerlo nei risultati della query. Anche se EF genererà comunque la query sul database, questo comportamento può ignorare la maggior parte del costo di materializzazione di entità più volte.

#### <a name="311-getting-entities-from-the-object-cache-using-dbcontext-find"></a>3.1.1 ottenere entità dalla cache degli oggetti tramite la ricerca di DbContext

A differenza di una query normali, il metodo Find nella DbSet (API incluse per la prima volta in Entity Framework 4.1) eseguirà una ricerca in memoria prima di rilasciare anche le query sul database. È importante notare che due istanze diverse di ObjectContext avrà due istanze diverse di ObjectStateManager, vale a dire che hanno le cache di oggetti distinta.

Ricerca Usa il valore della chiave primaria per tentare di trovare un'entità rilevata dal contesto. Se l'entità non è presente nel contesto quindi verrà eseguita e valutare rispetto al database una query e viene restituito null se l'entità non viene trovato nel contesto o nel database. Si noti che Find restituisce anche le entità che sono stati aggiunti al contesto, ma non sono ancora state salvate nel database.

È una considerazione sulle prestazioni da intraprendere quando si usa Cerca. Le chiamate a questo metodo per impostazione predefinita verranno attivata una convalida della cache oggetti per rilevare le modifiche che sono ancora in attesa di commit nel database. Questo processo può risultare dispendioso se sono presenti un numero molto elevato di oggetti nella cache degli oggetti o in un grafico di oggetti di grandi dimensioni da aggiungere alla cache degli oggetti, ma può anche essere disabilitato. In alcuni casi, si potrebbero percepire su un ordine di grandezza della differenza tra la chiamata di trova il metodo quando si disabilita automaticamente di rilevare le modifiche. Ancora un secondo ordine di grandezza viene percepito quando l'oggetto è effettivamente nella cache rispetto a quando l'oggetto deve essere recuperato dal database. Ecco un esempio di grafico con misurazioni effettuate utilizzando alcuni dei nostri microbenchmarks, espresso in millisecondi, con un carico di 5000 entità:

![.NET 4.5 scala logaritmica](~/ef6/media/net45logscale.png ".NET 4.5 - scala logaritmica")

Esempio di ricerca con rilevamento automatico delle modifiche disabilitato:

``` csharp
    context.Configuration.AutoDetectChangesEnabled = false;
    var product = context.Products.Find(productId);
    context.Configuration.AutoDetectChangesEnabled = true;
    ...
```

Ciò che è necessario prendere in considerazione quando si usa il metodo Find è:

1.  Se l'oggetto non è presente nella cache sono negare i vantaggi della ricerca, ma la sintassi è ancora più semplice rispetto a una query dalla chiave.
2.  Se il rilevamento automatico delle modifiche è abilitato il costo di metodo Find può aumentare per un ordine di grandezza o ancora più a seconda della complessità del modello e la quantità di entità nella cache oggetto.

Inoltre, tenere presente che solo trovare restituisce l'entità a cui che si sta cercando e appare automaticamente carica le entità associate se non sono già nella cache degli oggetti. Se si desidera recuperare le entità associate, è possibile utilizzare una query con chiave con il caricamento eager. Per altre informazioni vedere **8.1 Visual Studio il caricamento Lazy. Il caricamento eager**.

#### <a name="312-performance-issues-when-the-object-cache-has-many-entities"></a>3.1.2 problemi relativi alle prestazioni di quando la cache oggetti ha molte entità

La cache degli oggetti consente di aumentare la velocità di risposta complessiva di Entity Framework. Tuttavia, quando la cache oggetti ha una quantità molto elevata di entità caricata che può influire sulle determinate operazioni, ad esempio Aggiungi, Rimuovi, trovare, voce, SaveChanges e altro ancora. In particolare, operazioni che attivano una chiamata a DetectChanges saranno essere influenzate negativamente dalla cache degli oggetti grandi. DetectChanges Sincronizza l'oggetto grafico con la gestione dello stato degli oggetti e che le prestazioni determinato direttamente dalla dimensione dell'oggetto grafico. Per altre informazioni sulle DetectChanges, vedere [rilevamento delle modifiche nelle entità POCO](https://msdn.microsoft.com/library/dd456848.aspx).

Quando si usa Entity Framework 6, gli sviluppatori sono in grado di chiamare AddRange e RemoveRange direttamente su un elemento DbSet, anziché l'iterazione di una raccolta e la chiamata a Add una volta per ogni istanza. Il vantaggio di usare i metodi di intervallo è che il costo di DetectChanges soltanto una volta per l'intero set di entità anziché una volta per ogni entità aggiunto.

### <a name="32-query-plan-caching"></a>3.2 eseguire una query la memorizzazione nella cache di piano

La prima volta che viene eseguita una query, la chiamata passa attraverso il compilatore piano interna per tradurre le query concettuale il comando di archiviazione (ad esempio T-SQL che viene eseguito se viene eseguito in SQL Server).  Se è abilitata la memorizzazione nella cache piano di query, la volta successiva che la query viene eseguita nell'archivio comando viene recuperato direttamente dalla cache dei piani di query per l'esecuzione, ignorando il compilatore di piano.

Cache dei piani di query viene condiviso tra le istanze di ObjectContext all'interno dello stesso AppDomain. Non è necessario mantenere un'istanza di ObjectContext per trarre vantaggio dalla memorizzazione nella cache piano di query.

#### <a name="321-some-notes-about-query-plan-caching"></a>3.2.1 alcune note sulle Query prevede la memorizzazione nella cache

-   Cache dei piani di query viene condiviso per tutti i tipi di query: Entity SQL, LINQ to Entities e gli oggetti CompiledQuery.
-   Per impostazione predefinita, la memorizzazione nella cache piano di query è abilitata per le query Entity SQL, se è stata eseguita tramite un EntityCommand o tramite un ObjectQuery. Viene abilitata anche per impostazione predefinita per LINQ alle query di entità in Entity Framework in .NET 4.5 in Entity Framework 6
    -   Memorizzazione nella cache piano di query può essere disabilitata impostando la proprietà EnablePlanCaching (su EntityCommand o ObjectQuery) su false. Ad esempio:
``` csharp
                    var query = from customer in context.Customer
                                where customer.CustomerId == id
                                select new
                                {
                                    customer.CustomerId,
                                    customer.Name
                                };
                    ObjectQuery oQuery = query as ObjectQuery;
                    oQuery.EnablePlanCaching = false;
```
-   Per le query con parametri, modificare il valore del parametro ancora raggiungerà la query memorizzato nella cache. Ma modifica i facet del parametro (ad esempio, dimensioni, precisione o scala) destinati a raggiungere una diversa voce nella cache.
-   Quando si usa Entity SQL, la stringa di query è parte della chiave. Se si modifica la query affatto comporterà voci della cache diverso, anche se le query sono funzionalmente equivalenti. Ciò include le modifiche di maiuscole e minuscole o uno spazio vuoto.
-   Quando si utilizza LINQ, la query viene elaborata per generare una parte della chiave. Modifica dell'espressione LINQ pertanto genererà una chiave diversa.
-   Possono applicare altri limiti tecnici; per altre informazioni, vedere autocompilati query.

#### <a name="322------cache-eviction-algorithm"></a>3.2.2 algoritmo di rimozione della cache

Informazioni sul modo in cui il funzionamento dell'algoritmo interno consentirà di capire quando per abilitare o disabilitare query piano di memorizzazione nella cache. L'algoritmo di pulizia è come segue:

1.  Una volta che la cache contiene un numero di set di voci (800), si avvia un timer che periodicamente (una volta al minuto) organizza la cache.
2.  Durante le operazioni della cache, vengono rimosse le voci nella cache in un LFRU (meno di frequente, usato di recente) base. Questo algoritmo accetta sia il numero di passaggi e age in considerazione quando si decide quale le voci vengono espulsi.
3.  Alla fine di ogni durante lo sweep di cache, la cache contiene anche in questo caso le voci di 800.

Tutte le voci della cache vengono considerate ugualmente quando si determina quali voci da rimuovere. Ciò significa che il comando di archiviazione per un CompiledQuery ha la stessa probabilità di rimozione del comando di archiviazione per una query Entity SQL.

Si noti che il timer di eliminazione della cache viene avviato quando sono presenti 800 entità nella cache, ma la cache è sweep solo 60 secondi dopo l'avvio del timer. Ciò significa che fino a 60 secondi può raggiungere la cache sia di grandi dimensioni.

#### <a name="323-------test-metrics-demonstrating-query-plan-caching-performance"></a>3.2.3 le metriche che illustra il piano di query la memorizzazione nella cache delle prestazioni dei test

Per illustrare l'effetto del piano di query la cache per le prestazioni dell'applicazione, abbiamo eseguito un test in cui viene eseguito un numero di query Entity SQL rispetto al modello Navision. Vedere l'appendice per una descrizione del modello Navision e i tipi di query, che sono stati eseguiti. In questo test, è innanzitutto scorrere l'elenco delle query ed eseguite una volta ciascuno per aggiungerli alla cache (se è abilitata la memorizzazione nella cache). Questo passaggio è untimed. Successivamente, abbiamo sospensione il thread principale per oltre 60 secondi per consentire cache iperparametri per avvengono; Infine, viene iterato più il tempo di elenco un 2nd per eseguire le query memorizzate nella cache. Inoltre, egli cache dei piani di SQL Server viene scaricato prima che ogni set di query viene eseguita in modo che i tempi che si ottengono con precisione riflettono il vantaggio dato dalla cache dei piani di query.

##### <a name="3231-------test-results"></a>3.2.3.1 i risultati dei test

| Test                                                                   | EF5 Nessuna cache | EF5 memorizzato nella cache | EF6 Nessuna cache | EF6 memorizzato nella cache |
|:-----------------------------------------------------------------------|:-------------|:-----------|:-------------|:-----------|
| L'enumerazione di tutte le 18723 query                                          | 124          | 125.4      | 124.3        | 125.3      |
| Come evitare durante lo sweep (solo primi 800 query, indipendentemente dalla complessità)  | 41.7         | 5.5        | 40,5         | 5.4        |
| Solo le query AggregatingSubtotals (178 totale, che consente di evitare durante lo sweep) | 39,5         | 4.5        | 38,1         | 4.6        |

*Tutte le volte in secondi.*

Morale - durante l'esecuzione di numerose delle query distinct (ad esempio, in modo dinamico creato query), la memorizzazione nella cache non è stato utile e risultante lo svuotamento della cache per mantenere le query in grado di sfruttare le potenzialità del piano di memorizzazione nella cache da effettivamente usando.

Le query AggregatingSubtotals sono più complesse delle query che è stato testato con. Come previsto, è la query più complessa, maggiore sarà il vantaggio dalla memorizzazione nella cache piano di query verrà visualizzato.

Poiché un CompiledQuery è davvero una query LINQ con il piano memorizzato nella cache, il confronto di un CompiledQuery e l'equivalente nella query Entity SQL deve avere risultati simili. Infatti, se un'app ha un numero elevato di query Entity SQL dinamico, il riempimento della cache con le query anche determinerà in effetti CompiledQueries "decompilare" quando essi vengono scaricati dalla cache. In questo scenario, potrebbero migliorare le prestazioni, disabilitare la memorizzazione nella cache sulle query dinamiche per definire le priorità di CompiledQueries. Ancora meglio, naturalmente, sarebbe necessario riscrivere l'app per usare le query con parametri invece di query dinamiche.

### <a name="33-using-compiledquery-to-improve-performance-with-linq-queries"></a>3.3 utilizzo CompiledQuery per migliorare le prestazioni con le query LINQ

I test indicano che usando CompiledQuery può portare un vantaggio del 7% autocompilati le query LINQ. Ciò significa che è perdere 7% tempo per l'esecuzione di codice dallo stack di Entity Framework; Questo significa che l'applicazione sarà 7% più velocemente. In generale, il costo di scrivere e gestire oggetti CompiledQuery in Entity Framework 5.0 non può essere utile la briga rispetto ai vantaggi. Il chilometraggio può variare, prestare pertanto questa opzione se il progetto richiede il push aggiuntivo. Si noti che CompiledQueries solo siano compatibili con i modelli di ObjectContext derivato e non è compatibile con i modelli di DbContext derivata.

Per altre informazioni su come creare e richiamare un CompiledQuery, vedere [query compilate (LINQ to Entities)](https://msdn.microsoft.com/library/bb896297.aspx).

Esistono due aspetti che è necessario eseguire quando si usa un CompiledQuery, vale a dire la necessità di usare le istanze statiche e i problemi che hanno con la possibilità di composizione. Qui di seguito una spiegazione dettagliata di questi due aspetti.

#### <a name="331-------use-static-compiledquery-instances"></a>3.3.1 usare istanze CompiledQuery statiche

Poiché la compilazione di una query LINQ è un processo che richiedono molto tempo, non vogliamo eseguire questa operazione ogni volta che è necessario recuperare i dati dal database. Le istanze CompiledQuery consentono di compilare una sola volta ed eseguire più volte, ma è necessario prestare attenzione e procurare per riutilizzare la stessa istanza CompiledQuery ogni volta anziché compilarlo in modo continuativo. L'uso di membri statici per archiviare le istanze CompiledQuery diventa necessario; in caso contrario, non vedrete alcun vantaggio.

Si supponga, ad esempio, che la pagina contiene il corpo del metodo seguente per gestire la visualizzazione dei prodotti per la categoria selezionata:

``` csharp
    // Warning: this is the wrong way of using CompiledQuery
    using (NorthwindEntities context = new NorthwindEntities())
    {
        string selectedCategory = this.categoriesList.SelectedValue;

        var productsForCategory = CompiledQuery.Compile<NorthwindEntities, string, IQueryable<Product>>(
            (NorthwindEntities nwnd, string category) =>
                nwnd.Products.Where(p => p.Category.CategoryName == category)
        );

        this.productsGrid.DataSource = productsForCategory.Invoke(context, selectedCategory).ToList();
        this.productsGrid.DataBind();
    }

    this.productsGrid.Visible = true;
```

In questo caso, si creerà una nuova istanza CompiledQuery in tempo reale ogni volta che viene chiamato il metodo. Recuperando il comando di archiviazione dalla cache dei piani di query, anziché i vantaggi delle prestazioni di CompiledQuery passeranno attraverso il compilatore piano ogni volta che viene creata una nuova istanza. Infatti, si verrà contaminare la cache dei piani di query con una nuova voce CompiledQuery ogni volta che viene chiamato il metodo.

Al contrario, si desidera creare un'istanza statica della query compilate in modo che la stessa query compilata viene richiamato ogni volta che viene chiamato il metodo. Un modo per questo è aggiungendo l'istanza CompiledQuery come membro del contesto di oggetto.  È quindi possibile apportare cose così una piccola accedendo il CompiledQuery tramite un metodo helper:

``` csharp
    public partial class NorthwindEntities : ObjectContext
    {
        private static readonly Func<NorthwindEntities, string, IEnumerable<Product>> productsForCategoryCQ = CompiledQuery.Compile(
            (NorthwindEntities context, string categoryName) =>
                context.Products.Where(p => p.Category.CategoryName == categoryName)
            );

        public IEnumerable<Product> GetProductsForCategory(string categoryName)
        {
            return productsForCategoryCQ.Invoke(this, categoryName).ToList();
        }
```

Questo metodo di supporto potrebbe essere richiamato, come segue:

``` csharp
    this.productsGrid.DataSource = context.GetProductsForCategory(selectedCategory);
```

#### <a name="332-------composing-over-a-compiledquery"></a>3.3.2 la composizione in un CompiledQuery

La possibilità di comporre tutte le query LINQ è estremamente utile; a tale scopo, è sufficiente richiamare un metodo dopo all'oggetto IQueryable, ad esempio *Skip* oppure *Count ()*. Tuttavia, svolgendo così restituisce un nuovo oggetto IQueryable. Mentre non c'è nulla impedisce di tecnicamente composizione tramite un CompiledQuery, questa operazione causerà la generazione di un nuovo oggetto IQueryable che richiede anche in questo caso passando attraverso il compilatore di piano.

Alcuni componenti useranno IQueryable composti gli oggetti per abilitare la funzionalità avanzata. Ad esempio, ASP. Controllo GridView di NET può essere associato a dati a un oggetto IQueryable tramite la proprietà SelectMethod. GridView verrà quindi creato tramite questo oggetto IQueryable per consentire l'ordinamento e paging tramite il modello di dati. Come può notare, usando un CompiledQuery per GridView non raggiungeva la query compilata ma genera una nuova query autocompilati.

Il Team di consulenza clienti è descritta nell'articolo i post di blog "Potenziali prestazioni problemi con compilato LINQ Query viene ricompilato": <http://blogs.msdn.com/b/appfabriccat/archive/2010/08/06/potential-performance-issues-with-compiled-linq-query-re-compiles.aspx>.

Un'unica posizione in cui è possibile riscontrare questo è quando si aggiungono filtri progressivi per una query. Ad esempio, si supponga una pagina di clienti con diversi elenchi a discesa per i filtri facoltativi (ad esempio, paese e OrdersCount). È possibile comporre questi filtri sui risultati di una CompiledQuery IQueryable, ma tale operazione genererà una nuova query passare attraverso il compilatore piano ogni volta che la relativa esecuzione.

``` csharp
    using (NorthwindEntities context = new NorthwindEntities())
    {
        IQueryable<Customer> myCustomers = context.InvokeCustomersForEmployee();

        if (this.orderCountFilterList.SelectedItem.Value != defaultFilterText)
        {
            int orderCount = int.Parse(orderCountFilterList.SelectedValue);
            myCustomers = myCustomers.Where(c => c.Orders.Count > orderCount);
        }

        if (this.countryFilterList.SelectedItem.Value != defaultFilterText)
        {
            myCustomers = myCustomers.Where(c => c.Address.Country == countryFilterList.SelectedValue);
        }

        this.customersGrid.DataSource = myCustomers;
        this.customersGrid.DataBind();
    }
```

 Per evitare la ricompilazione, è possibile riscrivere il CompiledQuery per tener conto delle possibili filtri:

``` csharp
    private static readonly Func<NorthwindEntities, int, int?, string, IQueryable<Customer>> customersForEmployeeWithFiltersCQ = CompiledQuery.Compile(
        (NorthwindEntities context, int empId, int? countFilter, string countryFilter) =>
            context.Customers.Where(c => c.Orders.Any(o => o.EmployeeID == empId))
            .Where(c => countFilter.HasValue == false || c.Orders.Count > countFilter)
            .Where(c => countryFilter == null || c.Address.Country == countryFilter)
        );
```

Che verranno richiamati nell'interfaccia utente, ad esempio:

``` csharp
    using (NorthwindEntities context = new NorthwindEntities())
    {
        int? countFilter = (this.orderCountFilterList.SelectedIndex == 0) ?
            (int?)null :
            int.Parse(this.orderCountFilterList.SelectedValue);

        string countryFilter = (this.countryFilterList.SelectedIndex == 0) ?
            null :
            this.countryFilterList.SelectedValue;

        IQueryable<Customer> myCustomers = context.InvokeCustomersForEmployeeWithFilters(
                countFilter, countryFilter);

        this.customersGrid.DataSource = myCustomers;
        this.customersGrid.DataBind();
    }
```

 Un buon compromesso qui è il comando di archiviazione generato avrà sempre i filtri con i controlli null, ma devono essere abbastanza semplice per il server di database da ottimizzare:

``` SQL
...
WHERE ((0 = (CASE WHEN (@p__linq__1 IS NOT NULL) THEN cast(1 as bit) WHEN (@p__linq__1 IS NULL) THEN cast(0 as bit) END)) OR ([Project3].[C2] > @p__linq__2)) AND (@p__linq__3 IS NULL OR [Project3].[Country] = @p__linq__4)
```

### <a name="34-metadata-caching"></a>3.4 la memorizzazione nella cache dei metadati

Entity Framework supporta anche la memorizzazione nella cache dei metadati. Questo è essenzialmente la memorizzazione nella cache di informazioni sul tipo e informazioni sul mapping di tipo-database tra diverse connessioni allo stesso modello. La cache dei metadati è univoca per ogni dominio applicazione.

#### <a name="341-metadata-caching-algorithm"></a>3.4.1 algoritmo di memorizzazione nella cache dei metadati

1.  Informazioni sui metadati per un modello viene archiviati in una classe ItemCollection per ciascun EntityConnection.
    -   Come nota a margine, esistono diversi oggetti ItemCollection per le diverse parti del modello. Ad esempio, StoreItemCollections contiene le informazioni sul modello di database. ObjectItemCollection contiene informazioni sul modello di dati; EdmItemCollection contiene informazioni sul modello concettuale.

2.  Se due connessioni utilizzano la stessa stringa di connessione, condividono la stessa istanza ItemCollection.
3.  Le stringhe di connessione funzionalmente equivalenti ma testualmente diverse possono comportare le cache di metadati diversi. È suddividere le stringhe di connessione, semplicemente modificando l'ordine dei token dovrebbe restituire metadati condivisa. Tuttavia, due stringhe di connessione che sembrano funzionalmente identici non possono essere valutate come identici dopo la suddivisione in token.
4.  ItemCollection viene periodicamente verificata per l'uso. Se è stato stabilito che un'area di lavoro non eseguito di recente, si verrà contrassegnato per la pulizia in avanti sweep cache.
5.  La semplice creazione di un EntityConnection causerà una cache dei metadati da creare (anche se le raccolte di elementi in esso non verrà inizializzate fino a quando non viene aperta la connessione). Questa area di lavoro rimane in memoria fino a quando non determina l'algoritmo di memorizzazione nella cache non è "in uso".

Il Team di consulenza clienti ha scritto un post di blog che descrive conterrà un riferimento a un oggetto ItemCollection per evitare "deprecation" quando si usano modelli di grandi dimensioni: \< http://blogs.msdn.com/b/appfabriccat/archive/2010/10/22/metadataworkspace-reference-in-wcf-services.aspx>.

#### <a name="342-the-relationship-between-metadata-caching-and-query-plan-caching"></a>3.4.2 la relazione tra i metadati e pianificare la memorizzazione nella cache di Query

L'istanza di cache piano di query si trova in ItemCollection dell'elemento MetadataWorkspace di tipi di archivio. Ciò significa che comandi di archiviazione memorizzati nella cache verranno utilizzati per le query in qualsiasi contesto creata un'istanza con un determinato MetadataWorkspace. Significa anche che se si dispone di due stringhe di connessione che sono leggermente diversi e non corrispondono dopo la suddivisione in token, sarà necessario diverse query prevede le istanze di cache.

### <a name="35-results-caching"></a>3.5 comporta la memorizzazione nella cache

Con i risultati di memorizzazione nella cache (noto anche come "second-level la memorizzazione nella cache"), mantenere i risultati delle query in una cache locale. Quando si emette una query, prima di tutto verificare se i risultati sono disponibili in locale prima di query a fronte dell'archivio. Anche se la memorizzazione nella cache i risultati non sono direttamente supportati da Entity Framework, è possibile aggiungere una cache di secondo livello con un provider di ritorno a capo. Un esempio di provider di ritorno a capo con una cache di secondo livello è dell'Alachisoft [Cache di secondo livello di Entity Framework basato su NCache](http://www.alachisoft.com/ncache/entity-framework.html).

Questa implementazione della memorizzazione nella cache di secondo livello è una funzionalità inserita che ha luogo dopo che è stata valutata l'espressione LINQ (la funcletized) e il piano di esecuzione di query viene calcolato o recuperato dalla cache di primo livello. La cache di secondo livello solo i risultati non elaborati del database, quindi Archivia in modo che la pipeline di materializzazione viene comunque eseguita in un secondo momento.

#### <a name="351-additional-references-for-results-caching-with-the-wrapping-provider"></a>3.5.1 ulteriori riferimenti per i risultati la memorizzazione nella cache con il provider di ritorno a capo

-   Julie Lerman ha scritto un articolo di MSDN "Second-Level la memorizzazione nella cache in Entity Framework e Windows Azure" che include la procedura per aggiornare il provider di ritorno a capo di esempio per usare la memorizzazione nella cache di Windows Server AppFabric: [https://msdn.microsoft.com/magazine/hh394143.aspx](https://msdn.microsoft.com/magazine/hh394143.aspx)
-   Se si lavora con Entity Framework 5, il blog del team dispone di una richiesta post che descrive come eseguire le operazioni in esecuzione con il provider di memorizzazione nella cache per Entity Framework 5: \< http://blogs.msdn.com/b/adonet/archive/2010/09/13/ef-caching-with-jarek-kowalski-s-provider.aspx>. Include inoltre un modello T4 che facilitano l'automazione aggiungendo il 2 ° a livello di memorizzazione nella cache al progetto.

## <a name="4-autocompiled-queries"></a>4 autocompilati query

Quando viene eseguita una query su un database tramite Entity Framework, deve passare attraverso una serie di passaggi prima della materializzazione effettivamente i risultati. una tale operazione è la compilazione di Query. Query Entity SQL sono stati noti per avere buone prestazioni poiché essi vengono automaticamente memorizzate nella cache, pertanto il tempo secondo o il terzo è eseguire la stessa query può ignorare il compilatore di piano e usare invece il piano memorizzato nella cache.

Entity Framework 5 è stato introdotto memorizzazione automatica nella cache per LINQ alle query di entità anche. Nelle edizioni precedenti di Entity Framework la creazione di un CompiledQuery per velocizzare le prestazioni era pratica comune, come questo renderebbe LINQ per eseguire query di entità inseribili nella cache. Poiché la memorizzazione nella cache viene eseguita automaticamente senza l'uso di un CompiledQuery, questa funzionalità viene chiamato "autocompilati queries". Per altre informazioni sulla cache dei piani di query e i relativi meccanismi, vedere la memorizzazione nella cache di piano di Query.

Entity Framework rileva se in una query richiede la ricompilazione, e non esegue questa operazione quando la query viene richiamata anche se si fosse stato compilato in precedenza. Condizioni comuni che causano la query venga ricompilato sono:

-   Modifica il MergeOption associata alla query. La query memorizzato nella cache non verrà utilizzata, invece il compilatore piano verrà eseguite di nuovo e viene memorizzato nella cache il piano appena creato.
-   Modifica del valore di ContextOptions.UseCSharpNullComparisonBehavior. Si ottiene lo stesso effetto modifica il MergeOption.

Altre condizioni possono impedire l'uso della cache di query. Esempi comuni sono:

-   Usando IEnumerable&lt;T&gt;. Contiene&lt;&gt;(valore di T).
-   Utilizzo di funzioni che generano query con costanti.
-   Usando le proprietà di un oggetto non mappati.
-   La query di collegamento a un'altra query che richiede la ricompilazione.

### <a name="41-using-ienumerablelttgtcontainslttgtt-value"></a>4.1 usando IEnumerable&lt;T&gt;. Contiene&lt;T&gt;(valore di T)

Entity Framework non memorizza nella cache le query che richiamano IEnumerable&lt;T&gt;. Contiene&lt;T&gt;(valore di T) su una raccolta in memoria, poiché i valori della raccolta sono considerati volatili. La query di esempio seguente non verrà memorizzata, in modo che verrà sempre elaborata dal compilatore piano:

``` csharp
int[] ids = new int[10000];
...
using (var context = new MyContext())
{
    var query = context.MyEntities
                    .Where(entity => ids.Contains(entity.Id));

    var results = query.ToList();
    ...
}
```

Si noti che viene eseguita la dimensione di IEnumerable contro cui Contains determina la velocità con cui o come rallentamento della query viene compilata. Le prestazioni potrebbero diminuire in modo significativo quando si usano le raccolte di grandi dimensioni come quella illustrata nell'esempio precedente.

Entity Framework 6 contiene ottimizzazioni al modo in cui IEnumerable&lt;T&gt;. Contiene&lt;T&gt;(valore di T) funziona quando le query vengono eseguite. Il codice SQL generato è molto più veloce per produrre e più leggibile e nella maggior parte dei casi vengono anche eseguite rapidamente nel server.

### <a name="42-using-functions-that-produce-queries-with-constants"></a>4.2 usando le funzioni che generano query con costanti

Gli operatori Skip, Take (,) Contains () e LINQ DefautIfEmpty() non producono le query SQL con parametri, ma invece inserire i valori vengano passati come costanti. Per questo motivo, le query che potrebbero altrimenti essere identici finisce possano contaminare la query, cache dei piani nello stack di Entity Framework sia nel server di database e ottenere reutilized non a meno che non vengono utilizzate le costanti stesse durante l'esecuzione di query successiva. Ad esempio:

``` csharp
var id = 10;
...
using (var context = new MyContext())
{
    var query = context.MyEntities.Select(entity => entity.Id).Contains(id);

    var results = query.ToList();
    ...
}
```

In questo esempio, ogni volta che questa query viene eseguita con un valore diverso per l'id della query verrà compilato in un nuovo piano.

In particolare attenzione all'uso di Skip e Take quando si esegue il paging. In EF6 questi metodi hanno un overload del lambda in modo da rendere il piano di query memorizzati nella cache riutilizzabili perché EF è possibile acquisire variabili passate a questi metodi e convertirle in SQLparameters. Questo contribuirà a mantenere la cache più chiaro perché in caso contrario, ogni query con una costante diversa da Skip and Take otterrebbe la propria voce della cache piano di query.

Si consideri il codice seguente, che non è ottimale, ma è destinato esclusivamente a illustrare relativi di questa classe di query:

``` csharp
var customers = context.Customers.OrderBy(c => c.LastName);
for (var i = 0; i < count; ++i)
{
    var currentCustomer = customers.Skip(i).FirstOrDefault();
    ProcessCustomer(currentCustomer);
}
```

Una versione più veloce di questo stesso codice comporta la chiamata di Skip con un'espressione lambda:

``` csharp
var customers = context.Customers.OrderBy(c => c.LastName);
for (var i = 0; i \< count; ++i)
{
    var currentCustomer = customers.Skip(() => i).FirstOrDefault();
    ProcessCustomer(currentCustomer);
}
```

Il secondo frammento di codice possono essere eseguite fino a 11% più veloce perché lo stesso piano di query viene utilizzato ogni volta che viene eseguita la query, che consente di risparmiare tempo di CPU ed è possibile evitare di contaminare la cache delle query. Inoltre, perché il parametro Skip in una chiusura il codice potrebbe essere anche simile a questo punto:

``` csharp
var i = 0;
var skippyCustomers = context.Customers.OrderBy(c => c.LastName).Skip(() => i);
for (; i < count; ++i)
{
    var currentCustomer = skippyCustomers.FirstOrDefault();
    ProcessCustomer(currentCustomer);
}
```

### <a name="43-using-the-properties-of-a-non-mapped-object"></a>4.3 usando le proprietà di un oggetto non mappati

Quando una query Usa le proprietà di un tipo di oggetto non mappato come un parametro, quindi la query verrà non ottenere memorizzato nella cache. Ad esempio:

``` csharp
using (var context = new MyContext())
{
    var myObject = new NonMappedType();

    var query = from entity in context.MyEntities
                where entity.Name.StartsWith(myObject.MyProperty)
                select entity;

   var results = query.ToList();
    ...
}
```

In questo esempio, si supponga che classe NonMappedType non fa parte del modello di entità. Questa query può essere modificata facilmente per non usare un tipo non mappati, usare invece una variabile locale come parametro alla query:

``` csharp
using (var context = new MyContext())
{
    var myObject = new NonMappedType();
    var myValue = myObject.MyProperty;
    var query = from entity in context.MyEntities
                where entity.Name.StartsWith(myValue)
                select entity;

    var results = query.ToList();
    ...
}
```

In questo caso, la query sarà in grado di ottenere memorizzato nella cache e trarranno vantaggio dalla cache dei piani di query.

### <a name="44-linking-to-queries-that-require-recompiling"></a>4.4 il collegamento per le query che richiedono la ricompilazione

In seguito lo stesso esempio precedente, se si dispone di una seconda query si basa su una query che deve essere ricompilato, l'intera query secondo anche essere ricompilata. Ecco un esempio per illustrare questo scenario:

``` csharp
int[] ids = new int[10000];
...
using (var context = new MyContext())
{
    var firstQuery = from entity in context.MyEntities
                        where ids.Contains(entity.Id)
                        select entity;

    var secondQuery = from entity in context.MyEntities
                        where firstQuery.Any(otherEntity => otherEntity.Id == entity.Id)
                        select entity;

    var results = secondQuery.ToList();
    ...
}
```

L'esempio è generico, ma illustra come collegamento a firstQuery causa secondQuery non venga memorizzata. Se firstQuery non fosse stata una query che richiede la ricompilazione, quindi secondQuery sarebbe inseribile nella cache.

## <a name="5-notracking-queries"></a>5 NoTracking query

### <a name="51-disabling-change-tracking-to-reduce-state-management-overhead"></a>5.1 la disabilitazione di rilevamento delle modifiche per ridurre il sovraccarico di gestione dello stato

Se si desidera evitare l'overhead del caricamento di oggetti in ObjectStateManager sono in uno scenario di sola lettura, è possibile eseguire query "Rilevamento di n".  È possibile disabilitare il rilevamento delle modifiche a livello di query.

Si noti tuttavia che disabilitando il rilevamento di modifiche consentono in modo efficace di disattivare la cache degli oggetti. Quando esegue una query per un'entità, è non è possibile ignorare la proprietà materialization spostando i risultati della query materializzato in precedenza da ObjectStateManager. Se si cercano ripetutamente le stesse entità nello stesso contesto, è possibile visualizzare effettivamente un prestazioni trarre vantaggio dall'abilitazione del rilevamento delle modifiche.

Quando si esegue una query tramite ObjectContext, istanze ObjectQuery e ObjectSet memorizzerà una MergeOption dopo che è impostato, e le query che sono composte da su di essi erediterà il MergeOption effettivo della query principale. Quando si usa DbContext, rilevamento può essere disabilitato tramite una chiamata il modificatore AsNoTracking() alla classe DbSet.

#### <a name="511-disabling-change-tracking-for-a-query-when-using-dbcontext"></a>5.1.1 la disabilitazione di rilevamento delle modifiche per una query quando si usa DbContext

È possibile attivare la modalità di una query di NoTracking concatenando una chiamata al metodo AsNoTracking() nella query. A differenza di ObjectQuery, le classi DbSet e DbQuery nell'API DbContext non hanno una proprietà modificabile per il MergeOption.

``` csharp
    var productsForCategory = from p in context.Products.AsNoTracking()
                                where p.Category.CategoryName == selectedCategory
                                select p;


```

#### <a name="512-disabling-change-tracking-at-the-query-level-using-objectcontext"></a>5.1.2 Disabilitazione a livello di query tramite ObjectContext di rilevamento delle modifiche

``` csharp
    var productsForCategory = from p in context.Products
                                where p.Category.CategoryName == selectedCategory
                                select p;

    ((ObjectQuery)productsForCategory).MergeOption = MergeOption.NoTracking;
```

#### <a name="513-disabling-change-tracking-for-an-entire-entity-set-using-objectcontext"></a>5.1.3 la disabilitazione di rilevamento delle modifiche per un'intera entità impostata tramite ObjectContext

``` csharp
    context.Products.MergeOption = MergeOption.NoTracking;

    var productsForCategory = from p in context.Products
                                where p.Category.CategoryName == selectedCategory
                                select p;
```

### <a name="52-test-metrics-demonstrating-the-performance-benefit-of-notracking-queries"></a>5.2 verificare metriche di dimostrare il miglioramento delle prestazioni delle query NoTracking

In questo test vogliamo aumentando tuttavia il riempimento ObjectStateManager confrontando il rilevamento per le query NoTracking per il modello Navision. Vedere l'appendice per una descrizione del modello Navision e i tipi di query, che sono stati eseguiti. In questo test, vengono scorrere l'elenco delle query e si eseguono ognuna una sola volta. È stato eseguito due varianti del test, una volta con query NoTracking e una volta con l'opzione di merge predefinita di "AppendOnly". È stata eseguita ogni variante 3 volte ed eseguire il valore medio delle esecuzioni. Tra i test si cancella la cache delle query in SQL Server e ridurre il tempdb eseguendo i comandi seguenti:

1.  DBCC DROPCLEANBUFFERS
2.  DBCC FREEPROCCACHE
3.  DBCC SHRINKDATABASE (tempdb, 0)

Testare i risultati, viene eseguito più di 3 mediano:

|                        | NESSUN RILEVAMENTO – WORKING SET | NESSUN RILEVAMENTO – ORA | AGGIUNGERE SOLO – WORKING SET | AGGIUNGERE SOLO: ORA |
|:-----------------------|:--------------------------|:-------------------|:--------------------------|:-------------------|
| **Entity Framework 5** | 460361728                 | 1163536 ms         | 596545536                 | 1273042 ms         |
| **Entity Framework 6** | 647127040                 | 190228 ms          | 832798720                 | 195521 ms          |

Entity Framework 5 avrà un footprint di memoria minore alla fine dell'esecuzione rispetto a Entity Framework 6. La memoria aggiuntiva utilizzata da Entity Framework 6 è il risultato di strutture di memoria aggiuntiva e il codice che abilita le nuove funzionalità e prestazioni migliori.

È inoltre disponibile una netta differenza footprint di memoria quando si utilizza ObjectStateManager. Entity Framework 5 aumentato il footprint del 30% quando tiene traccia di tutte le entità che si materializzati dal database. Entity Framework 6 aumentato il footprint di 28% in questo caso.

In termini di tempo, Entity Framework 6 ha prestazioni migliori rispetto Entity Framework 5 in questo test con un margine di grandi dimensioni. Entity Framework 6 completato il test in circa 16% del tempo utilizzato da Entity Framework 5. Inoltre, Entity Framework 5 richiede più di % 9 tempo per il completamento quando viene utilizzato il ObjectStateManager. In confronto, Entity Framework 6 Usa più tempo quando si utilizza ObjectStateManager di % 3.

## <a name="6-query-execution-options"></a>Opzioni di esecuzione di query 6

Entity Framework offre diversi modi per eseguire query. Verrà esaminare le opzioni seguenti, confrontare i vantaggi e svantaggi di ognuna ed esaminare le caratteristiche delle prestazioni:

-   LINQ to Entities.
-   Nessun rilevamento LINQ to Entities.
-   Entity SQL su un oggetto ObjectQuery.
-   Entity SQL su un EntityCommand.
-   ExecuteStoreQuery.
-   SqlQuery.
-   CompiledQuery.

### <a name="61-------linq-to-entities-queries"></a>6.1 query LINQ to Entities

``` csharp
var q = context.Products.Where(p => p.Category.CategoryName == "Beverages");
```

**Professionisti**

-   Adatto per le operazioni CUD.
-   Oggetti materializzati completamente.
-   Più semplici da scrivere con sintassi incorporata nel linguaggio di programmazione.
-   Buone prestazioni.

**Svantaggi**

-   Alcune limitazioni tecniche, ad esempio:
    -   Modelli di utilizzo di DefaultIfEmpty per le query con OUTER JOIN generare query più complesse rispetto alle semplici istruzioni OUTER JOIN in Entity SQL.
    -   Non è possibile usare LIKE e la generale criteri di ricerca.

### <a name="62-------no-tracking-linq-to-entities-queries"></a>6.2 alcun rilevamento LINQ alle query di entità

Quando il contesto deriva ObjectContext:

``` csharp
context.Products.MergeOption = MergeOption.NoTracking;
var q = context.Products.Where(p => p.Category.CategoryName == "Beverages");
```

Quando il contesto deriva DbContext:

``` csharp
var q = context.Products.AsNoTracking()
                        .Where(p => p.Category.CategoryName == "Beverages");
```

**Professionisti**

-   Miglioramento delle prestazioni su query LINQ normali.
-   Oggetti materializzati completamente.
-   Più semplici da scrivere con sintassi incorporata nel linguaggio di programmazione.

**Svantaggi**

-   Non è adatto per le operazioni CUD.
-   Alcune limitazioni tecniche, ad esempio:
    -   Modelli di utilizzo di DefaultIfEmpty per le query con OUTER JOIN generare query più complesse rispetto alle semplici istruzioni OUTER JOIN in Entity SQL.
    -   Non è possibile usare LIKE e la generale criteri di ricerca.

Si noti che le query che le proprietà scalari del progetto non vengono registrate anche se il NoTracking non è specificato. Ad esempio:

``` csharp
var q = context.Products.Where(p => p.Category.CategoryName == "Beverages").Select(p => new { p.ProductName });
```

Questa particolare query non specifica in modo esplicito da NoTracking, ma dal momento che non è la materializzazione non viene rilevato un tipo che è noto al gestore degli stati di oggetti, il risultato materializzato.

### <a name="63-------entity-sql-over-an-objectquery"></a>6.3 entity SQL su un oggetto ObjectQuery

``` csharp
ObjectQuery<Product> products = context.Products.Where("it.Category.CategoryName = 'Beverages'");
```

**Professionisti**

-   Adatto per le operazioni CUD.
-   Oggetti materializzati completamente.
-   Supporta query piano la memorizzazione nella cache.

**Svantaggi**

-   Implica le stringhe di query testuale che sono più soggette a errori rispetto a costrutti delle query integrati nel linguaggio.

### <a name="64-------entity-sql-over-an-entity-command"></a>6.4 entity SQL su un comando di entità

``` csharp
EntityCommand cmd = eConn.CreateCommand();
cmd.CommandText = "Select p From NorthwindEntities.Products As p Where p.Category.CategoryName = 'Beverages'";

using (EntityDataReader reader = cmd.ExecuteReader(CommandBehavior.SequentialAccess))
{
    while (reader.Read())
    {
        // manually 'materialize' the product
    }
}
```

**Professionisti**

-   Supporta query piano la memorizzazione nella cache in .NET 4.0 (piano di memorizzazione nella cache è supportato da tutti gli altri tipi di query in .NET 4.5).

**Svantaggi**

-   Implica le stringhe di query testuale che sono più soggette a errori rispetto a costrutti delle query integrati nel linguaggio.
-   Non è adatto per le operazioni CUD.
-   I risultati non vengono materializzati automaticamente e devono essere letto dal lettore di dati.

### <a name="65-------sqlquery-and-executestorequery"></a>6.5 SqlQuery ed ExecuteStoreQuery

SqlQuery sul Database:

``` csharp
// use this to obtain entities and not track them
var q1 = context.Database.SqlQuery<Product>("select * from products");
```

SqlQuery su DbSet:

``` csharp
// use this to obtain entities and have them tracked
var q2 = context.Products.SqlQuery("select * from products");
```

ExecyteStoreQuery:

``` csharp
var beverages = context.ExecuteStoreQuery<Product>(
@"     SELECT        P.ProductID, P.ProductName, P.SupplierID, P.CategoryID, P.QuantityPerUnit, P.UnitPrice, P.UnitsInStock, P.UnitsOnOrder, P.ReorderLevel, P.Discontinued, P.DiscontinuedDate
       FROM            Products AS P INNER JOIN Categories AS C ON P.CategoryID = C.CategoryID
       WHERE        (C.CategoryName = 'Beverages')"
);
```

**Professionisti**

-   In genere prestazioni più veloci perché il compilatore di piano viene ignorato.
-   Oggetti materializzati completamente.
-   Adatto per le operazioni CUD quando vengono usati da alla classe DbSet.

**Svantaggi**

-   Si tratta di una query di testo e tendente all'errore.
-   Query è associato a un back-end specifico usando la semantica di archivio anziché semantica.
-   Quando l'ereditarietà è presente, query artigianali deve tenere conto delle condizioni di mapping per il tipo richiesto.

### <a name="66-------compiledquery"></a>6.6 CompiledQuery

``` csharp
private static readonly Func<NorthwindEntities, string, IQueryable<Product>> productsForCategoryCQ = CompiledQuery.Compile(
    (NorthwindEntities context, string categoryName) =>
        context.Products.Where(p => p.Category.CategoryName == categoryName)
        );
…
var q = context.InvokeProductsForCategoryCQ("Beverages");
```

**Professionisti**

-   Offre fino a un miglioramento delle prestazioni di % 7 tramite query LINQ normali.
-   Oggetti materializzati completamente.
-   Adatto per le operazioni CUD.

**Svantaggi**

-   Una maggiore complessità e costi generali di programmazione.
-   Il miglioramento delle prestazioni viene perso quando la composizione all'inizio di una query compilata.
-   Alcune query LINQ non possono essere scritti come un CompiledQuery - ad esempio, le proiezioni di tipi anonimi.

### <a name="67-------performance-comparison-of-different-query-options"></a>6.7 confronto delle prestazioni di opzioni query diverse

Sono stati inseriti microbenchmarks semplice in cui è non stata programmata la creazione del contesto al test. È stata misurata l'esecuzione di query 5000 volte per un set di entità non in cache in un ambiente controllato. Questi numeri vengono considerati con un messaggio di avviso: non rappresentano i valori effettivi prodotti da un'applicazione, ma sono invece una misura molto accurata della quantità di una differenza nelle prestazioni quando vengono confrontati con le opzioni di query diverse mele con mele, escluso il costo della creazione di un nuovo contesto.

| EF  | Test                                 | Tempo (ms) | Memoria   |
|:----|:-------------------------------------|:----------|:---------|
| EF5 | ESQL ObjectContext                   | 2414      | 38801408 |
| EF5 | Query Linq di ObjectContext             | 2692      | 38277120 |
| EF5 | Nessun rilevamento di Query Linq di DbContext     | 2818      | 41840640 |
| EF5 | Query Linq DbContext                 | 2930      | 41771008 |
| EF5 | Nessun rilevamento di Query Linq di ObjectContext | 3013      | 38412288 |
|     |                                      |           |          |
| EF6 | ESQL ObjectContext                   | 2059      | 46039040 |
| EF6 | Query Linq di ObjectContext             | 3074      | 45248512 |
| EF6 | Nessun rilevamento di Query Linq di DbContext     | 3125      | 47575040 |
| EF6 | Query Linq DbContext                 | 3420      | 47652864 |
| EF6 | Nessun rilevamento di Query Linq di ObjectContext | 3593      | 45260800 |

![EF5 micro benchmark, 5000 iterazioni warm](~/ef6/media/ef5micro5000warm.png)

![EF6 micro benchmark, 5000 iterazioni warm](~/ef6/media/ef6micro5000warm.png)

Microbenchmarks sono molto sensibili a piccole variazioni nel codice. In questo caso, la differenza tra i costi di Entity Framework 5 ed Entity Framework 6 sono a causa dell'aggiunta dei [intercettazione](~/ef6/fundamentals/logging-and-interception.md) e [transazionali miglioramenti](~/ef6/saving/transactions.md). Questi numeri microbenchmarks, tuttavia, sono una visione amplificata, in un frammento di dimensioni molto ridotta delle operazioni eseguite da Entity Framework. Scenari reali di query a caldo non dovrebbe essere una regressione delle prestazioni durante l'aggiornamento da Entity Framework 5 per Entity Framework 6.

Per confrontare le prestazioni reali delle opzioni di query diversi, abbiamo creato 5 variazioni di test separato in cui si usa un'opzione di query diversi per selezionare tutti i prodotti il cui nome di categoria è "Bibite". Ogni iterazione include il costo della creazione del contesto e il costo di materializzazione di entità restituite tutto. 10 iterazioni vengono eseguite untimed prima di portare la somma dei timeout 1000 iterazioni. I risultati illustrati sono l'esecuzione medio impiegato da 5 esecuzioni di ogni test. Per altre informazioni, vedere l'appendice B che include il codice per il test.

| EF  | Test                                        | Tempo (ms) | Memoria   |
|:----|:--------------------------------------------|:----------|:---------|
| EF5 | Comando entità ObjectContext                | 621       | 39350272 |
| EF5 | DbContext Query Sql sul Database             | 825       | 37519360 |
| EF5 | Query Store di ObjectContext                   | 878       | 39460864 |
| EF5 | Nessun rilevamento di Query Linq di ObjectContext        | 969       | 38293504 |
| EF5 | ObjectContext Entity Sql con Query di oggetto | 1089      | 38981632 |
| EF5 | Query compilate ObjectContext                | 1099      | 38682624 |
| EF5 | Query Linq di ObjectContext                    | 1152      | 38178816 |
| EF5 | Nessun rilevamento di Query Linq di DbContext            | 1208      | 41803776 |
| EF5 | Query Sql DbContext su DbSet                | 1414      | 37982208 |
| EF5 | Query Linq DbContext                        | 1574      | 41738240 |
|     |                                             |           |          |
| EF6 | Comando entità ObjectContext                | 480       | 47247360 |
| EF6 | Query Store di ObjectContext                   | 493       | 46739456 |
| EF6 | DbContext Query Sql sul Database             | 614       | 41607168 |
| EF6 | Nessun rilevamento di Query Linq di ObjectContext        | 684       | 46333952 |
| EF6 | ObjectContext Entity Sql con Query di oggetto | 767       | 48865280 |
| EF6 | Query compilate ObjectContext                | 788       | 48467968 |
| EF6 | Nessun rilevamento di Query Linq di DbContext            | 878       | 47554560 |
| EF6 | Query Linq di ObjectContext                    | 953       | 47632384 |
| EF6 | Query Sql DbContext su DbSet                | 1023      | 41992192 |
| EF6 | Query Linq DbContext                        | 1290      | 47529984 |


![Iterazioni warm query 1000 EF5](~/ef6/media/ef5warmquery1000.png)

![Iterazioni warm query 1000 EF6](~/ef6/media/ef6warmquery1000.png)

> [!NOTE]
> Per motivi di completezza, sono state incluse una variante in cui viene eseguita una query Entity SQL su un EntityCommand. Tuttavia, poiché i risultati non vengono materializzati per tali query, il confronto non è necessariamente mele con mele. Il test include un'approssimazione molto vicina al materializzazione per provare a effettuare il confronto equa.

In questo caso end-to-end, Entity Framework 6 ha prestazioni migliori rispetto Entity Framework 5 a causa di miglioramenti delle prestazioni ottenuti in diverse parti dello stack, inclusa un'inizializzazione DbContext molto più chiaro e più veloci MetadataCollection&lt;T&gt; ricerche.

## <a name="7-design-time-performance-considerations"></a>Considerazioni sulle prestazioni di tempo progettazione 7

### <a name="71-------inheritance-strategies"></a>7.1 strategie di ereditarietà

Un'altra considerazione sulle prestazioni quando si usa Entity Framework è la strategia di ereditarietà. Entity Framework supporta 3 tipi di base di ereditarietà e le relative combinazioni:

-   Tabella per gerarchia (TPH), in cui ogni ereditarietà impostato esegue il mapping a una tabella con una colonna discriminatore per indicare quale tipo specifico nella gerarchia è rappresentato nella riga.
-   Tabella per ogni tipo TPT (), in cui ogni tipo ha la propria tabella nel database. le tabelle figlio definiscono solo le colonne che non contiene la tabella padre.
-   Tabella per ogni classe (TP), in cui ogni tipo ha una proprio completa della tabella nel database. le tabelle figlio definiscono tutti i relativi campi, inclusi quelli definiti nei tipi padre.

Se il modello utilizza TPT (ereditarietà), le query che vengono generate saranno più complesse rispetto a quelli che vengono generati con le altre strategie di ereditarietà, potrebbe essere in tempi di esecuzione sull'archivio.  In genere richiederà più tempo per generare le query su un modello di tabella per tipo e per materializzare gli oggetti risultanti.

Vedere "Considerazioni sulle prestazioni quando si usa l'ereditarietà tabella per tipo (tabella per tipo) in Entity Framework" post di blog MSDN: \< http://blogs.msdn.com/b/adonet/archive/2010/08/17/performance-considerations-when-using-tpt-table-per-type-inheritance-in-the-entity-framework.aspx>.

#### <a name="711-------avoiding-tpt-in-model-first-or-code-first-applications"></a>7.1.1 evitando TPT nelle applicazioni Code First o Model First

Quando si crea un modello in un database esistente che dispone di uno schema di tabella per tipo, non è necessario numerose opzioni. Tuttavia, quando si crea un'applicazione con Code First o Model First, è opportuno evitare TPT (ereditarietà) per evitare problemi di prestazioni.

Quando si usa il primo modello nella creazione guidata finestra di progettazione entità, si otterrà TPT per qualsiasi tipo di ereditarietà nel modello. Se si desidera passare a una strategia di ereditarietà tabella per gerarchia con Model First, è possibile usare "Entity Designer Database generazione Power Pack" disponibile da Visual Studio Gallery ( \< http://visualstudiogallery.msdn.microsoft.com/df3541c3-d833-4b65-b942-989e7ec74c87/>).

Quando si utilizza Code First per configurare il mapping di un modello con ereditarietà, Entity Framework userà una tabella per gerarchia per impostazione predefinita, pertanto tutte le entità nella gerarchia di ereditarietà sarà possibile eseguire il mapping alla stessa tabella. Vedere la sezione "Mapping con l'API Fluent" dell'articolo "Code prima nell'entità Framework4.1" in MSDN Magazine ( [ http://msdn.microsoft.com/magazine/hh126815.aspx ](https://msdn.microsoft.com/magazine/hh126815.aspx)) per altri dettagli.

### <a name="72-------upgrading-from-ef4-to-improve-model-generation-time"></a>7.2 l'aggiornamento da EF4 per migliorare la generazione del modello di tempo

Un miglioramento delle specifiche di SQL Server per l'algoritmo che genera il livello di archivio (SSDL) del modello è disponibile in Entity Framework 5 e 6 e come aggiornamento a Entity Framework 4 quando viene installato Visual Studio 2010 SP1. I risultati dei test seguente illustrare il miglioramento durante la generazione di un modello di dimensioni molto grande, in questo caso il modello Navision. Per altre informazioni, vedere Appendice C.

Il modello contiene set di entità 1005 e 4227 set di associazioni.

| Configurazione                              | Suddivisione di tempo usato                                                                                                                                               |
|:-------------------------------------------|:-------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Visual Studio 2010, Entity Framework 4     | Generazione SSDL: 2 hr 27 min <br/> Generazione del mapping: 1 secondo <br/> Generazione di CSDL: 1 secondo <br/> Generazione ObjectLayer: 1 secondo <br/> Generazione di visualizzazioni: 2 ore e 14 minuti |
| Visual Studio 2010 SP1, Entity Framework 4 | Generazione SSDL: 1 secondo <br/> Generazione del mapping: 1 secondo <br/> Generazione di CSDL: 1 secondo <br/> Generazione ObjectLayer: 1 secondo <br/> Generazione di visualizzazioni: 1 ora 53 min   |
| Visual Studio 2013, Entity Framework 5     | Generazione SSDL: 1 secondo <br/> Generazione del mapping: 1 secondo <br/> Generazione di CSDL: 1 secondo <br/> Generazione ObjectLayer: 1 secondo <br/> Generazione di visualizzazioni: 65 minuti    |
| Visual Studio 2013, Entity Framework 6     | Generazione SSDL: 1 secondo <br/> Generazione del mapping: 1 secondo <br/> Generazione di CSDL: 1 secondo <br/> Generazione ObjectLayer: 1 secondo <br/> Generazione di visualizzazioni: 28 secondi.   |


Vale la pena notare che durante la generazione SSDL, il carico è quasi interamente dedicato a SQL Server, mentre è in attesa il computer di sviluppo client inattivo per ottenere risultati ritorno dal server. Gli amministratori di database deve particolarmente grati per questo miglioramento. È anche importante notare che fondamentalmente l'intero costo di generazione del modello non venga eseguita nella generazione di visualizzazioni a questo punto.

### <a name="73-------splitting-large-models-with-database-first-and-model-first"></a>7.3 suddividendo i modelli di grandi dimensioni con il Database prima di tutto e Model First

Man mano che aumenta le dimensioni del modello, nell'area di progettazione diventa risulti troppo affollato e difficili da usare. In genere, si consideri un modello con più di 300 entità troppo grande per l'utilizzo efficiente della finestra di progettazione. Post di blog seguente vengono descritte diverse opzioni per la divisione di modelli di grandi dimensioni: \< http://blogs.msdn.com/b/adonet/archive/2008/11/25/working-with-large-models-in-entity-framework-part-2.aspx>.

Il post è stato scritto per la prima versione di Entity Framework, ma i passaggi sono comunque applicabili.

### <a name="74-------performance-considerations-with-the-entity-data-source-control"></a>7.4 considerazioni sulle prestazioni con il controllo origine dati di entità

Abbiamo visto case nei test di stress e prestazioni multithreading in cui le prestazioni di un'applicazione web tramite il controllo EntityDataSource causa la riduzione in modo significativo. La causa principale è che EntityDataSource chiama ripetutamente MetadataWorkspace.LoadFromAssembly sugli assembly fa riferimento l'applicazione Web per individuare i tipi da utilizzare come entità.

La soluzione consiste nell'impostare il ContextTypeName di EntityDataSource per il nome del tipo della classe ObjectContext derivato. Ciò consente di disattivare il meccanismo che esegue l'analisi di tutti gli assembly di riferimento per i tipi di entità.

L'impostazione del campo ContextTypeName impedisce anche un problema funzionale in cui EntityDataSource in .NET 4.0 genera un'eccezione ReflectionTypeLoadException quando Impossibile caricare un tipo da un assembly tramite reflection. Questo problema è stato risolto in .NET 4.5.

### <a name="75-------poco-entities-and-change-tracking-proxies"></a>7.5 entità POCO e rilevamento delle modifiche

Entity Framework consente di utilizzare classi di dati personalizzate insieme al modello di dati senza apportare alcuna modifica alle classi di dati. Pertanto è possibile pertanto utilizzare oggetti POCO (Plain-Old CLR Object), ad esempio gli oggetti di dominio esistenti, con il modello di dati. Queste classi di dati POCO (noto anche come oggetti che non riconoscono la persistenza), che vengono eseguito il mapping a entità definite in un modello di dati, supportano la maggior parte della stessa query, inseriscono, aggiornano ed eliminare i comportamenti come tipi di entità generati dagli strumenti di Entity Data Model.

Entity Framework è anche possibile creare le classi proxy derivate dai tipi POCO, che vengono usati quando si desidera abilitare le funzionalità, ad esempio il caricamento lazy e automatica rilevamento delle modifiche nelle entità POCO. Le classi POCO devono soddisfare determinati requisiti per consentire a Entity Framework di usare un proxy, come descritto qui: [ http://msdn.microsoft.com/library/dd468057.aspx ](https://msdn.microsoft.com/library/dd468057.aspx).

Proxy di rilevamento delle probabilità informerà il gestore di stato oggetto ogni volta che le proprietà delle entità presenta il valore modificato, in modo che Entity Framework riconosce lo stato effettivo delle entità di tutto il tempo. Questa operazione viene eseguita aggiungendo gli eventi di notifica al corpo dei metodi setter della proprietà e quando il gestore di stato oggetto l'elaborazione di tali eventi. Si noti che la creazione di un proxy di entità viene in genere essere più costoso rispetto alla creazione di un'entità POCO non proxy a causa di un ulteriore set di eventi creati da Entity Framework.

Quando un'entità POCO non dispone di un proxy di rilevamento modifiche, vengono rilevate modifiche confrontando il contenuto delle entità in una copia di uno stato salvato precedente. Questo confronto approfondito diventerà molto tempo quando si dispone di molte entità nel contesto, o quando l'entità dispone di una quantità molto grande di proprietà, anche se nessuno di essi modificato dall'ultimo confronto ha avuto luogo.

In sintesi: si pagherà un calo durante la creazione di proxy di rilevamento delle modifiche delle prestazioni, sebbene il rilevamento delle modifiche consente di velocizzare il processo di rilevamento modifiche quando le entità hanno molte proprietà oppure quando si dispone di molte entità nel modello. Per le entità con un numero limitato di proprietà in cui la quantità di entità non aumentare una quantità eccessiva, con il proxy di rilevamento delle modifiche potrebbe non essere molto utili.

## <a name="8-loading-related-entities"></a>8 durante il caricamento di entità correlate

### <a name="81-lazy-loading-vs-eager-loading"></a>8.1 Visual Studio durante il caricamento lazy. Caricamento eager

Entity Framework offre diversi modi per caricare le entità correlate all'entità di destinazione. Ad esempio, quando esegue una query per i prodotti, esistono diversi modi che gli ordini correlati verranno caricati nel gestore di stato oggetto. Dal punto di vista delle prestazioni, la domanda principale da prendere in considerazione durante il caricamento di entità correlate saranno se utilizzare il caricamento Lazy o il caricamento Eager.

Quando si utilizza il caricamento Eager, le entità correlate vengono caricate insieme ai set di entità di destinazione. Si usa un'istruzione di inclusione nella query per indicare quali si vuole importare entità correlate.

Quando si utilizza il caricamento Lazy, la query iniziale riporta solo nel set di entità di destinazione. Ma ogni volta che si accede a una proprietà di navigazione, viene generata un'altra query a fronte dell'archivio per caricare l'entità correlata.

Una volta che un'entità è stata caricata, altre query per l'entità lo caricherà direttamente dal gestore di stato oggetto, se si utilizza il caricamento lazy o il caricamento eager.

### <a name="82-how-to-choose-between-lazy-loading-and-eager-loading"></a>8.2 come scegliere tra il caricamento Lazy e il caricamento Eager

È importante comprendere la differenza tra il caricamento Lazy e il caricamento Eager in modo che è possibile effettuare la scelta corretta per l'applicazione. Ciò consentirà di valutare il compromesso tra più richieste nel database rispetto a una singola richiesta che può contenere un payload di grandi dimensioni. Potrebbe essere opportuno usare il caricamento eager in alcune parti dell'applicazione e il caricamento lazy in altre parti.

Ad esempio di ciò che accade dietro le quinte, si supponga di che voler eseguire una query per i clienti che vivono nel Regno Unito e il numero di ordini.

**Tramite il caricamento Eager**

``` csharp
using (NorthwindEntities context = new NorthwindEntities())
{
    var ukCustomers = context.Customers.Include(c => c.Orders).Where(c => c.Address.Country == "UK");
    var chosenCustomer = AskUserToPickCustomer(ukCustomers);
    Console.WriteLine("Customer Id: {0} has {1} orders", customer.CustomerID, customer.Orders.Count);
}
```

**Utilizzando il caricamento Lazy**

``` csharp
using (NorthwindEntities context = new NorthwindEntities())
{
    context.ContextOptions.LazyLoadingEnabled = true;

    //Notice that the Include method call is missing in the query
    var ukCustomers = context.Customers.Where(c => c.Address.Country == "UK");

    var chosenCustomer = AskUserToPickCustomer(ukCustomers);
    Console.WriteLine("Customer Id: {0} has {1} orders", customer.CustomerID, customer.Orders.Count);
}
```

Quando si utilizza il caricamento eager, si verrà inviata una singola query che restituisce tutti i clienti e tutti gli ordini. Il comando di archiviazione è simile:

``` SQL
SELECT
[Project1].[C1] AS [C1],
[Project1].[CustomerID] AS [CustomerID],
[Project1].[CompanyName] AS [CompanyName],
[Project1].[ContactName] AS [ContactName],
[Project1].[ContactTitle] AS [ContactTitle],
[Project1].[Address] AS [Address],
[Project1].[City] AS [City],
[Project1].[Region] AS [Region],
[Project1].[PostalCode] AS [PostalCode],
[Project1].[Country] AS [Country],
[Project1].[Phone] AS [Phone],
[Project1].[Fax] AS [Fax],
[Project1].[C2] AS [C2],
[Project1].[OrderID] AS [OrderID],
[Project1].[CustomerID1] AS [CustomerID1],
[Project1].[EmployeeID] AS [EmployeeID],
[Project1].[OrderDate] AS [OrderDate],
[Project1].[RequiredDate] AS [RequiredDate],
[Project1].[ShippedDate] AS [ShippedDate],
[Project1].[ShipVia] AS [ShipVia],
[Project1].[Freight] AS [Freight],
[Project1].[ShipName] AS [ShipName],
[Project1].[ShipAddress] AS [ShipAddress],
[Project1].[ShipCity] AS [ShipCity],
[Project1].[ShipRegion] AS [ShipRegion],
[Project1].[ShipPostalCode] AS [ShipPostalCode],
[Project1].[ShipCountry] AS [ShipCountry]
FROM ( SELECT
      [Extent1].[CustomerID] AS [CustomerID],
       [Extent1].[CompanyName] AS [CompanyName],
       [Extent1].[ContactName] AS [ContactName],
       [Extent1].[ContactTitle] AS [ContactTitle],
       [Extent1].[Address] AS [Address],
       [Extent1].[City] AS [City],
       [Extent1].[Region] AS [Region],
       [Extent1].[PostalCode] AS [PostalCode],
       [Extent1].[Country] AS [Country],
       [Extent1].[Phone] AS [Phone],
       [Extent1].[Fax] AS [Fax],
      1 AS [C1],
       [Extent2].[OrderID] AS [OrderID],
       [Extent2].[CustomerID] AS [CustomerID1],
       [Extent2].[EmployeeID] AS [EmployeeID],
       [Extent2].[OrderDate] AS [OrderDate],
       [Extent2].[RequiredDate] AS [RequiredDate],
       [Extent2].[ShippedDate] AS [ShippedDate],
       [Extent2].[ShipVia] AS [ShipVia],
       [Extent2].[Freight] AS [Freight],
       [Extent2].[ShipName] AS [ShipName],
       [Extent2].[ShipAddress] AS [ShipAddress],
       [Extent2].[ShipCity] AS [ShipCity],
       [Extent2].[ShipRegion] AS [ShipRegion],
       [Extent2].[ShipPostalCode] AS [ShipPostalCode],
       [Extent2].[ShipCountry] AS [ShipCountry],
      CASE WHEN ([Extent2].[OrderID] IS NULL) THEN CAST(NULL AS int) ELSE 1 END AS [C2]
      FROM  [dbo].[Customers] AS [Extent1]
      LEFT OUTER JOIN [dbo].[Orders] AS [Extent2] ON [Extent1].[CustomerID] = [Extent2].[CustomerID]
      WHERE N'UK' = [Extent1].[Country]
)  AS [Project1]
ORDER BY [Project1].[CustomerID] ASC, [Project1].[C2] ASC
```

Quando si utilizza il caricamento lazy, è possibile eseguire la query seguente inizialmente:

``` SQL
SELECT
[Extent1].[CustomerID] AS [CustomerID],
[Extent1].[CompanyName] AS [CompanyName],
[Extent1].[ContactName] AS [ContactName],
[Extent1].[ContactTitle] AS [ContactTitle],
[Extent1].[Address] AS [Address],
[Extent1].[City] AS [City],
[Extent1].[Region] AS [Region],
[Extent1].[PostalCode] AS [PostalCode],
[Extent1].[Country] AS [Country],
[Extent1].[Phone] AS [Phone],
[Extent1].[Fax] AS [Fax]
FROM [dbo].[Customers] AS [Extent1]
WHERE N'UK' = [Extent1].[Country]
```

E ogni volta che si accede alla proprietà di navigazione di ordini di un cliente a cui viene eseguita a fronte dell'archivio query un'altra simile al seguente:

``` SQL
exec sp_executesql N'SELECT
[Extent1].[OrderID] AS [OrderID],
[Extent1].[CustomerID] AS [CustomerID],
[Extent1].[EmployeeID] AS [EmployeeID],
[Extent1].[OrderDate] AS [OrderDate],
[Extent1].[RequiredDate] AS [RequiredDate],
[Extent1].[ShippedDate] AS [ShippedDate],
[Extent1].[ShipVia] AS [ShipVia],
[Extent1].[Freight] AS [Freight],
[Extent1].[ShipName] AS [ShipName],
[Extent1].[ShipAddress] AS [ShipAddress],
[Extent1].[ShipCity] AS [ShipCity],
[Extent1].[ShipRegion] AS [ShipRegion],
[Extent1].[ShipPostalCode] AS [ShipPostalCode],
[Extent1].[ShipCountry] AS [ShipCountry]
FROM [dbo].[Orders] AS [Extent1]
WHERE [Extent1].[CustomerID] = @EntityKeyValue1',N'@EntityKeyValue1 nchar(5)',@EntityKeyValue1=N'AROUT'
```

Per altre informazioni, vedere la [caricamento di oggetti correlati](https://msdn.microsoft.com/library/bb896272.aspx).

#### <a name="821-lazy-loading-versus-eager-loading-cheat-sheet"></a>8.2.1 il caricamento lazy e il caricamento Eager foglio informativo

Non è disponibile come un unico a scegliere il caricamento eager e il caricamento lazy. Provare innanzitutto a comprendere le differenze tra entrambe le strategie permette quindi di eseguire delle decisioni ben informate; si consideri inoltre, se il codice si adatta a uno dei seguenti scenari:

| Scenario                                                                    | Il suggerimento                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                         |
|:----------------------------------------------------------------------------|:-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| È necessario accedere a molte proprietà di navigazione dalle entità recuperate? | **No** -entrambe le opzioni probabilmente eseguirà. Tuttavia, se il payload che assolutamente innovativo, la query non è troppo grande, che potrebbero verificarsi i vantaggi delle prestazioni tramite il caricamento Eager, perché sarà necessario meno round trip ai fini della materializzazione degli oggetti della rete. <br/> <br/> **Sì** -se è necessario accedere a molte proprietà di navigazione da entità, si farebbe che usando più includono le istruzioni nella query con il caricamento Eager. Le altre entità si aggiungono, più grande di payload restituiti dalla query. Una volta nella query si includono tre o più entità, considerare il passaggio per il caricamento Lazy. |
| Si conosce esattamente i dati che saranno necessarie in fase di esecuzione?                   | **No** -caricamento Lazy sarà migliore per l'utente. In caso contrario, si potrebbero avere l'esecuzione di query per i dati che non è necessario. <br/> <br/> **Sì** : Eager durante il caricamento è probabilmente la scelta migliore; aiuterà a caricano set intero più velocemente. Se la query richiede il recupero di una quantità molto grande di dati e questo diventa troppo lento, provare invece il caricamento Lazy.                                                                                                                                                                                                                                                       |
| È il codice in esecuzione tutt'altro che il database? (maggiore latenza di rete)  | **No** : quando la latenza di rete non è un problema, utilizzando il caricamento Lazy può semplificare il codice. Tenere presente che potrebbe cambiare la topologia dell'applicazione, in modo da non richiedere prossimità di database per concesso. <br/> <br/> **Sì** : quando la rete è un problema, solo è possibile decidere cosa si adatta meglio per il proprio scenario. Il caricamento Eager in genere sarà preferibile perché richiede meno round trip.                                                                                                                                                                                                      |


#### <a name="822-------performance-concerns-with-multiple-includes"></a>8.2.2 problemi di prestazioni with include più

Quando si pongono domande sulle prestazioni che comportano problemi di tempo di risposta server, l'origine del problema è spesso le query con più istruzioni di inclusione. Incluse le entità correlate in una query è potente, è importante comprendere ciò che accade dietro le quinte.

Bastano un tempo relativamente lungo per una query con più istruzioni di inclusione in essa passare attraverso il compilatore piano interno per generare il comando di archiviazione. La maggior parte di questo tempo viene impiegata per tenta di ottimizzare la query risultante. Il comando di archiviazione generato conterrà un Outer Join o Union per ogni includono, a seconda del mapping. Le query simile alla seguente aggiungeranno grandi grafici connessi dal database in un payload singolo, quale sarà acerbate eventuali problemi di larghezza di banda, soprattutto quando c'è molto di ridondanza nel payload (ad esempio, quando vengono utilizzati più livelli di inclusione per attraversare associazioni nella direzione uno-a-molti).

È possibile controllare nei casi in cui la query restituisce i payload di dimensioni eccessive accedendo TSQL sottostante per la query usando ToTraceString ed eseguendo il comando di archiviazione in SQL Server Management Studio per visualizzare le dimensioni del payload. In questi casi che è possibile provare a ridurre il numero di istruzioni Include nella query per importare i dati che necessari. In alternativa, è possibile suddividere la query in una sequenza più piccola di una sottoquery, ad esempio:

**Prima di interrompere la query:**

``` csharp
using (NorthwindEntities context = new NorthwindEntities())
{
    var customers = from c in context.Customers.Include(c => c.Orders)
                    where c.LastName.StartsWith(lastNameParameter)
                    select c;

    foreach (Customer customer in customers)
    {
        ...
    }
}
```

**Dopo l'interruzione della query:**

``` csharp
using (NorthwindEntities context = new NorthwindEntities())
{
    var orders = from o in context.Orders
                 where o.Customer.LastName.StartsWith(lastNameParameter)
                 select o;

    orders.Load();

    var customers = from c in context.Customers
                    where c.LastName.StartsWith(lastNameParameter)
                    select c;

    foreach (Customer customer in customers)
    {
        ...
    }
}
```

Che funzionerà solo query rilevate, in seguito all'utilizzo della capacità il contesto deve eseguire automaticamente la correzione della risoluzione e associazione di identità.

Come con il caricamento lazy, il compromesso sarà più query per i payload più piccoli. È anche possibile usare le proiezioni di singole proprietà selezionare in modo esplicito solo i dati che necessari da ogni entità, ma è non caricano le entità in questo caso, e gli aggiornamenti non saranno più supportati.

#### <a name="823-workaround-to-get-lazy-loading-of-properties"></a>8.2.3 soluzione alternativa per ottenere il caricamento lazy di proprietà

Entity Framework attualmente non supporta il caricamento lazy di proprietà scalare o complessa. Tuttavia, nei casi in cui si dispone di una tabella che include un oggetto di grandi dimensioni, ad esempio un BLOB, è possibile utilizzare la suddivisione di tabelle per separare le proprietà di grandi dimensioni in un'entità distinta. Ad esempio, si supponga di che avere una tabella Product che include una colonna photo varbinary. Se non è spesso necessario accedere a questa proprietà nelle query, è possibile usare per importare solo le parti dell'entità che è in genere suddivisione di tabelle. L'entità che rappresenta la foto del prodotto verrà caricato solo quando serve in modo esplicito.

Un'ottima risorsa che illustra come abilitare la suddivisione di tabelle è post di blog di Gil Fink "Tabella la suddivisione in Entity Framework": \< http://blogs.microsoft.co.il/blogs/gilf/archive/2009/10/13/table-splitting-in-entity-framework.aspx>.

## <a name="9-other-considerations"></a>9 altre considerazioni

### <a name="91------server-garbage-collection"></a>9.1 Garbage Collection per server

Alcuni utenti potrebbero verificarsi conflitti di risorse che limita il parallelismo in attesa quando il Garbage Collector non è configurato correttamente. Ogni volta che EF viene utilizzato in uno scenario multithreading, o in qualsiasi applicazione che è simile a un sistema sul lato server, assicurarsi di abilitare la Garbage Collection per Server. Questa operazione viene eseguita tramite una semplice impostazione nel file di configurazione dell'applicazione:

``` xml
<?xmlversion="1.0" encoding="utf-8" ?>
<configuration>
        <runtime>
               <gcServer enabled="true" />
        </runtime>
</configuration>
```

Questo dovrebbe ridurre i conflitti di thread e aumentare la velocità effettiva fino al 30% in scenari di CPU saturato. In termini generali, è necessario verificare sempre il comportamento dell'applicazione usando il classico Garbage Collection (e migliore per gli scenari di lato client e dell'interfaccia utente), nonché la Garbage Collection per Server.

### <a name="92------autodetectchanges"></a>9.2 AutoDetectChanges

Come accennato in precedenza, Entity Framework potrebbe mostrare i problemi di prestazioni quando la cache oggetti ha molte entità. Determinate operazioni, ad esempio Add, Remove, Find, voce e SaveChanges, attivano le chiamate a cui è possibile che occupino una grande quantità di CPU in base la cache degli oggetti è diventato DetectChanges. Il motivo è che la cache degli oggetti e il tentativo di gestione dello stato oggetto rimanere come il più possibile sincronizzati a ogni operazione eseguita per un contesto in modo che i dati generati sono sempre in un'ampia gamma di scenari corrette.

In genere è consigliabile lasciare il rilevamento delle modifiche automatico di Entity Framework abilitato per l'intera durata dell'applicazione. Se lo scenario viene compromessa per un utilizzo elevato della CPU e i profili indicano che la causa del problema è la chiamata a DetectChanges, prendere in considerazione di disattivare temporaneamente AutoDetectChanges nella parte sensibile del codice:

``` csharp
try
{
    context.Configuration.AutoDetectChangesEnabled = false;
    var product = context.Products.Find(productId);
    ...
}
finally
{
    context.Configuration.AutoDetectChangesEnabled = true;
}
```

Prima di spegnere AutoDetectChanges, è importante comprendere che ciò potrebbe causare Entity Framework potrebbe perdere la possibilità di monitorare determinate informazioni sulle modifiche che hanno luogo sulle entità. Se vengono gestite in modo non corretto, ciò potrebbe causare un'incoerenza dei dati nell'applicazione. Per altre informazioni su come disattivare AutoDetectChanges, leggere \< http://blog.oneunicorn.com/2012/03/12/secrets-of-detectchanges-part-3-switching-off-automatic-detectchanges/>.

### <a name="93------context-per-request"></a>9.3 contesto per ogni richiesta

Contesti di Entity Framework sono concepiti per essere utilizzate come esperienza di istanze di breve durate per garantire prestazioni ottimali. Contesti devono essere breve durata ed eliminati e di conseguenza sono stati implementati per essere molto semplici e reutilize metadati laddove possibile. In scenari web è importante tenere a mente e non dispone di un contesto per supera la durata di una singola richiesta. Analogamente, in scenari non web, contesto deve essere eliminato in base la comprensione dei diversi livelli di memorizzazione nella cache in Entity Framework. In generale, è necessario evitare con un'istanza del contesto per tutta la durata dell'applicazione, nonché i contesti per ogni thread e contesti statici.

### <a name="94------database-null-semantics"></a>9.4 semantica null di database

Entity Framework per impostazione predefinita verrà generato codice SQL con C\# null la semantica di confronto. Si consideri la query di esempio seguente:

``` csharp
            int? categoryId = 7;
            int? supplierId = 8;
            decimal? unitPrice = 0;
            short? unitsInStock = 100;
            short? unitsOnOrder = 20;
            short? reorderLevel = null;

            var q = from p incontext.Products
                    wherep.Category.CategoryName == "Beverages"
                          || (p.CategoryID == categoryId
                                || p.SupplierID == supplierId
                                || p.UnitPrice == unitPrice
                                || p.UnitsInStock == unitsInStock
                                || p.UnitsOnOrder == unitsOnOrder
                                || p.ReorderLevel == reorderLevel)
                    select p;

            var r = q.ToList();
```

In questo esempio, è sta confrontato un numero di variabili nullable rispetto alle proprietà che ammette valori null nell'entità, ad esempio SupplierID e UnitPrice. Il codice SQL generato per questa query verrà richiesto se il valore del parametro è identico al valore di colonna o se entrambi i parametri e i valori della colonna sono null. Ciò consente di nascondere il modo in cui il server di database gestisce valori null e fornirà un C coerente\# null esperienza tra i fornitori di database diverso. D'altra parte, il codice generato è un po' contorta e potrebbero non funzionare bene quando la quantità di confronti in where istruzione della query aumenta fino a un numero elevato.

Un modo per gestire questa situazione consiste nell'usare la semantica null di database. Si noti che questo potrebbe potenzialmente comportarsi in modo diverso a C\# null semantica dall'ora Entity Framework verrà generato codice SQL più semplice che espone il modo in cui il motore di database gestisce valori null. Semantica null di database può essere attivato per ogni contesto con una riga singola configurazione in base alla configurazione di contesto:

``` csharp
                context.Configuration.UseDatabaseNullSemantics = true;
```

Piccole e medie dimensioni query non verranno visualizzati un miglioramento delle prestazioni percettibile quando si usa la semantica null di database, ma la differenza diventerà più evidente nelle query con un numero elevato di potenziali confronti di valori null.

Nella query di esempio precedente, la differenza nelle prestazioni è inferiore al 2% in un microbenchmark in esecuzione in un ambiente controllato.

### <a name="95------async"></a>9,5 Async

Supporto di Entity Framework 6 introdotte delle operazioni asincrone quando in esecuzione in .NET 4.5 o versioni successive. Nella maggior parte, le applicazioni con i/o correlati contesa sarà più vantaggioso utilizzare query asincrona e operazioni di salvataggio. Se l'applicazione non subisca una contesa dei / o, l'utilizzo di async verrà, nei casi migliori, eseguire in modo sincrono e restituiscono il risultato nella stessa quantità di tempo come una chiamata sincrona, o nel peggiore dei casi, semplicemente rinvia l'esecuzione di un'attività asincrona e aggiungere tim extra e per il completamento dello scenario.

Per informazioni sull'attività di programmazione asincrono utili per decidere se async consentirà di migliorare le prestazioni dell'applicazione visitati [ http://msdn.microsoft.com/library/hh191443.aspx ](https://msdn.microsoft.com/library/hh191443.aspx). Per altre informazioni sull'uso delle operazioni asincrone in Entity Framework, vedere [Async Query e salvataggio](~/ef6/fundamentals/async.md
).

### <a name="96------ngen"></a>9.6 NGEN

Entity Framework 6 non viene fornito nell'installazione predefinita di .NET framework. Di conseguenza, gli assembly di Entity Framework non sono che Natively per impostazione predefinita, il che significa che tutto il codice di Entity Framework è soggetta a costi JIT'ing stesso come qualsiasi altro assembly MSIL. Questo potrebbe compromettere l'esperienza F5 durante lo sviluppo e anche l'avvio a freddo dell'applicazione negli ambienti di produzione. Per ridurre i costi della CPU e memoria di JIT'ing è consigliabile NGEN di immagini di Entity Framework come appropriato. Per altre informazioni su come migliorare le prestazioni di avvio di Entity Framework 6 con NGEN, vedere [miglioramento delle prestazioni di avvio con NGen](~/ef6/fundamentals/performance/ngen.md).

### <a name="97------code-first-versus-edmx"></a>9.7 code First per un confronto EDMX

Motivi di Entity Framework relativi al problema di mancata corrispondenza dell'impedenza tra la programmazione orientata agli oggetti e i database relazionali facendo in modo che una rappresentazione in memoria del modello concettuale (oggetto), lo schema di archiviazione (database) e un mapping tra il due. Questi metadati viene chiamato un Entity Data Model, o EDM per brevità. Da EDM, Entity Framework verrà derivare le viste per i dati di round trip dagli oggetti in memoria nel database e il backup.

Quando Entity Framework viene usato con un file EDMX che formalmente specifica del modello concettuale, lo schema di archiviazione e il mapping, quindi la fase di caricamento del modello ha solo convalidare che il modello EDM è corretto (ad esempio, assicurarsi che nessun mapping non è presente), quindi Genera le visualizzazioni, convalidare le visualizzazioni e hanno questo pronto per l'uso di metadati. Solo può quindi una query da eseguire o salvare i dati di nuovo all'archivio dati.

L'approccio Code First è, essenzialmente, un sofisticato generatore Entity Data Model. Entity Framework ha per produrre un modello EDM da codice fornito; Ciò avviene tramite l'analisi le classi coinvolte nel modello di applicazione delle convenzioni e la configurazione di modello tramite l'API Fluent. Una volta creato il modello EDM, Entity Framework essenzialmente si comporta come verrebbe avesse un file EDMX partecipato nel progetto. Di conseguenza, compilazione del modello da Code First aggiunge complessità aggiuntive che si traduce in un tempo di avvio più lento per Entity Framework rispetto alla presenza di un EDMX. Il costo dipende completamente le dimensioni e complessità del modello che viene compilato.

Se si sceglie di usare EDMX rispetto a Code First, è importante sapere che la flessibilità introdotta da Code First aumenta il costo della creazione del modello per la prima volta. Se l'applicazione è in grado di resistere il costo di questo tipo di caricamento prima volta, in genere Code First sarà il modo migliore per passare.

## <a name="10-investigating-performance"></a>Prestazioni di analisi dei 10

### <a name="101-using-the-visual-studio-profiler"></a>10.1 usando il Profiler di Visual Studio

Se si sono verificati problemi di prestazioni con Entity Framework, è possibile utilizzare un profiler simile a quello incorporato in Visual Studio per vedere dove l'applicazione trascorre del tempo. Questo è lo strumento è stato usato per generare i grafici a torta nel post di blog "Analisi delle prestazioni di ADO.NET Entity Framework - parte 1" ( \< http://blogs.msdn.com/b/adonet/archive/2008/02/04/exploring-the-performance-of-the-ado-net-entity-framework-part-1.aspx>) che mostra dove Entity Framework Usa il tempo durante le query a freddo e a caldo.

Il post di blog "Profilatura Entity Framework con il Profiler di 2010 Visual Studio" scritto dagli adattatori dati e modellazione Customer Advisory Team illustra un esempio reale di come usavano il profiler per analizzare un problema di prestazioni.  \<http://blogs.msdn.com/b/dmcat/archive/2010/04/30/profiling-entity-framework-using-the-visual-studio-2010-profiler.aspx>. Questo post è stato scritto per un'applicazione windows. Se si desidera profilare un'applicazione web potrebbero funzionare meglio di lavoro da Visual Studio gli strumenti di Windows Performance Recorder (WPR) e Windows Performance Analyzer (WPA). WPR e WPA fanno parte di Windows Performance Toolkit incluso in Windows Assessment and Deployment Kit ( [ http://www.microsoft.com/en-US/download/details.aspx?id=39982 ](https://www.microsoft.com/en-US/download/details.aspx?id=39982)).

### <a name="102-applicationdatabase-profiling"></a>10.2 profilatura Database e dell'applicazione

Strumenti come profiler incorporato in Visual Studio indicano dove l'applicazione trascorre del tempo.  È disponibile un altro tipo di profiler che esegue l'analisi dinamica dell'applicazione in esecuzione, in produzione o di pre-produzione in base alle esigenze e si esegue la ricerca di problemi più comuni e anti-modelli di accesso al database.

Due profiler disponibile in commercio sono il Profiler di Entity Framework ( \< http://efprof.com>) ORMProfiler e ( \< http://ormprofiler.com>).

Se l'applicazione è un'applicazione MVC con Code First, è possibile usare MiniProfiler di StackExchange. Scott Hanselman descrive questo strumento nel suo blog all'indirizzo: \< http://www.hanselman.com/blog/NuGetPackageOfTheWeek9ASPNETMiniProfilerFromStackExchangeRocksYourWorld.aspx>.

Per altre informazioni sulla profilatura delle attività del database dell'applicazione, vedere articolo di MSDN Magazine di Julie intitolata [attività di profilatura del Database in Entity Framework](https://msdn.microsoft.com/magazine/gg490349.aspx).

### <a name="103-database-logger"></a>10.3 logger database

Se si usa Entity Framework 6 anche provare a usare la funzionalità di registrazione predefiniti. La proprietà del Database di contesto è possibile indicare al log attività tramite una semplice configurazione di una sola riga:

``` csharp
    using (var context = newQueryComparison.DbC.NorthwindEntities())
    {
        context.Database.Log = Console.WriteLine;
        var q = context.Products.Where(p => p.Category.CategoryName == "Beverages");
        q.ToList();
    }
```

In questo esempio l'attività del database verrà registrato nella console, ma la proprietà del Log può essere configurata per richiamare qualsiasi azione&lt;stringa&gt; delegare.

Se si vuole abilitare la registrazione del database senza dover ricompilare e si usa Entity Framework 6.1 o versioni successive, è possibile farlo mediante l'aggiunta di un intercettore nel file Web. config o App. config dell'applicazione.

``` xml
  <interceptors>
    <interceptor type="System.Data.Entity.Infrastructure.Interception.DatabaseLogger, EntityFramework">
      <parameters>
        <parameter value="C:\Path\To\My\LogOutput.txt"/>
      </parameters>
    </interceptor>
  </interceptors>
```

Per altre informazioni su come aggiungere la registrazione senza ricompilare Vai a \< http://blog.oneunicorn.com/2014/02/09/ef-6-1-turning-on-logging-without-recompiling/>.

## <a name="11-appendix"></a>Appendice 11

### <a name="111-a-test-environment"></a>11.1 ambiente di Test r.

Questo ambiente Usa una configurazione di 2-computer con il database in un computer separato dall'applicazione client. Le macchine sono nello stesso rack, in modo che la latenza di rete è relativamente bassa, ma più realistico di un ambiente singolo computer.

#### <a name="1111-------app-server"></a>11.1.1 app Server

##### <a name="11111------software-environment"></a>11.1.1.1 ambiente software

-   Ambiente di Software di Entity Framework 4
    -   Nome del sistema operativo: Windows Server 2008 R2 Enterprise SP1.
    -   Visual Studio 2010-Ultimate.
    -   Visual Studio 2010 SP1 (solo per alcuni confronti).
-   Ambiente di Software di Entity Framework 5 e 6
    -   Nome del sistema operativo: Windows 8.1 Enterprise
    -   Visual Studio 2013-Ultimate.

##### <a name="11112------hardware-environment"></a>11.1.1.2 ambiente hardware

-   Doppio processore: Intel (r) Xeon (r) CPU L5520 W3530 @ 2,27ghz, 2261 Mhz8 GHz, 4 memoria centrale, 84 dei processori logici.
-   RamRAM 2412 GB.
-   136 unità della 3GB/s rpm di GB SCSI250GB SATA 7200 suddividere in 4 partizioni.

#### <a name="1112-------db-server"></a>11.1.2 server DB

##### <a name="11121------software-environment"></a>11.1.2.1 ambiente software

-   Nome del sistema operativo: Windows Server 2008 R28.1 Enterprise SP1.
-   SQL Server 2008 R22012.

##### <a name="11122------hardware-environment"></a>11.1.2.2 ambiente hardware

-   Solo processore: Intel (r) Xeon (r) CPU L5520 @ 2,27ghz, 2261 MhzES-1620 0 @ 3.60 GHz, memoria 4 centrale, 8 processori logici.
-   RamRAM 824 GB.
-   465 GB ATA500GB SATA 7200 6GB/s rigido a rpm suddividere in 4 partizioni.

### <a name="112------b-query-performance-comparison-tests"></a>11.2 confronto tra i test delle prestazioni di Query B.

Per eseguire questi test è stato usato il modello Northwind. È stato generato dal database utilizzando la finestra di progettazione di Entity Framework. Quindi, il codice seguente è stato utilizzato per confrontare le prestazioni delle opzioni di esecuzione query:

``` csharp
using System;
using System.Collections.Generic;
using System.Data;
using System.Data.Common;
using System.Data.Entity.Infrastructure;
using System.Data.EntityClient;
using System.Data.Objects;
using System.Linq;

namespace QueryComparison
{
    public partial class NorthwindEntities : ObjectContext
    {
        private static readonly Func<NorthwindEntities, string, IQueryable<Product>> productsForCategoryCQ = CompiledQuery.Compile(
            (NorthwindEntities context, string categoryName) =>
                context.Products.Where(p => p.Category.CategoryName == categoryName)
                );

        public IQueryable<Product> InvokeProductsForCategoryCQ(string categoryName)
        {
            return productsForCategoryCQ(this, categoryName);
        }
    }

    public class QueryTypePerfComparison
    {
        private static string entityConnectionStr = @"metadata=res://*/Northwind.csdl|res://*/Northwind.ssdl|res://*/Northwind.msl;provider=System.Data.SqlClient;provider connection string='data source=.;initial catalog=Northwind;integrated security=True;multipleactiveresultsets=True;App=EntityFramework'";

        public void LINQIncludingContextCreation()
        {
            using (NorthwindEntities context = new NorthwindEntities())
            {                 
                var q = context.Products.Where(p => p.Category.CategoryName == "Beverages");
                q.ToList();
            }
        }

        public void LINQNoTracking()
        {
            using (NorthwindEntities context = new NorthwindEntities())
            {
                context.Products.MergeOption = MergeOption.NoTracking;

                var q = context.Products.Where(p => p.Category.CategoryName == "Beverages");
                q.ToList();
            }
        }

        public void CompiledQuery()
        {
            using (NorthwindEntities context = new NorthwindEntities())
            {
                var q = context.InvokeProductsForCategoryCQ("Beverages");
                q.ToList();
            }
        }

        public void ObjectQuery()
        {
            using (NorthwindEntities context = new NorthwindEntities())
            {
                ObjectQuery<Product> products = context.Products.Where("it.Category.CategoryName = 'Beverages'");
                products.ToList();
            }
        }

        public void EntityCommand()
        {
            using (EntityConnection eConn = new EntityConnection(entityConnectionStr))
            {
                eConn.Open();
                EntityCommand cmd = eConn.CreateCommand();
                cmd.CommandText = "Select p From NorthwindEntities.Products As p Where p.Category.CategoryName = 'Beverages'";

                using (EntityDataReader reader = cmd.ExecuteReader(CommandBehavior.SequentialAccess))
                {
                    List<Product> productsList = new List<Product>();
                    while (reader.Read())
                    {
                        DbDataRecord record = (DbDataRecord)reader.GetValue(0);

                        // 'materialize' the product by accessing each field and value. Because we are materializing products, we won't have any nested data readers or records.
                        int fieldCount = record.FieldCount;

                        // Treat all products as Product, even if they are the subtype DiscontinuedProduct.
                        Product product = new Product();  

                        product.ProductID = record.GetInt32(0);
                        product.ProductName = record.GetString(1);
                        product.SupplierID = record.GetInt32(2);
                        product.CategoryID = record.GetInt32(3);
                        product.QuantityPerUnit = record.GetString(4);
                        product.UnitPrice = record.GetDecimal(5);
                        product.UnitsInStock = record.GetInt16(6);
                        product.UnitsOnOrder = record.GetInt16(7);
                        product.ReorderLevel = record.GetInt16(8);
                        product.Discontinued = record.GetBoolean(9);

                        productsList.Add(product);
                    }
                }
            }
        }

        public void ExecuteStoreQuery()
        {
            using (NorthwindEntities context = new NorthwindEntities())
            {
                ObjectResult<Product> beverages = context.ExecuteStoreQuery<Product>(
@"    SELECT        P.ProductID, P.ProductName, P.SupplierID, P.CategoryID, P.QuantityPerUnit, P.UnitPrice, P.UnitsInStock, P.UnitsOnOrder, P.ReorderLevel, P.Discontinued
    FROM            Products AS P INNER JOIN Categories AS C ON P.CategoryID = C.CategoryID
    WHERE        (C.CategoryName = 'Beverages')"
);
                beverages.ToList();
            }
        }

        public void ExecuteStoreQueryDbContext()
        {
            using (var context = new QueryComparison.DbC.NorthwindEntities())
            {
                var beverages = context.Database.SqlQuery\<QueryComparison.DbC.Product>(
@"    SELECT        P.ProductID, P.ProductName, P.SupplierID, P.CategoryID, P.QuantityPerUnit, P.UnitPrice, P.UnitsInStock, P.UnitsOnOrder, P.ReorderLevel, P.Discontinued
    FROM            Products AS P INNER JOIN Categories AS C ON P.CategoryID = C.CategoryID
    WHERE        (C.CategoryName = 'Beverages')"
);
                beverages.ToList();
            }
        }

        public void ExecuteStoreQueryDbSet()
        {
            using (var context = new QueryComparison.DbC.NorthwindEntities())
            {
                var beverages = context.Products.SqlQuery(
@"    SELECT        P.ProductID, P.ProductName, P.SupplierID, P.CategoryID, P.QuantityPerUnit, P.UnitPrice, P.UnitsInStock, P.UnitsOnOrder, P.ReorderLevel, P.Discontinued
    FROM            Products AS P INNER JOIN Categories AS C ON P.CategoryID = C.CategoryID
    WHERE        (C.CategoryName = 'Beverages')"
);
                beverages.ToList();
            }
        }

        public void LINQIncludingContextCreationDbContext()
        {
            using (var context = new QueryComparison.DbC.NorthwindEntities())
            {                 
                var q = context.Products.Where(p => p.Category.CategoryName == "Beverages");
                q.ToList();
            }
        }

        public void LINQNoTrackingDbContext()
        {
            using (var context = new QueryComparison.DbC.NorthwindEntities())
            {
                var q = context.Products.AsNoTracking().Where(p => p.Category.CategoryName == "Beverages");
                q.ToList();
            }
        }
    }
}
```

### <a name="113-c-navision-model"></a>Modello Navision 11.3 C.

Il database Navision è un database di grandi dimensioni consente di demo di Microsoft Dynamics-NAV. Il modello concettuale generato contiene set di entità 1005 e 4227 set di associazioni. Il modello usato nel test è "flat" – Nessuna ereditarietà è stato aggiunto ad esso.

#### <a name="1131-queries-used-for-navision-tests"></a>11.3.1 query usate per i test Navision

L'elenco di query con il modello Navision contiene 3 categorie di query Entity SQL:

##### <a name="11311-lookup"></a>11.3.1.1 ricerca

Una query di ricerca semplice senza aggregazioni

-   Conteggio: 16232
-   Esempio:

``` xml
  <Query complexity="Lookup">
    <CommandText>Select value distinct top(4) e.Idle_Time From NavisionFKContext.Session as e</CommandText>
  </Query>
```

##### <a name="11312-singleaggregating"></a>11.3.1.2 SingleAggregating

Una normale query BI con più aggregazioni, ma non i subtotali (query singola)

-   Conteggio: 2313
-   Esempio:

``` xml
  <Query complexity="SingleAggregating">
    <CommandText>NavisionFK.MDF_SessionLogin_Time_Max()</CommandText>
  </Query>
```

In cui MDF\_SessionLogin\_ora\_max () è definito nel modello come:

``` xml
  <Function Name="MDF_SessionLogin_Time_Max" ReturnType="Collection(DateTime)">
    <DefiningExpression>SELECT VALUE Edm.Min(E.Login_Time) FROM NavisionFKContext.Session as E</DefiningExpression>
  </Function>
```

##### <a name="11313-aggregatingsubtotals"></a>11.3.1.3 AggregatingSubtotals

Una query di BI con le aggregazioni e i subtotali (tramite unione tutti)

-   Conteggio: 178
-   Esempio:

``` xml
  <Query complexity="AggregatingSubtotals">
    <CommandText>
using NavisionFK;
function AmountConsumed(entities Collection([CRONUS_International_Ltd__Zone])) as
(
    Edm.Sum(select value N.Block_Movement FROM entities as E, E.CRONUS_International_Ltd__Bin as N)
)
function AmountConsumed(P1 Edm.Int32) as
(
    AmountConsumed(select value e from NavisionFKContext.CRONUS_International_Ltd__Zone as e where e.Zone_Ranking = P1)
)
----------------------------------------------------------------------------------------------------------------------
(
    select top(10) Zone_Ranking, Cross_Dock_Bin_Zone, AmountConsumed(GroupPartition(E))
    from NavisionFKContext.CRONUS_International_Ltd__Zone as E
    where AmountConsumed(E.Zone_Ranking) > @MinAmountConsumed
    group by E.Zone_Ranking, E.Cross_Dock_Bin_Zone
)
union all
(
    select top(10) Zone_Ranking, Cast(null as Edm.Byte) as P2, AmountConsumed(GroupPartition(E))
    from NavisionFKContext.CRONUS_International_Ltd__Zone as E
    where AmountConsumed(E.Zone_Ranking) > @MinAmountConsumed
    group by E.Zone_Ranking
)
union all
{
    Row(Cast(null as Edm.Int32) as P1, Cast(null as Edm.Byte) as P2, AmountConsumed(select value E
                                                                         from NavisionFKContext.CRONUS_International_Ltd__Zone as E
                                                                         where AmountConsumed(E.Zone_Ranking) > @MinAmountConsumed))
}</CommandText>
    <Parameters>
      <Parameter Name="MinAmountConsumed" DbType="Int32" Value="10000" />
    </Parameters>
  </Query>
```
