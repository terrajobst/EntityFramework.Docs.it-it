---
title: Log delle modifiche che influiscono sul provider - EF Core
author: ajcvickers
ms.author: avickers
ms.date: 08/08/2018
ms.assetid: 7CEF496E-A5B0-4F5F-B68E-529609B23EF9
ms.technology: entity-framework-core
uid: core/providers/provider-log
ms.openlocfilehash: 70fe2d934901f5366c96904b08f49a35f6590b47
ms.sourcegitcommit: 6c4e06bc62d98442530e93a44725e38e59483d42
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/18/2019
ms.locfileid: "58131402"
---
# <a name="provider-impacting-changes"></a>Modifiche che influiscono sul provider

Questa pagina contiene collegamenti per eseguire il pull le richieste effettuate nel repository EF Core che possono richiedere gli autori di altri provider di database per rispondere. Lo scopo consiste nel fornire un punto di partenza per gli autori di provider di database di terze parti esistenti quando si aggiorna il provider a una nuova versione.

Questo log è iniziata con le modifiche da 2.1 a 2.2. Prima di 2.1 è stato usato il [ `providers-beware` ](https://github.com/aspnet/EntityFrameworkCore/labels/providers-beware) e [ `providers-fyi` ](https://github.com/aspnet/EntityFrameworkCore/labels/providers-fyi) etichette nei nostri problemi e richieste pull.

## <a name="22-----30"></a>2.2 ---> 3.0

* https://github.com/aspnet/EntityFrameworkCore/pull/14022
  * API obsolete rimosse e gli overload di parametro facoltativo compressa
  * DatabaseColumn.GetUnderlyingStoreType() rimosso
* https://github.com/aspnet/EntityFrameworkCore/pull/14589
  * Rimuovere API obsolete
* https://github.com/aspnet/EntityFrameworkCore/pull/15044
  * Le sottoclassi di CharTypeMapping potrebbero essere stato interrotte a causa di modifiche al comportamento necessarie per la correzione di alcuni bug nell'implementazione di base.

## <a name="21-----22"></a>2.1 ---> 2.2

### <a name="test-only-changes"></a>Modifiche apportate a solo scopo di test

* [https://github.com/aspnet/EntityFrameworkCore/pull/12057](https://github.com/aspnet/EntityFrameworkCore/pull/12057) -Consenti personalizzabile delimeters SQL nei test
  * Testare le modifiche che consentono di BuiltInDataTypesTestBase non strict confronti completi dei punti a virgola mobile
  * Modifiche di test che consentono i test di query essere riutilizzata con diversi delimeters SQL
* [https://github.com/aspnet/EntityFrameworkCore/pull/12072](https://github.com/aspnet/EntityFrameworkCore/pull/12072) -Aggiungere test DbFunction ai test relazionale specifica
  * In modo che questi test possono essere eseguiti su tutti i provider di database
* [https://github.com/aspnet/EntityFrameworkCore/pull/12362](https://github.com/aspnet/EntityFrameworkCore/pull/12362) -Pulizia del test Async
  * Rimuovere `Wait` chiamate, non necessari async e rinominati alcuni metodi di test
* [https://github.com/aspnet/EntityFrameworkCore/pull/12666](https://github.com/aspnet/EntityFrameworkCore/pull/12666) -Unificare l'infrastruttura di test di registrazione
  * Aggiunto `CreateListLoggerFactory` e rimossi alcuni precedente infrastruttura di registrazione, in modo da richiederla provider utilizzando questi test per rispondere
* [https://github.com/aspnet/EntityFrameworkCore/pull/12500](https://github.com/aspnet/EntityFrameworkCore/pull/12500) -Eseguire altri test di query sia in modo sincrono e asincrono
  * I nomi di test e le stesse caratteristiche è stato modificato, che richiederà l'uso nei provider tramite questi test per rispondere
* [https://github.com/aspnet/EntityFrameworkCore/pull/12766](https://github.com/aspnet/EntityFrameworkCore/pull/12766) -Spostamenti a ridenominazione del modello ComplexNavigations
  * Provider di questi test potrebbe essere necessario rispondere
* [https://github.com/aspnet/EntityFrameworkCore/pull/12141](https://github.com/aspnet/EntityFrameworkCore/pull/12141) -Restituire il contesto per il pool anziché eliminare in test funzionali
  * Questa modifica include alcuni test di refactoring che può richiedere ai provider di react


### <a name="test-and-product-code-changes"></a>Modifiche al codice di test e prodotto

* [https://github.com/aspnet/EntityFrameworkCore/pull/12109](https://github.com/aspnet/EntityFrameworkCore/pull/12109) -Consolidare RelationalTypeMapping.Clone metodi
  * Le modifiche in 2.1 per il RelationalTypeMapping consentite per semplificare il lavoro per le classi derivate. Non si ritiene che questo era di rilievo ai provider, ma i provider possono sfruttare i vantaggi di questa modifica nel relativo tipo derivato mapping di classi.
* [https://github.com/aspnet/EntityFrameworkCore/pull/12069](https://github.com/aspnet/EntityFrameworkCore/pull/12069) -Query denominate o tag
  * Aggiunge l'infrastruttura per l'assegnazione di tag query LINQ e presenza di tali tag visualizzato come commenti in SQL. Ciò può richiedere ai provider di reagire in generazione SQL.
* [https://github.com/aspnet/EntityFrameworkCore/pull/13115](https://github.com/aspnet/EntityFrameworkCore/pull/13115) -Supportano i dati spaziali tramite NTS
  * Consente di mapping dei tipi e membri perché i traduttori possano essere registrati di fuori del provider
    * I provider devono chiamare base. FindMapping() nella propria implementazione ITypeMappingSource per farli funzionare
  * Seguire questo modello per aggiungere il supporto spaziale per il provider che è coerenza per tutti i provider.
* [https://github.com/aspnet/EntityFrameworkCore/pull/13199](https://github.com/aspnet/EntityFrameworkCore/pull/13199) -Aggiungere debug avanzato per la creazione di provider del servizio
  * Consente a DbContextOptionsExtensions implementare una nuova interfaccia che può aiutare le persone a comprendere il motivo per cui il provider di servizi interna è in fase di ricompilazione
* [https://github.com/aspnet/EntityFrameworkCore/pull/13289](https://github.com/aspnet/EntityFrameworkCore/pull/13289) -Aggiunge CanConnect API per l'uso da controlli di integrità
  * Questa richiesta pull viene aggiunto il concetto di `CanConnect` che verrà utilizzata da health di ASP.NET Core verifica per determinare se il database è disponibile. Per impostazione predefinita, l'implementazione relazionale chiama semplicemente `Exist`, anche se i provider possono implementare un elemento diverso se necessario. Provider relazionali dovrà implementare la nuova API in ordine per il controllo di integrità sia utilizzabile.
* [https://github.com/aspnet/EntityFrameworkCore/pull/13306](https://github.com/aspnet/EntityFrameworkCore/pull/13306) -Aggiornare base RelationalTypeMapping per non impostare dimensioni DbParameter
  * Arrestare l'impostazione delle dimensioni per impostazione predefinita, in quanto può causare il troncamento. I provider potrebbero essere necessario aggiungere la propria logica se sono necessario impostare le dimensioni.
* https://github.com/aspnet/EntityFrameworkCore/pull/13372 -RevEng: Specificare sempre il tipo di colonna per le colonne decimale
  * Configurare sempre il tipo di colonna per le colonne decimale in codice con scaffolding anziché la configurazione per convenzione.
  * I provider non dovrebbero richiedere eventuali modifiche estremità.
* [https://github.com/aspnet/EntityFrameworkCore/pull/13469](https://github.com/aspnet/EntityFrameworkCore/pull/13469) -Aggiunge CaseExpression per la generazione di espressioni CASE SQL
* [https://github.com/aspnet/EntityFrameworkCore/pull/13648](https://github.com/aspnet/EntityFrameworkCore/pull/13648) -Aggiunge la possibilità di specificare i mapping dei tipi in SqlFunctionExpression per migliorare l'inferenza del tipo di archivio di argomenti e i risultati.
