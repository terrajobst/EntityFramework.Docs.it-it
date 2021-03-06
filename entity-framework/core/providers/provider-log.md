---
title: Log delle modifiche che influiscano sul provider-EF Core
author: ajcvickers
ms.author: avickers
ms.date: 08/08/2018
ms.assetid: 7CEF496E-A5B0-4F5F-B68E-529609B23EF9
ms.technology: entity-framework-core
uid: core/providers/provider-log
ms.openlocfilehash: b911a2da493e20c4e4ce6f1e25024bd0efd38b44
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78417822"
---
# <a name="provider-impacting-changes"></a>Modifiche che influiscono sul provider

Questa pagina contiene collegamenti alle richieste pull effettuate sul repository EF Core che possono richiedere agli autori di altri provider di database di rispondere. L'intenzione è fornire un punto di partenza per gli autori di provider di database di terze parti esistenti durante l'aggiornamento del provider a una nuova versione.

Questo log verrà avviato con le modifiche da 2,1 a 2,2. Prima del 2,1 abbiamo usato il [`providers-beware`](https://github.com/aspnet/EntityFrameworkCore/labels/providers-beware) e [`providers-fyi`](https://github.com/aspnet/EntityFrameworkCore/labels/providers-fyi) etichette sui problemi e sulle richieste pull.

## <a name="22-----30"></a>2,2---> 3,0

Si noti che molte delle [modifiche di rilievo a livello di applicazione](../what-is-new/ef-core-3.0/breaking-changes.md) influiscano anche sui provider.

* <https://github.com/aspnet/EntityFrameworkCore/pull/14022>
  * Sono state rimosse le API obsolete e gli overload di parametro facoltativi compressi
  * Rimozione di DatabaseColumn. GetUnderlyingStoreType ()
* <https://github.com/aspnet/EntityFrameworkCore/pull/14589>
  * API obsolete rimosse
* <https://github.com/aspnet/EntityFrameworkCore/pull/15044>
  * Le sottoclassi di CharTypeMapping potrebbero essere state interrotte a causa di modifiche del comportamento necessarie per la correzione di un paio di bug nell'implementazione di base.
* <https://github.com/aspnet/EntityFrameworkCore/pull/15090>
  * Aggiunta di una classe di base per IDatabaseModelFactory e aggiornamento per l'uso di un oggetto di parametro per attenuare le interruzioni future.
* <https://github.com/aspnet/EntityFrameworkCore/pull/15123>
  * Oggetti Parameter usati in MigrationsSqlGenerator per mitigare le interruzioni future.
* <https://github.com/aspnet/EntityFrameworkCore/pull/14972>
  * La configurazione esplicita dei livelli di log richiedeva alcune modifiche alle API che i provider potrebbero usare. In particolare, se i provider utilizzano direttamente l'infrastruttura di registrazione, questa modifica potrebbe interrompere l'utilizzo di. Inoltre, i provider che usano l'infrastruttura (che sarà pubblica) in futuro dovranno derivare da `LoggingDefinitions` o `RelationalLoggingDefinitions`. Per esempi, vedere la SQL Server e i provider in memoria.
* <https://github.com/aspnet/EntityFrameworkCore/pull/15091>
  * Le stringhe di risorse core, relazionali e astratte sono ora pubbliche.
  * `CoreLoggerExtensions` e `RelationalLoggerExtensions` sono ora pubblici. I provider devono usare queste API per la registrazione di eventi definiti a livello di core o relazionale. Non accedere direttamente alle risorse di registrazione; Questi sono ancora interni.
  * `IRawSqlCommandBuilder` è stato modificato da un servizio singleton a un servizio con ambito
  * `IMigrationsSqlGenerator` è stato modificato da un servizio singleton a un servizio con ambito
* <https://github.com/aspnet/EntityFrameworkCore/pull/14706>
  * L'infrastruttura per la creazione di comandi relazionali è stata resa pubblica, in modo che possa essere usata in modo sicuro dai provider e sottoposto a refactoring leggermente.
* <https://github.com/aspnet/EntityFrameworkCore/pull/14733>
  * `ILazyLoader` è stato modificato da un servizio con ambito a un servizio temporaneo
* <https://github.com/aspnet/EntityFrameworkCore/pull/14610>
  * `IUpdateSqlGenerator` è stato modificato da un servizio con ambito a un servizio singleton
  * Inoltre, `ISingletonUpdateSqlGenerator` è stato rimosso
* <https://github.com/aspnet/EntityFrameworkCore/pull/15067>
  * Un numero elevato di codice interno usato dai provider è ora reso pubblico
  * Non deve più essere necssary per fare riferimento a `IndentedStringBuilder` perché è stato fattorizzato dalle posizioni che lo hanno esposto
  * Gli utilizzi di `NonCapturingLazyInitializer` devono essere sostituiti con `LazyInitializer` dalla libreria di classi base
* <https://github.com/aspnet/EntityFrameworkCore/pull/14608>
  * Questa modifica è interamente analizzata nel documento modifiche di rilievo dell'applicazione. Per i provider, questo potrebbe avere un impatto maggiore perché il test di EF core può spesso comportare la causa di questo problema, quindi l'infrastruttura di test è cambiata per renderlo meno probabile.
* <https://github.com/aspnet/EntityFrameworkCore/issues/13961>
  * `EntityMaterializerSource` è stato semplificato
* <https://github.com/aspnet/EntityFrameworkCore/pull/14895>
  * La traduzione StartsWith è stata modificata in modo che i provider potessero voler rispondere
* <https://github.com/aspnet/EntityFrameworkCore/pull/15168>
  * I servizi dei set di convenzioni sono stati modificati. I provider devono ora ereditare da "ProviderConventionSet" o "RelationalConventionSet".
  * Le personalizzazioni possono essere aggiunte tramite `IConventionSetCustomizer` Services, ma questo è destinato a essere utilizzato da altre estensioni, non da provider.
  * Le convenzioni usate in fase di esecuzione devono essere risolte da `IConventionSetBuilder`.
* <https://github.com/aspnet/EntityFrameworkCore/pull/15288>
  * Il seeding dei dati è stato sottoposto a refactoring in un'API pubblica per evitare la necessità di usare tipi interni. Questa operazione dovrebbe influisca solo sui provider non relazionali, poiché il seeding viene gestito dalla classe relazionale di base per tutti i provider relazionali.

## <a name="21-----22"></a>2,1---> 2,2

### <a name="test-only-changes"></a>Modifiche solo test

* <https://github.com/aspnet/EntityFrameworkCore/pull/12057>-Consenti delimitatori SQL personalizzabili nei test
  * Testare le modifiche che consentono confronti non limitati a virgola mobile in BuiltInDataTypesTestBase
  * Testare le modifiche che consentono di riutilizzare i test di query con delimitatori SQL diversi
* <https://github.com/aspnet/EntityFrameworkCore/pull/12072>-aggiungere test DbFunction ai test delle specifiche relazionali
  * Tali test possono essere eseguiti su tutti i provider di database
* pulizia del test <https://github.com/aspnet/EntityFrameworkCore/pull/12362> asincrono
  * Rimuovere `Wait` chiamate, async non necessario e rinominare alcuni metodi di test
* <https://github.com/aspnet/EntityFrameworkCore/pull/12666> unificare l'infrastruttura di test di registrazione
  * Aggiunta `CreateListLoggerFactory` e rimozione di un'infrastruttura di registrazione precedente, che richiederà ai provider di usare questi test per rispondere
* <https://github.com/aspnet/EntityFrameworkCore/pull/12500> eseguire più test di query sia in modo sincrono che asincrono
  * I nomi dei test e il factoring sono stati modificati, per cui è necessario che i provider utilizzino questi test per rispondere
* <https://github.com/aspnet/EntityFrameworkCore/pull/12766>-rinominare le navigazioni nel modello ComplexNavigations
  * I provider che usano questi test potrebbero dover reagire
* <https://github.com/aspnet/EntityFrameworkCore/pull/12141>-restituisce il contesto al pool anziché eliminarlo nei test funzionali
  * Questa modifica include il refactoring dei test che può richiedere che i provider reagiscano

### <a name="test-and-product-code-changes"></a>Modifiche al codice prodotto e di test

* <https://github.com/aspnet/EntityFrameworkCore/pull/12109>: consolidare i metodi RelationalTypeMapping. Clone
  * Le modifiche apportate a 2,1 al RelationalTypeMapping sono consentite per semplificare le classi derivate. Questa modifica non è credibile nei provider, ma i provider possono sfruttare questa modifica nelle classi di mapping dei tipi derivati.
* query denominate o con tag <https://github.com/aspnet/EntityFrameworkCore/pull/12069>
  * Aggiunge l'infrastruttura per l'assegnazione di tag alle query LINQ e la visualizzazione di tali tag come commenti in SQL. Questa operazione può richiedere che i provider reagiscono nella generazione SQL.
* <https://github.com/aspnet/EntityFrameworkCore/pull/13115> supportare i dati spaziali tramite NTS
  * Consente la registrazione di mapping dei tipi e traduttori di membri all'esterno del provider
    * I provider devono chiamare base. FindMapping () nell'implementazione di ITypeMappingSource per il funzionamento
  * Seguire questo modello per aggiungere il supporto spaziale al provider coerente tra i provider.
* <https://github.com/aspnet/EntityFrameworkCore/pull/13199>-aggiungere il debug migliorato per la creazione del provider di servizi
  * Consente a DbContextOptionsExtensions di implementare una nuova interfaccia che può aiutare gli utenti a comprendere il motivo per cui il provider di servizi interno viene ricompilato
* <https://github.com/aspnet/EntityFrameworkCore/pull/13289>: aggiunge l'API CanConnect per l'uso da controlli di integrità
  * Questa richiesta pull aggiunge il concetto di `CanConnect` che verrà usato dai controlli di integrità ASP.NET Core per determinare se il database è disponibile. Per impostazione predefinita, l'implementazione relazionale chiama semplicemente `Exist`, ma i provider possono implementare elementi diversi, se necessario. I provider non relazionali dovranno implementare la nuova API affinché il controllo di integrità sia utilizzabile.
* <https://github.com/aspnet/EntityFrameworkCore/pull/13306>-aggiornare RelationalTypeMapping di base per non impostare la dimensione DbParameter
  * Per impostazione predefinita, interrompere l'impostazione delle dimensioni poiché può causare il troncamento. È possibile che i provider debbano aggiungere la propria logica se è necessario impostare la dimensione.
* <https://github.com/aspnet/EntityFrameworkCore/pull/13372>-RevEng: specificare sempre il tipo di colonna per le colonne decimali
  * Configurare sempre il tipo di colonna per le colonne decimali nel codice con impalcature anziché configurare per convenzione.
  * I provider non devono richiedere alcuna modifica al termine.
* <https://github.com/aspnet/EntityFrameworkCore/pull/13469>: aggiunge CaseExpression per la generazione di espressioni CASE SQL
* <https://github.com/aspnet/EntityFrameworkCore/pull/13648>: aggiunge la possibilità di specificare i mapping dei tipi in SqlFunctionExpression per migliorare l'inferenza del tipo di archivio degli argomenti e dei risultati.
