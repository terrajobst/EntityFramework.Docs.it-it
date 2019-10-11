---
title: Strumenti ed estensioni - EF Core
author: ErikEJ
ms.date: 01/07/2019
ms.assetid: 14fffb6c-a687-4881-a094-af4a1359a296
uid: core/extensions/index
ms.openlocfilehash: 0c9671eb77181d85cd493341cd1abf842d13fb0e
ms.sourcegitcommit: 708b18520321c587b2046ad2ea9fa7c48aeebfe5
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 10/09/2019
ms.locfileid: "72181266"
---
# <a name="ef-core-tools--extensions"></a>Strumenti ed estensioni di EF Core

Questi strumenti ed estensioni offrono funzionalità aggiuntive per Entity Framework Core 2.0 e versioni successive.

> [!IMPORTANT]  
> Le estensioni sono composte da diversi tipi di origine e non vengono mantenute nell'ambito del progetto di Entity Framework Core. Quando si prende in considerazione un'estensione di terze parti, valutarne con cura gli aspetti relativi a qualità, licenze, compatibilità, supporto e così via, per essere certi che soddisfi i propri requisiti.

## <a name="tools"></a>Strumenti

### <a name="llblgen-pro"></a>LLBLGen Pro

LLBLGen Pro è una soluzione per la modellazione delle entità con supporto per Entity Framework ed Entity Framework Core. Consente di definire facilmente il modello di entità e di eseguirne il mapping al database, usando l'approccio Database-First o Model-First, in modo da iniziare subito a scrivere le query.

[Sito Web](https://www.llblgen.com/)

### <a name="devart-entity-developer"></a>Devart Entity Developer

Entity Developer è una potente finestra di progettazione ORM per ADO.NET Entity Framework, NHibernate, LinqConnect, Telerik Data Access e LINQ to SQL. Supporta la progettazione visiva di modelli EF Core, usando l'approccio con precedenza del modello o precedenza del database e la generazione di codice C# o Visual Basic. 

[Sito Web](https://www.devart.com/entitydeveloper/)

### <a name="ef-core-power-tools"></a>EF Core Power Tools

EF Core Power Tools è un'estensione di Visual Studio 2017 che visualizza diverse attività di progettazione di EF Core in un'interfaccia utente intuitiva. Include la decompilazione di classi DbContext e classi di entità da database esistenti e [pacchetti di applicazione livello dati SQL Server](https://docs.microsoft.com/sql/relational-databases/data-tier-applications/data-tier-applications), la gestione delle migrazioni del database e le visualizzazioni del modello.

[Wiki di GitHub](https://github.com/ErikEJ/EFCorePowerTools/wiki)

### <a name="entity-framework-visual-editor"></a>Entity Framework Visual Editor

Entity Framework Visual Editor è un'estensione di Visual Studio che aggiunge una finestra di progettazione ORM per la progettazione visiva di classi di Entity Framework 6 ed EF Core. Il codice viene generato usando i modelli T4, pertanto può essere personalizzato per soddisfare qualsiasi esigenza. Supporta l'ereditarietà, le associazioni unidirezionali e bidirezionali, le enumerazioni e la possibilità di usare una codifica a colori per le classi e di aggiungere blocchi di testo, per spiegare parti potenzialmente molto complesse del progetto.

[Marketplace](https://marketplace.visualstudio.com/items?itemName=michaelsawczyn.EFDesigner)

### <a name="catfactory"></a>CatFactory

CatFactory è un motore di scaffolding per .NET Core in grado di automatizzare la generazione di classi, entità, configurazioni di mapping e classi repository DbContext da un database SQL Server.

[Repository GitHub](https://github.com/hherzl/CatFactory.EntityFrameworkCore)

### <a name="loresofts-entity-framework-core-generator"></a>Generatore Entity Framework Core di LoreSoft

Il Generatore Entity Framework Core (efg) è uno strumento della riga di comando di .NET Core che genera modelli EF Core da un database esistente con modalità simili a `dotnet ef dbcontext scaffold`, ma supporta anche la [rigenerazione](https://efg.loresoft.com/en/latest/regeneration/) del codice sicuro tramite la sostituzione dell'area o tramite l'analisi dei file di mapping. Lo strumento supporta la generazione di modelli di visualizzazione, la convalida e il codice mapper degli oggetti. 

[Esercitazione](https://www.loresoft.com/Generate-ASP-NET-Web-API)
[Documentazione](https://efg.loresoft.com/en/latest/)

## <a name="extensions"></a>Estensioni

### <a name="microsoftentityframeworkcoreautohistory"></a>Microsoft.EntityFrameworkCore.AutoHistory

Libreria di plug-in che consente di registrare automaticamente le modifiche ai dati eseguite da EF Core in una tabella di cronologia.

[Repository GitHub](https://github.com/Arch/AutoHistory/)

### <a name="microsoftentityframeworkcoredynamiclinq"></a>Microsoft.EntityFrameworkCore.DynamicLinq

Porta .NET Core/.NET Standard di System.Linq.Dynamic che include il supporto asincrono con EF Core.
System.Linq.Dynamic nasce come esempio Microsoft, che indica come costruire dinamicamente query LINQ da espressioni stringa anziché tramite codice.

[Repository GitHub](https://github.com/StefH/System.Linq.Dynamic.Core/)

### <a name="efsecondlevelcachecore"></a>EFSecondLevelCache.Core

Estensione che consente di archiviare i risultati delle query di EF Core in una cache di secondo livello, in modo che le esecuzioni successive delle stesse query evitino l'accesso al database e recuperino i dati direttamente dalla cache.

[Repository GitHub](https://github.com/VahidN/EFSecondLevelCache.Core/)

### <a name="entityframeworkcoreprimarykey"></a>EntityFrameworkCore.PrimaryKey

Questa libreria consente il recupero dei valori chiave primaria (incluse le chiavi composte) da qualsiasi entità come dizionario.

[Repository GitHub](https://github.com/NickStrupat/EntityFramework.PrimaryKey/)

### <a name="entityframeworkcoretypedoriginalvalues"></a>EntityFrameworkCore.TypedOriginalValues

Questa libreria consente l'accesso fortemente tipizzato ai valori originali delle proprietà dell'entità. 

[Repository GitHub](https://github.com/NickStrupat/EntityFramework.TypedOriginalValues/)

### <a name="geco"></a>Geco

Geco (Generator Console) è un generatore di codice semplice basato su un progetto di console, che viene eseguito in .NET Core e usa stringhe interpolate C# per la generazione di codice. Geco include un generatore di modelli inversi per EF Core con supporto di pluralizzazione, singolarizzazione e modelli modificabili. Include anche un generatore di script per dati di inizializzazione e uno strumento di pulizia database.

[Repository GitHub](https://github.com/iQuarc/Geco)

### <a name="linqkitmicrosoftentityframeworkcore"></a>LinqKit.Microsoft.EntityFrameworkCore

LinqKit.Microsoft.EntityFrameworkCore è una versione della libreria LINQKit compatibile con EF Core. LINQKit è un set gratuito di estensioni per gli utenti avanzati di LINQ to SQL ed Entity Framework. Supporta funzionalità avanzate, quali la creazione dinamica di espressioni predicato e l'uso di variabili espressione nelle sottoquery.  

[Repository GitHub](https://github.com/scottksmith95/LINQKit/)

### <a name="neinlinqentityframeworkcore"></a>NeinLinq.EntityFrameworkCore

NeinLinq estende i provider LINQ, ad esempio Entity Framework, e consente il riuso delle funzioni, la riscrittura di query e la creazione di query dinamiche usando predicati e selettori traducibili.

[Repository GitHub](https://github.com/axelheer/nein-linq/)

### <a name="microsoftentityframeworkcoreunitofwork"></a>Microsoft.EntityFrameworkCore.UnitOfWork

Plug-in per Microsoft.EntityFrameworkCore che supporta repository, modelli di unità di lavoro e più database con la transazione distribuita supportata.

[Repository GitHub](https://github.com/Arch/UnitOfWork/)

### <a name="efcorebulkextensions"></a>EFCore.BulkExtensions

Estensioni EF Core per operazioni in blocco (inserimento, aggiornamento, eliminazione).

[Repository GitHub](https://github.com/borisdj/EFCore.BulkExtensions)

### <a name="bricelamentityframeworkcorepluralizer"></a>Bricelam.EntityFrameworkCore.Pluralizer

Aggiunge la pluralizzazione in fase di progettazione a EF Core.

[Repository GitHub](https://github.com/bricelam/EFCore.Pluralizer)

### <a name="pomelofoundationpomeloentityframeworkcoreextensionstosql"></a>PomeloFoundation/Pomelo.EntityFrameworkCore.Extensions.ToSql

Metodo di estensione semplice che consente di ottenere l'istruzione SQL che verrebbe generata da EF Core per una determinata query LINQ in scenari semplici. Il metodo ToSql è limitato agli scenari semplici, perché EF Core può generare più istruzioni SQL per una singola query LINQ, nonché diverse istruzioni SQL a seconda dei valori di parametro.

[Repository GitHub](https://github.com/PomeloFoundation/Pomelo.EntityFrameworkCore.Extensions.ToSql)

### <a name="toolbeltentityframeworkcoreindexattribute"></a>Toolbelt.EntityFrameworkCore.IndexAttribute

Ripresa dell'attributo [Index] per EF Core (con estensione per la compilazione del modello).

[Repository GitHub](https://github.com/jsakamoto/EntityFrameworkCore.IndexAttribute)

### <a name="efcoreinmemoryhelpers"></a>EfCore.InMemoryHelpers

Definisce un wrapper per il Provider di database in memoria EF Core. Ne rende il funzionamento più simile a quello di un provider relazionale.

[Repository GitHub](https://github.com/SimonCropp/EfCore.InMemoryHelpers)

### <a name="efcoretemporalsupport"></a>EFCore.TemporalSupport

Implementazione di supporto temporale per EF Core.

[Repository GitHub](https://github.com/cpoDesign/EFCore.TemporalSupport)

### <a name="entityframeworkcorecacheable"></a>EntityFrameworkCore.Cacheable

Cache della query di secondo livello ad alte prestazioni per EF Core.

[Repository GitHub](https://github.com/SteffenMangold/EntityFrameworkCore.Cacheable)

### <a name="entity-framework-plus"></a>Entity Framework Plus

Estende DbContext con funzionalità quali: filtro di inclusione, controllo, memorizzazione nella cache, query Future, eliminazione in batch, aggiornamento in batch e molte altre.

[Sito Web](https://entityframework-plus.net/)
[Repository GitHub](https://github.com/zzzprojects/EntityFramework-Plus)

### <a name="entity-framework-extensions"></a>Entity Framework Extensions

Estende DbContext con operazioni in blocco ad alte prestazioni: BulkSaveChanges, BulkInsert, BulkUpdate, BulkDelete, BulkMerge e altre.

[Sito Web](https://entityframework-extensions.net/)

### <a name="reconciler"></a>Riconciliatore

Aggiornare un grafo di entità nell'archivio con uno specificato inserendo, aggiornando e rimuovendo le rispettive entità.

[Repository GitHub](https://github.com/jtheisen/reconciler)
