---
title: Strumenti ed estensioni - EF Core
author: ErikEJ
ms.date: 7/3/2018
ms.assetid: 14fffb6c-a687-4881-a094-af4a1359a296
uid: core/extensions/index
ms.openlocfilehash: e9f9a6cbbceeb0379ddb5588b564b0d2a962795f
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 08/27/2018
ms.locfileid: "42995513"
---
# <a name="ef-core-tools--extensions"></a>Strumenti ed estensioni di EF Core

Gli strumenti e le estensioni offrono funzionalità aggiuntive per Entity Framework Core.

> [!IMPORTANT]  
> Le estensioni sono composte da diversi tipi di origine e non vengono mantenute nell'ambito del progetto di Entity Framework Core. Quando si prende in considerazione un'estensione di terze parti, valutarne con cura gli aspetti relativi a qualità, licenze, compatibilità, supporto e così via, per essere certi che soddisfi i propri requisiti.

## <a name="tools"></a>Strumenti

### <a name="llblgen-pro"></a>LLBLGen Pro

LLBLGen Pro è una soluzione per la modellazione delle entità con supporto per Entity Framework ed Entity Framework Core. Consente di definire facilmente il modello di entità e di eseguirne il mapping al database, usando l'approccio Database-First o Model-First, in modo da iniziare subito a scrivere le query.

[Sito Web](https://www.llblgen.com/)

### <a name="devart-entity-developer"></a>Devart Entity Developer

Entity Developer è una potente finestra di progettazione ORM per ADO.NET Entity Framework, NHibernate, LinqConnect, Telerik Data Access e LINQ to SQL. È possibile usare approcci di tipo Model-First e Database-First per progettare il modello ORM e generare per quest'ultimo codice C# o Visual Basic .NET. Introduce nuove modalità di progettazione dei modelli ORM, ottimizza la produttività e semplifica lo sviluppo delle applicazioni di database.

[Sito Web](https://www.devart.com/entitydeveloper/)

### <a name="ef-core-power-tools"></a>EF Core Power Tools

Estensione di Visual Studio 2017+. È possibile decompilare DbContext e le classi POCO da un database esistente o un progetto di database di SQL Server e visualizzare e controllare DbContext in vari modi.

[Wiki di GitHub](https://github.com/ErikEJ/SqlCeToolbox/wiki/EF-Core-Power-Tools)

## <a name="extensions"></a>Estensioni

### <a name="microsoftentityframeworkcoreautohistory"></a>Microsoft.EntityFrameworkCore.AutoHistory

Plug-in per Microsoft.EntityFrameworkCore per supportare la registrazione automatica della cronologia delle modifiche dei dati.

[Repository GitHub](https://github.com/Arch/AutoHistory/)

### <a name="microsoftentityframeworkcoredynamiclinq"></a>Microsoft.EntityFrameworkCore.DynamicLinq

Estensioni Linq dinamiche per Microsoft.EntityFrameworkCore con aggiunta del supporto asincrono

 [Repository GitHub](https://github.com/StefH/System.Linq.Dynamic.Core/)

### <a name="efcorepractices"></a>EFCore.Practices

Tentativo di acquisire alcune procedure consigliate in un'API che supporta i test, tra cui un piccolo framework per l'analisi delle query N+1.

[Repository GitHub](https://github.com/riezebosch/efcore-practices/tree/master/src/EFCore.Practices/)

### <a name="efsecondlevelcachecore"></a>EFSecondLevelCache.Core

Libreria con memorizzazione nella cache di secondo livello. La memorizzazione nella cache di secondo livello è una cache della query. I risultati dei comandi EF verranno memorizzati nella cache, in modo che gli stessi comandi EF recupereranno i dati dalla cache anziché eseguirli di nuovo sul database.

[Repository GitHub](https://github.com/VahidN/EFSecondLevelCache.Core/)

### <a name="detachedentityframework"></a>Detached.EntityFramework

Carica e salva i grafici di un'intera entità scollegata, ovvero l'entità con i relativi elenchi ed entità figlio. Ispirato da [GraphDiff](https://github.com/refactorthis/GraphDiff/). L'idea prevede anche l'aggiunta di alcuni plug-in per semplificare alcune attività ripetitive, ad esempio il controllo e l'impaginazione.

[Repository GitHub](https://github.com/leonardoporro/Detached/)

### <a name="entityframeworkcoreprimarykey"></a>EntityFrameworkCore.PrimaryKey

Recupero della chiave primaria (incluse le chiavi composte) da qualsiasi entità come dizionario.

[Repository GitHub](https://github.com/NickStrupat/EntityFramework.PrimaryKey/)

### <a name="entityframeworkcorerx"></a>EntityFrameworkCore.Rx

Wrapper di estensione reattivi per gli oggetti osservabili delle entità di Entity Framework.

[Repository GitHub](https://github.com/NickStrupat/EntityFramework.Rx/)

### <a name="entityframeworkcoretriggers"></a>EntityFrameworkCore.Triggers

Aggiungere trigger alle entità con eventi di inserimento, aggiornamento ed eliminazione. Esistono tre eventi per ogni oggetto: prima, dopo e in caso di errore.

[Repository GitHub](https://github.com/NickStrupat/EntityFramework.Triggers/)

### <a name="entityframeworkcoretypedoriginalvalues"></a>EntityFrameworkCore.TypedOriginalValues

Ottenere l'accesso tipizzato all'elemento OriginalValue delle proprietà dell'entità. Le proprietà semplici e complesse sono supportate, la navigazione e le raccolte non lo sono.

[Repository GitHub](https://github.com/NickStrupat/EntityFramework.TypedOriginalValues/)

### <a name="geco"></a>Geco

Geco offre un generatore di modelli di reverse engineering con supporto per pluralizzazione e singolarizzazione e modelli modificabili basati su stringhe C# 6.0 interpolate e in esecuzione su .Net Core. Offre anche un generatore di script di inizializzazione con script merge SQL e uno strumento di esecuzione script.

[Repository GitHub](https://github.com/iQuarc/Geco)

### <a name="linqkitmicrosoftentityframeworkcore"></a>LinqKit.Microsoft.EntityFrameworkCore

LinqKit.Microsoft.EntityFrameworkCore è un set gratuito di estensioni per utenti esperti di LINQ to SQL ed EntityFrameworkCore. Con supporto Include(...) e IDbAsync.

[Repository GitHub](https://github.com/scottksmith95/LINQKit/)

### <a name="neinlinqentityframeworkcore"></a>NeinLinq.EntityFrameworkCore

NeinLinq.EntityFrameworkCore offre estensioni utili per l'uso dei provider LINQ, ad esempio Entity Framework, che supportano solo un subset limitato di funzioni .NET, riusando le funzioni, riscrivendo le query, rendendole anche null-safe, e compilando le query dinamiche usando predicati e selettori traducibili.

[Repository GitHub](https://github.com/axelheer/nein-linq/)

### <a name="microsoftentityframeworkcoreunitofwork"></a>Microsoft.EntityFrameworkCore.UnitOfWork

Plug-in per Microsoft.EntityFrameworkCore supportare il repository, i modelli di unità di lavoro e più database con transazione distribuita supportata.

[Repository GitHub](https://github.com/Arch/UnitOfWork/)

### <a name="entityframeworklazyloading"></a>EntityFramework.LazyLoading

Caricamento lazy per EF Core 1.1

[Repository GitHub](https://github.com/darxis/EntityFramework.LazyLoading)

### <a name="efcorebulkextensions"></a>EFCore.BulkExtensions

Estensioni EntityFrameworkCore per operazioni in blocco (inserimento, aggiornamento, eliminazione).

[Repository GitHub](https://github.com/borisdj/EFCore.BulkExtensions)

### <a name="bricelamentityframeworkcorepluralizer"></a>Bricelam.EntityFrameworkCore.Pluralizer

Aggiunge la pluralizzazione in fase di progettazione a EF Core.

[Repository GitHub](https://github.com/bricelam/EFCore.Pluralizer)
