---
title: Le versioni precedenti di Entity Framework - Entity Framework 6
author: divega
ms.date: 2016-10-23
ms.prod: entity-framework
ms.author: divega
ms.manager: avickers
ms.technology: entity-framework-6
ms.topic: article
ms.assetid: 1060bb99-765f-4f32-aaeb-d6635d3dbd3e
caps.latest.revision: 4
ms.openlocfilehash: 5be3632fd3a3f04e12e2d3aa67de6c1d9c7b56a2
ms.sourcegitcommit: bdd06c9a591ba5e6d6a3ec046c80de98f598f3f3
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/10/2018
ms.locfileid: "39122810"
---
# <a name="past-releases-of-entity-framework"></a>Le versioni precedenti di Entity Framework

La prima versione di Entity Framework è stata rilasciata nel 2008, come parte di .NET Framework 3.5 SP1 e Visual Studio 2008 SP1.

A partire dalla versione 4.1 è incluso come le [pacchetto NuGet EntityFramework](https://www.nuget.org/packages/EntityFramework/) -attualmente uno dei più diffusi pacchetti in NuGet.org.

Tra le versioni 4.1 e 5.0, il pacchetto EntityFramework NuGet esteso le librerie di Entity Framework forniti come parte di .NET Framework.   

A partire dalla versione 6, Entity Framework è diventato un progetto open source e anche spostati completamente dal form fuori banda in .NET Framework.
Ciò significa che quando si aggiunge il pacchetto NuGet di Entity Framework versione 6 per un'applicazione, si ottiene una copia completa della libreria di Entity Framework che non dipendono i bit di Entity Framework che vengono fornite come parte di .NET Framework.
In questo modo agevole in qualche modo accelera il ritmo di sviluppo e il recapito delle nuove funzionalità.

A giugno 2016, abbiamo rilasciato EF Core 1.0. EF Core si basa su una nuova base di codici ed è progettata come una versione più leggera ed estendibile di Entity Framework.
EF Core è attualmente l'obiettivo principale di sviluppo per il Team di Entity Framework in Microsoft.
Ciò significa che non sono previste nuove funzionalità principali per EF6. Tuttavia Entity Framework 6 viene conservata come un progetto open source e un prodotto Microsoft supportato.

Ecco l'elenco delle versioni precedenti, in ordine cronologico inverso, con informazioni sulle nuove funzionalità introdotte in ogni versione.

## <a name="ef-613"></a>EF 6.1.3
EF 6.1.3 runtime è stato rilasciato a NuGet nel mese di ottobre 2015.
Questa versione include correzioni sole per i difetti ad alta priorità e regressioni crearti 6.1.2 la versione.
Le correzioni includono:

- Query: Regressione in EF 6.1.2: OUTER APPLY introdotto e query più complesse per le relazioni 1:1 e la clausola "let"
- Problema TPT nascondendo proprietà della classe di base nelle classi ereditate
- DbMigration.Sql ha esito negativo quando la parola 'go' è contenuta nel testo
- Creazione di flag di compatibilità per UnionAll trae e Intersect supporto bidimensionale
- Query con include più non funziona in 6.1.2 (utilizza 6.1.1)
- "È presente un errore nella sintassi SQL" eccezione dopo l'aggiornamento da Entity Framework 6.1.1 a 6.1.2

## <a name="ef-612"></a>EF 6.1.2
EF 6.1.2 runtime è stato rilasciato a NuGet dicembre 2014.
Questa versione è principalmente sulle correzioni di bug. Sono accettati anche un paio di modifiche importanti dai membri della community:
- **Parametri di cache di query possono essere configurati dal file app/web.configuration**
    ``` xml
    <entityFramework>
      <queryCache size='1000' cleaningIntervalInSeconds='-1'/>
    </entityFramework>
    ```
- **Metodi di SqlFile e SqlResource su DbMigration** consentono di eseguire un SQL archiviata come file di script o risorsa incorporata.

## <a name="ef-611"></a>ENTITY FRAMEWORK 6.1.1
Di Entity Framework 6.1.1 runtime è stato rilasciato a NuGet giugno 2014.
Questa versione contiene correzioni per problemi che hanno rilevato un numero di persone. Tra gli altri:
- Designer: Errore apertura EF5 edmx con precisione decimale nella finestra di progettazione di Entity Framework 6
- Logica di rilevamento predefinito istanza di Local DB non funziona con SQL Server 2014

## <a name="ef-610"></a>EF 6.1.0
EF 6.1.0 runtime è stato rilasciato a NuGet nel mese di marzo del 2014.
Questo aggiornamento secondario include un numero significativo di nuove funzionalità:

- **Gli strumenti di consolidamento** fornisce un modo coerente per creare un nuovo modello di Entity Framework. Questa caratteristica [estende la procedura guidata ADO.NET Entity Data Model per supportare la creazione di modelli di Code First](~/ef6/modeling/code-first/workflows/existing-database.md), tra cui decompilazione da un database esistente. Queste funzionalità in precedenza erano disponibili in qualità di Beta negli strumenti avanzati di Entity Framework.
- **[La gestione di errori di commit delle transazioni](~/ef6/fundamentals/connection-resiliency/commit-failures.md)**  fornisce il CommitFailureHandler che usa la funzionalità appena introdotta di intercettare le operazioni di transazioni. Il CommitFailureHandler consente il recupero automatico da errori di connessione durante il commit di una transazione.
- **[IndexAttribute](~/ef6/modeling/code-first/data-annotations.md)**  sono consentiti indici specificare inserendo un `[Index]` attributo su una proprietà (o proprietà) nel modello Code First. Codice prima di tutto quindi creerà un indice corrispondente nel database.
- **L'API di mapping pubblico** fornisce l'accesso alle informazioni di Entity Framework è in modalità di mapping dei tipi e proprietà a colonne e tabelle nel database. Nelle versioni precedenti questa API è interna.
- **[Possibilità di configurare gli intercettori tramite il file App/Web.config](~/ef6/fundamentals/configuring/config-file.md)**  consente gli intercettori essere aggiunti senza dover ricompilare l'applicazione.
- **System.Data.Entity.Infrastructure.Interception.DatabaseLogger**è disponibile un nuovo intercettore che rende più semplice registrare tutte le operazioni di database in un file. In combinazione con la funzionalità precedente, in questo modo è possibile facilmente [attivare la registrazione delle operazioni di database per un'applicazione distribuita](~/ef6/fundamentals/configuring/config-file.md), senza dover ricompilare.
- **Rilevamento delle modifiche del modello migrazioni** è stata migliorata in modo che le migrazioni con scaffolding sono più accurate; le prestazioni del processo di rilevamento modifiche sono stato inoltre migliorata.
- **Miglioramenti delle prestazioni** tra cui operazioni di database ridotto durante l'inizializzazione, le ottimizzazioni per il confronto di uguaglianza null nelle query LINQ, visualizzazione generazione (la creazione del modello) più velocemente in più scenari e più efficiente materializzazione di entità rilevate con più associazioni.

## <a name="ef-602"></a>EF 6.0.2
EF 6.0.2 runtime è stato rilasciato a NuGet nel dicembre del 2013.
Questa versione patch è limitata alla risoluzione dei problemi che sono state introdotte nella versione di Entity Framework 6 (regressioni di prestazioni/comportamento poiché EF5).

## <a name="ef-601"></a>EF 6.0.1
EF 6.0.1 runtime è stato rilasciato a NuGet nell'ottobre del 2013 contemporaneamente con Entity Framework 6.0.0, perché quest'ultimo è stato incorporato in una versione di Visual Studio che ha bloccato alcuni mesi prima.
Questa versione patch è limitata alla risoluzione dei problemi che sono state introdotte nella versione di Entity Framework 6 (regressioni di prestazioni/comportamento poiché EF5).
Le modifiche più importanti sono state a risolvere alcuni problemi di prestazioni durante il warm-up per i modelli di Entity Framework.
Questo è importante in quanto le prestazioni di riscaldamento è stata un'area di interesse in EF6 e questi problemi sono stati ricompilati impedendo alcuni altri miglioramenti delle prestazioni apportati in EF6.

## <a name="ef-60"></a>ENTITY FRAMEWORK 6.0
EF 6.0.0 runtime è stato rilasciato a NuGet nell'ottobre del 2013.
Questa è la prima versione in cui è incluso un runtime di Entity Framework completo nel [pacchetto NuGet EntityFramework](https://www.nuget.org/packages/EntityFramework/) che non dipende dai bit di Entity Framework che fanno parte di .NET Framework.
Spostare le parti restanti del runtime per il pacchetto NuGet obbligatorio un numero di una modifica per codice esistente.
Vedere la sezione sulla [l'aggiornamento a Entity Framework 6](upgrading-to-ef6.md) per altri dettagli sui passaggi manuali necessari per eseguire l'aggiornamento.

Questa versione include numerose nuove funzionalità.
Le funzionalità seguenti funzionano per i modelli creati con Code First o Entity Framework Designer:

- **[Query asincrone e salvataggio](~/ef6/fundamentals/async.md)**  aggiunge il supporto per i modelli asincroni basati su attività che sono state introdotte in .NET 4.5.
- **[Resilienza della connessione](~/ef6/fundamentals/connection-resiliency/retry-logic.md)**  consente il ripristino automatico dagli errori di connessione temporanei.
- **[Configurazione basata su codice](~/ef6/fundamentals/configuring/code-based.md)**  offre la possibilità di eseguire la configurazione dei – tradizionalmente eseguita in un file di configurazione: nel codice.
- **[Risoluzione delle dipendenze](~/ef6/fundamentals/configuring/dependency-resolution.md)**  introduce il supporto per il localizzatore servizio pattern e abbiamo fattorizzato alcune parti di funzionalità che possono essere sostituiti con implementazioni personalizzate.
- **[Registrazione/SQL di intercettazione](~/ef6/fundamentals/logging-and-interception.md)**  fornisce blocchi predefiniti di basso livello per l'intercettazione delle operazioni di Entity Framework con la registrazione SQL semplice creata nella parte superiore.
- **Miglioramenti della testabilità** rendono più semplice creare copie di test per DbContext e DbSet quando [usando un framework di simulazione](~/ef6/fundamentals/testing/mocking.md) oppure [scrivendo un proprio delle copie di test](~/ef6/fundamentals/testing/writing-test-doubles.md).
- **[DbContext è ora possibile creare con un elemento DbConnection che è già aperta](~/ef6/fundamentals/connection-management.md)**  consentendo così scenari in cui sarebbe utile se è stato possibile aprire la connessione durante la creazione del contesto (ad esempio la condivisione di una connessione tra i componenti in cui è possibile non garantisce che lo stato della connessione).
- **[Migliorato il supporto delle transazioni](~/ef6/saving/transactions.md)**  fornisce il supporto per una transazione esterna per i framework, nonché altri metodi di creazione di una transazione all'interno del Framework.
- **Le enumerazioni, spaziali e prestazioni migliori in .NET 4.0** - spostando i componenti principali che erano in .NET Framework nel pacchetto NuGet di Entity Framework ora siamo in grado di offrire supporto enum, tipi di dati spaziali e i miglioramenti delle prestazioni da EF5 su .NET 4.0.
- **Miglioramento delle prestazioni di Enumerable nelle query LINQ**.
- **Riscaldamento migliorata del tempo (generazione di visualizzazioni)**, in particolare per modelli di grandi dimensioni.
- **Pluralizzazione collegabile &amp; Singolarizzazione servizio**.
- **Le implementazioni personalizzate di GetHashCode o Equals** nell'entità sono ora supportate le classi.
- **DbSet.AddRange/RemoveRange** consente di aggiungere o rimuovere più entità da un set in modo ottimizzato.
- **DbChangeTracker.HasChanges** fornisce un modo semplice ed efficiente per vedere se sono presenti modifiche in sospeso da salvare nel database.
- **SqlCeFunctions** fornisce un SQL Compact equivalente al SqlFunctions.

Le funzionalità seguenti si applicano solo alle Code First:

- **[Convenzioni del primo codice personalizzato](~/ef6/modeling/code-first/conventions/custom.md)**  consentire le convenzioni per evitare la configurazione ripetitive di scrittura. Offriamo una semplice API per le convenzioni leggere, nonché alcuni blocchi più complessi predefiniti per consentire la creazione di convenzioni più complicate.
- **[Codice prima il Mapping a Stored procedure Insert/Update/Delete](~/ef6/modeling/code-first/fluent/cud-stored-procedures.md)**  è ora supportato.
- **[Script idempotenti migrazioni](~/ef6/modeling/code-first/migrations/index.md)**  consentono di generare uno script SQL che è possibile eseguire l'aggiornamento di un database in qualsiasi versione fino alla versione più recente.
- **[Tabella di cronologia migrazioni configurabile](~/ef6/modeling/code-first/migrations/history-customization.md)**  consente di personalizzare la definizione della tabella di cronologia delle migrazioni. Ciò è particolarmente utile per i provider di database che richiedono i tipi di dati appropriati e così via essere specificato per la tabella di cronologia di migrazioni per funzionare correttamente.
- **Più contesti per ogni Database** rimuove il limite precedente di un modello Code First per ogni database quando si usano le migrazioni o quando Code First creato automaticamente il database.
- **[DbModelBuilder.HasDefaultSchema](~/ef6/modeling/code-first/fluent/types-and-properties.md)**  è una nuova API di primo codice che consente lo schema del database predefinito per un modello Code First essere configurato in un'unica posizione. In precedenza lo schema predefinito Code First è stato impostati come hardcoded &quot;dbo&quot; ed è stato l'unico modo per configurare lo schema a cui apparteneva una tabella tramite l'API ToTable.
- **Metodo DbModelBuilder.Configurations.AddFromAssembly** consente di aggiungere facilmente tutte le classi di configurazione definite in un assembly quando si utilizzano classi di configurazione con l'API Office Fluent Code First.
- **[Operazioni di migrazioni personalizzate](http://romiller.com/2013/02/27/ef6-writing-your-own-code-first-migration-operations/)**  abilitata è possibile aggiungere operazioni aggiuntive da utilizzare per le migrazioni basate su codice.
- **Livello di isolamento transazione predefinito viene modificato in READ_COMMITTED_SNAPSHOT** per i database creati utilizzando Code First, consentendo maggiore scalabilità e un minor numero di deadlock.
- **Entità e tipi complessi possono ora essere classi nestedinside**. |

## <a name="ef-50"></a>ENTITY FRAMEWORK 5.0
EF 5.0.0 runtime agosto del 2012 è stato rilasciato a NuGet.
Questa versione introduce alcune nuove funzionalità, tra cui il supporto di enumerazione, funzioni con valori di tabella, tipi di dati spaziali e vari miglioramenti delle prestazioni.

Entity Framework Designer in Visual Studio 2012 introduce anche il supporto per più-diagrammi per ogni modello, colorazione delle forme dell'importazione di batch e nell'area di progettazione di stored procedure.

Ecco un elenco del contenuto che viene raccolto in modo specifico per la versione di Entity Framework 5.

-   [Entity Framework versione 5 Post](http://blogs.msdn.com/b/adonet/archive/2012/08/15/ef5-released.aspx)
-   Nuove funzionalità in EF5
    -   [Enumerazione supportare prima di tutto nel codice](~/ef6/modeling/code-first/data-types/enums.md)
    -   [Supporto di enumerazione nella finestra di progettazione di Entity Framework](~/ef6/modeling/designer/data-types/enums.md)
    -   [Tipi di dati spaziali nel codice prima di tutto](~/ef6/modeling/code-first/data-types/spatial.md)
    -   [Tipi di dati spaziali nella finestra di progettazione di Entity Framework](~/ef6/modeling/designer/data-types/spatial.md)
    -   [Supporto del provider per i tipi spaziali](~/ef6/fundamentals/providers/spatial-support.md)
    -   [Funzioni con valori di tabella](~/ef6/modeling/designer/advanced/tvfs.md)
    -   [Più diagrammi per ogni modello](~/ef6/modeling/designer/multiple-diagrams.md)
-   Configurazione del modello
    -   [Creazione di un modello](~/ef6/modeling/index.md)
    -   [Modelli e le connessioni](~/ef6/fundamentals/configuring/connection-strings.md)
    -   [Considerazioni sulle prestazioni](~/ef6/fundamentals/performance/perf-whitepaper.md)
    -   [Utilizzo di Microsoft SQL Azure](~/ef6/fundamentals/connection-resiliency/retry-logic.md)
    -   [Impostazioni del File di configurazione](~/ef6/fundamentals/configuring/config-file.md)
    -   [Glossario](~/ef6/resources/glossary.md)
    -   Code First
        -   [Code First per un nuovo database (procedura dettagliata e video)](~/ef6/modeling/code-first/workflows/new-database.md)
        -   [Code First per un database esistente (procedura dettagliata e video)](~/ef6/modeling/code-first/workflows/existing-database.md)
        -   [Convenzioni](~/ef6/modeling/code-first/conventions/built-in.md)
        -   [Annotazioni dei dati](~/ef6/modeling/code-first/data-annotations.md)
        -   [API Fluent - configurazione/Mapping tipi di & proprietà](~/ef6/modeling/code-first/fluent/types-and-properties.md)
        -   [API Fluent - configurazione di relazioni](~/ef6/modeling/code-first/fluent/relationships.md)
        -   [API Fluent con Visual Basic.NET](~/ef6/modeling/code-first/fluent/vb.md)
        -   [Migrazioni Code First](~/ef6/modeling/code-first/migrations/index.md)
        -   [Migrazioni automatiche Code First](~/ef6/modeling/code-first/migrations/automatic.md)
        -   [Migrate.exe](~/ef6/modeling/code-first/migrations/migrate-exe.md)
        -   [La definizione di DbSet](~/ef6/modeling/code-first/dbsets.md)
    -   Finestra di progettazione di Entity Framework
        -   [Primo modello (procedura dettagliata e video)](~/ef6/modeling/designer/workflows/model-first.md)
        -   [Database First (procedura dettagliata e video)](~/ef6/modeling/designer/workflows/database-first.md)
        -   [Tipi complessi](~/ef6/modeling/designer/data-types/complex-types.md)
        -   [Le associazioni/relazioni](~/ef6/modeling/designer/relationships.md)
        -   [Modello di ereditarietà tabella per tipo](~/ef6/modeling/designer/inheritance/tpt.md)
        -   [Modello di ereditarietà tabella per gerarchia](~/ef6/modeling/designer/inheritance/tph.md)
        -   [Query con le Stored procedure](~/ef6/modeling/designer/stored-procedures/query.md)
        -   [Stored procedure con più set di risultati](~/ef6/modeling/designer/advanced/multiple-result-sets.md)
        -   [Inserimento, aggiornamento ed eliminazione con le Stored procedure](~/ef6/modeling/designer/stored-procedures/cud.md)
        -   [Eseguire il mapping di un'entità a più tabelle (suddivisione di entità)](~/ef6/modeling/designer/entity-splitting.md)
        -   [Eseguire il mapping di più entità a una tabella (la suddivisione di tabelle)](~/ef6/modeling/designer/table-splitting.md)
        -   [Definizione di query](~/ef6/modeling/designer/advanced/defining-query.md)
        -   [Modelli di generazione di codice](~/ef6/modeling/designer/codegen/index.md)
        -   [Ripristino di ObjectContext](~/ef6/modeling/designer/codegen/legacy-objectcontext.md)
-   Usando il modello
    -   [Utilizzo di DbContext](~/ef6/fundamentals/working-with-dbcontext.md)
    -   [L'esecuzione di query/ricerca entità](~/ef6/querying/index.md)
    -   [Utilizzo delle relazioni](~/ef6/fundamentals/relationships.md)
    -   [Il caricamento di entità correlate](~/ef6/querying/related-data.md)
    -   [Utilizzo di dati locali](~/ef6/querying/local-data.md)
    -   [Applicazioni a più livelli](~/ef6/fundamentals/disconnected-entities/index.md)
    -   [Query SQL non elaborate](~/ef6/querying/raw-sql.md)
    -   [Modelli di concorrenza ottimistica](~/ef6/saving/concurrency.md)
    -   [Utilizzo di proxy](~/ef6/fundamentals/proxies.md)
    -   [Automatica rilevare le modifiche](~/ef6/saving/change-tracking/auto-detect-changes.md)
    -   [Query senza registrazione](~/ef6/querying/no-tracking.md)
    -   [Metodo Load](~/ef6/querying/load-method.md)
    -   [Aggiungere o collegare e stati di entità](~/ef6/saving/change-tracking/entity-state.md)
    -   [Utilizzo di valori di proprietà](~/ef6/saving/change-tracking/property-values.md)
    -   [Data Binding WPF (Windows Presentation Foundation)](~/ef6/fundamentals/databinding/wpf.md)
    -   [Data Binding con Windows Form (Windows Form)](~/ef6/fundamentals/databinding/winforms.md)

## <a name="ef-431"></a>EF 4.3.1
EF 4.3.1 runtime è stato rilasciato a NuGet nel febbraio 2012 poco dopo EF 4.3.0.
Questa versione patch incluse alcune correzioni di bug nella versione 4.3 di EF e ha introdotto il supporto di Local DB migliorato per i clienti che usano EF 4.3 con Visual Studio 2012.

Ecco un elenco del contenuto che viene raccolto in modo specifico per EF 4.3.1 della versione, la maggior parte dei contenuti disponibili per Entity Framework 4.1 sono ancora valide per Entity Framework 4.3 anche.

-   [Versione di Entity Framework 4.3.1 Post di Blog](http://blogs.msdn.com/b/adonet/archive/2012/02/29/ef4-3-1-and-ef5-beta-1-available-on-nuget.aspx)

## <a name="ef-43"></a>EF 4.3
EF 4.3.0 runtime febbraio del 2012 è stato rilasciato a NuGet.
Questa versione è inclusa la nuova funzionalità migrazioni Code First che consente a un database creato da Code First per essere modificato in modo incrementale con il modello Code First evoluzione.

Ecco un elenco del contenuto che viene raccolto in modo specifico per la versione 4.3 di Entity Framework, la maggior parte dei contenuti disponibili per Entity Framework 4.1 sono ancora valide per Entity Framework 4.3 anche:
-   [Entity Framework versione 4.3 Post](http://blogs.msdn.com/b/adonet/archive/2012/02/09/ef-4-3-released.aspx)
-   [Procedura dettagliata le migrazioni basate su codice 4.3 di Entity Framework](http://blogs.msdn.com/b/adonet/archive/2012/02/09/ef-4-3-code-based-migrations-walkthrough.aspx)
-   [Procedura dettagliata 4.3 migrazioni automatiche di Entity Framework](http://blogs.msdn.com/b/adonet/archive/2012/02/09/ef-4-3-automatic-migrations-walkthrough.aspx)

## <a name="ef-42"></a>ENTITY FRAMEWORK 4.2
EF 4.2.0 runtime è stato rilasciato a NuGet nel novembre 2011.
Questa versione include correzioni di bug per EF 4.1.1 di rilascio.
Poiché questa versione è inclusa solo versione di patch di correzioni di bug potrebbe essersi verificati EF 4.1.2 ma abbiamo optato per spostare in 4.2 ci permettono di passare dalla data basati su numeri di versione patch abbiamo utilizzato la 4.1.x rilascia e adottano le [semantica Versionsing](https://semver.org) standard per il controllo delle versioni semantico.

Ecco un elenco del contenuto che viene raccolto in modo specifico per la versione di Entity Framework 4.2, il contenuto specificato per Entity Framework 4.1 si applica comunque a Entity Framework 4.2 anche.

-   [Post versione 4.2 di Entity Framework](http://blogs.msdn.com/b/adonet/archive/2011/11/01/ef-4-2-released.aspx)
-   [Procedura dettagliata del primo codice](http://blogs.msdn.com/b/adonet/archive/2011/09/28/ef-4-2-code-first-walkthrough.aspx)
-   [Modello di & Database prima procedura dettagliata](http://blogs.msdn.com/b/adonet/archive/2011/09/28/ef-4-2-model-amp-database-first-walkthrough.aspx)

## <a name="ef-411"></a>EF 4.1.1
EF 4.1.10715 runtime è stato rilasciato a NuGet di luglio 2011.
Oltre a correzioni di bug questa versione patch introdotto alcuni componenti per renderne più semplice per la fase di progettazione degli strumenti per lavorare con un modello Code First.
Questi componenti vengono usati da EF Power Tools e migrazioni Code First (incluso in Entity Framework 4.3).

Si noterà che la versione strano numerica 4.1.10715 del pacchetto.
È stato usato da usare le versioni patch data basata prima che abbiamo deciso di adottare [Versionamento semantico](https://semver.org).
Considerare questa versione di patch di Entity Framework 4.1 1 (o EF 4.1.1).

Ecco un elenco di contenuto è raccolto per il 4.1.1 versione:

-   [EF 4.1.1 Release Post](http://blogs.msdn.com/b/adonet/archive/2011/07/25/ef-4-1-update-1-released.aspx)

## <a name="ef-41"></a>ENTITY FRAMEWORK 4.1
EF 4.1.10331 runtime è stato il primo a essere pubblicata in NuGet, in aprile 2011.
Questa versione è inclusa l'API DbContext semplificata e il flusso di lavoro di Code First.

Si noterà il numero di versione strano, 4.1.10331, che devono effettivamente sono stati compilati 4.1. Inoltre è presente un 4.1.10311 versione che avrebbe dovuto essere 4.1.0-rc ('rc' è l'acronimo di 'versione finale candidata').
È stato usato da usare le versioni patch data basata prima che abbiamo deciso di adottare [Versionamento semantico](https://semver.org).

Ecco un elenco di contenuti che è raccolto per la versione 4.1. Gran parte di esso sono ancora valide per le versioni successive di Entity Framework:

-   [Entity Framework 4.1 versione Post](http://blogs.msdn.com/b/adonet/archive/2011/04/11/ef-4-1-released.aspx)
-   [Procedura dettagliata del primo codice](http://blogs.msdn.com/b/adonet/archive/2011/03/15/ef-4-1-code-first-walkthrough.aspx)
-   [Modello di & Database prima procedura dettagliata](http://blogs.msdn.com/b/adonet/archive/2011/03/15/ef-4-1-model-amp-database-first-walkthrough.aspx)
-   [SQL Azure federazioni ed Entity Framework](http://blogs.msdn.com/b/adonet/archive/2012/01/10/sql-azure-federations-and-the-entity-framework.aspx)

## <a name="ef-40"></a>EF 4.0
Questa versione è stato incluso in .NET Framework 4 e Visual Studio 2010, in aprile del 2010.
Nuove funzionalità importanti in questa versione inclusi POCO supporto, mapping di chiave esterna, il caricamento lazy, miglioramenti della testabilità, la generazione del codice personalizzabile e il primo modello flusso di lavoro.

Anche se era la seconda release di Entity Framework, Entity Framework 4 in modo da allinearsi con la versione di .NET Framework fornito con era denominato.
Dopo questa versione, abbiamo avviato rende Entity Framework disponibile in NuGet e adottato versionamento semantico perché abbiamo stavamo non più associati alla versione di .NET Framework.

Si noti che alcune versioni successive di .NET Framework spediti con aggiornamenti significativi ai bit di Entity Framework incluso.
Infatti, molte delle nuove funzionalità di Entity Framework 5.0 sono stati implementati come miglioramenti su questi bit.
Tuttavia, per razionalizzare la storia di controllo delle versioni per Entity Framework, continuiamo a fare riferimento ai bit di Entity Framework che fanno parte di .NET Framework come il runtime di Entity Framework 4.0, mentre tutte le versioni più recenti sono costituiti i [pacchetto NuGet EntityFramework](https://www.nuget.org/packages/EntityFramework/).         

## <a name="ef-35"></a>ENTITY FRAMEWORK 3.5
La versione iniziale di Entity Framework è stato incluso in Visual Studio 2008 SP1, rilasciata nell'agosto del 2008 e .NET 3.5 Service Pack 1.
Questa versione è fornito il supporto di base O/RM usando il flusso di lavoro Database First.
