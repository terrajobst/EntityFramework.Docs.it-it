---
title: Log delle modifiche che influiscono sul provider - EF Core
author: ajcvickers
ms.author: avickers
ms.date: 08/08/2018
ms.assetid: 7CEF496E-A5B0-4F5F-B68E-529609B23EF9
ms.technology: entity-framework-core
uid: core/providers/provider-log
ms.openlocfilehash: 5da1043310e2858638c81a0654a9cab23e39c220
ms.sourcegitcommit: 0d36e8ff0892b7f034b765b15e041f375f88579a
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/09/2018
ms.locfileid: "44250816"
---
# <a name="provider-impacting-changes"></a>Modifiche che influiscono sul provider

Questa pagina contiene collegamenti per eseguire il pull le richieste effettuate nel repository EF Core che possono richiedere gli autori di altri provider di database per rispondere. Lo scopo consiste nel fornire un punto di partenza per gli autori di provider di database di terze parti esistenti quando si aggiorna il provider a una nuova versione.

Questo log è iniziata con le modifiche da 2.1 a 2.2. Prima di 2.1 è stato usato il [ `providers-beware` ](https://github.com/aspnet/EntityFrameworkCore/labels/providers-beware) e [ `providers-fyi` ](https://github.com/aspnet/EntityFrameworkCore/labels/providers-fyi) etichette nei nostri problemi e richieste pull.

## <a name="21-----22"></a>2.1---> 2.2

### <a name="test-only-changes"></a>Modifiche apportate a solo scopo di test

* https://github.com/aspnet/EntityFrameworkCore/pull/12057 -Consenti personalizzabile delimeters SQL nei test
  * Testare le modifiche che consentono di BuiltInDataTypesTestBase non strict confronti completi dei punti a virgola mobile
  * Modifiche di test che consentono i test di query essere riutilizzata con diversi delimeters SQL
* https://github.com/aspnet/EntityFrameworkCore/pull/12072 -Aggiungere test DbFunction ai test relazionale specifica
  * In modo che questi test possono essere eseguiti su tutti i provider di database
* https://github.com/aspnet/EntityFrameworkCore/pull/12362 -Pulizia del test Async
  * Rimuovere `Wait` chiamate, non necessari async e rinominati alcuni metodi di test
* https://github.com/aspnet/EntityFrameworkCore/pull/12666 -Unificare l'infrastruttura di test di registrazione
  * Aggiunto `CreateListLoggerFactory` e rimossi alcuni precedente infrastruttura di registrazione, in modo da richiederla provider utilizzando questi test per rispondere
* https://github.com/aspnet/EntityFrameworkCore/pull/12500 -Eseguire altri test di query sia in modo sincrono e asincrono
  * I nomi di test e le stesse caratteristiche è stato modificato, che richiederà l'uso nei provider tramite questi test per rispondere
* https://github.com/aspnet/EntityFrameworkCore/pull/12766 -Spostamenti a ridenominazione del modello ComplexNavigations
  * Provider di questi test potrebbe essere necessario rispondere
* https://github.com/aspnet/EntityFrameworkCore/pull/12141 -Restituire il contesto per il pool anziché eliminare in test funzionali
  * Questa modifica include alcuni test di refactoring che può richiedere ai provider di react


### <a name="test-and-product-code-changes"></a>Modifiche al codice di test e prodotto

* https://github.com/aspnet/EntityFrameworkCore/pull/12109 -Consolidare RelationalTypeMapping.Clone metodi
  * Le modifiche in 2.1 per il RelationalTypeMapping consentite per semplificare il lavoro per le classi derivate. Non si ritiene che questo era di rilievo ai provider, ma i provider possono sfruttare i vantaggi di questa modifica nel relativo tipo derivato mapping di classi.
* https://github.com/aspnet/EntityFrameworkCore/pull/12069 -Query denominate o tag
  * Aggiunge l'infrastruttura per l'assegnazione di tag query LINQ e presenza di tali tag visualizzato come commenti in SQL. Ciò può richiedere ai provider di reagire in generazione SQL.
* https://github.com/aspnet/EntityFrameworkCore/pull/13115 -Supportano i dati spaziali tramite NTS
  * Consente di mapping dei tipi e membri perché i traduttori possano essere registrati di fuori del provider
    * I provider devono chiamare base. FindMapping() nella propria implementazione ITypeMappingSource per farli funzionare
  * Seguire questo modello per aggiungere il supporto spaziale per il provider che è coerenza per tutti i provider.
* https://github.com/aspnet/EntityFrameworkCore/pull/13199 -Aggiungere debug avanzato per la creazione di provider del servizio
  * Consente a DbContextOptionsExtensions implementare una nuova interfaccia che può aiutare le persone a comprendere il motivo per cui il provider di servizi interna è in fase di ricompilazione
