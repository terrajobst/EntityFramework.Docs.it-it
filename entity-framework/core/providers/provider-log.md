---
title: Log delle modifiche che influiscono sul provider - EF Core
author: ajcvickers
ms.author: avickers
ms.date: 08/08/2018
ms.assetid: 7CEF496E-A5B0-4F5F-B68E-529609B23EF9
ms.technology: entity-framework-core
uid: core/providers/provider-log
ms.openlocfilehash: ee73940e3c0030b76e73438b1852cc29ebeadb45
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 08/27/2018
ms.locfileid: "42998350"
---
# <a name="provider-impacting-changes"></a>Modifiche che influiscono sul provider

Questa pagina contiene collegamenti per eseguire il pull le richieste effettuate nel repository EF Core che possono richiedere gli autori di altri provider di database per rispondere. Lo scopo consiste nel fornire un punto di partenza per gli autori di provider di database di terze parti esistenti quando si aggiorna il provider a una nuova versione.

Questo log è iniziata con le modifiche da 2.1 a 2.2. Prima di 2.1 è stato usato il [ `providers-beware` ](https://github.com/aspnet/EntityFrameworkCore/labels/providers-beware) e [ `providers-fyi` ](https://github.com/aspnet/EntityFrameworkCore/labels/providers-fyi) etichette nei nostri problemi e richieste pull.

### <a name="21-----22"></a>2.1---> 2.2

#### <a name="test-only-changes"></a>Modifiche apportate a solo scopo di test

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


#### <a name="test-and-product-code-changes"></a>Modifiche al codice di test e prodotto

* https://github.com/aspnet/EntityFrameworkCore/pull/12109 -Consolidare RelationalTypeMapping.Clone metodi
  * Le modifiche in 2.1 per il RelationalTypeMapping consentite per semplificare il lavoro per le classi derivate. Non si ritiene che questo era di rilievo ai provider, ma i provider possono sfruttare i vantaggi di questa modifica nel relativo tipo derivato mapping di classi.
* https://github.com/aspnet/EntityFrameworkCore/pull/12069 -Query denominate o tag
  * Aggiunge l'infrastruttura per l'assegnazione di tag query LINQ e presenza di tali tag visualizzato come commenti in SQL. Ciò può richiedere ai provider di reagire in generazione SQL.
