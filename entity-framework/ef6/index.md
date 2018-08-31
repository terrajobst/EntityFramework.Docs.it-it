---
title: Panoramica - EF6
author: divega
ms.date: 2016-10-23
ms.assetid: 8ae74d63-6bad-4686-b325-bbf9d68f3743
uid: ef6/index
ms.openlocfilehash: 5218f3f8149b15cb5ad3a14779dfa9c23eb71a0e
ms.sourcegitcommit: 0cef7d448e1e47bdb333002e2254ed42d57b45b6
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 08/29/2018
ms.locfileid: "43152494"
---
# <a name="entity-framework-6"></a>Entity Framework 6
Entity Framework 6 (EF6) è un mapper relazionale a oggetti comprovato e testato per .NET le cui funzionalità vengono sviluppate e stabilizzate da anni.

In quanto mapper relazionale a oggetti, EF6 riduce la mancata corrispondenza dell'impedenza tra i mondi relazionale e orientato agli oggetti, consentendo agli sviluppatori di scrivere applicazioni che interagiscono con i dati archiviati in database relazionali mediante oggetti .NET fortemente tipizzati che rappresentano il dominio dell'applicazione ed eliminando la necessità di gran parte del codice di accesso ai dati complesso che di solito devono scrivere.

EF6 implementa diverse funzionalità del mapping relazionale a oggetti molto comuni:
- Mapping delle classi di entità [POCO](~/ef6/resources/glossary.md#poco) che non dipendono da alcun tipo di Entity Framework
- Rilevamento automatico delle modifiche
- Risoluzione di identità e unità di lavoro
- Caricamento eager, lazy ed esplicito
- Traduzione di query fortemente tipizzate con uso di LINQ (Language Integrated Query)
- Funzionalità di mapping complete che includono il supporto per:
  - Relazioni uno-a-uno, uno-a-molti e molti-a-molti
  - Ereditarietà (tabella per gerarchia, tabella per tipo e tabella per classe concreta)
  - Tipi complessi
  - Stored procedure
- Una finestra di progettazione per creare modelli di entità.
- Un'esperienza Code First per creare modelli di entità mediante la scrittura di codice.
- I modelli possono essere generati da database esistenti e poi modificati manualmente, oppure possono essere creati da zero e usati per generare nuovi database.
- Integrazione con i modelli di applicazione .NET Framework, tra cui ASP.NET e, tramite associazione dati, con WPF e WinForms.
- Connettività del database basata su ADO.NET e numerosi provider disponibili per la connessione a SQL Server, Oracle, MySQL, SQLite, PostgreSQL, DB2 e così via.

## <a name="should-i-use-ef6-or-ef-core"></a>È meglio usare EF6 o EF Core?

EF Core è una versione più moderna, leggera ed estendibile di Entity Framework che offre molte funzionalità e vantaggi simili a quelli di EF6.
EF Core è una versione completamente riscritta e contiene molte nuove funzionalità non disponibili in EF6, anche se non include ancora alcune delle funzionalità di mapping più avanzate di EF6.
È consigliabile usare EF Core nelle nuove applicazioni, purché il set di funzionalità soddisfi i propri requisiti.
[Confronto tra EF Core ed EF6](xref:efcore-and-ef6/index) esamina questa scelta in modo più dettagliato.

## <a name="get-startedef6get-startedmd"></a>[Introduzione](~/ef6/get-started.md)

Aggiungere al progetto il pacchetto NuGet EntityFramework o installare Entity Framework Tools per Visual Studio. In seguito guardare i video e leggere le esercitazioni e la documentazione avanzata per sfruttare al meglio EF6.

## <a name="past-entity-framework-versions"></a>Versioni precedenti di Entity Framework

Questa è la documentazione per la versione più recente di Entity Framework 6, anche se la maggior parte di essa si applica anche alle versioni precedenti.
Vedere [Novità](~/ef6/what-is-new/index.md) e [Versioni precedenti](~/ef6/what-is-new/past-releases.md) per un elenco completo delle versioni di Entity Framework e delle funzionalità introdotte.
