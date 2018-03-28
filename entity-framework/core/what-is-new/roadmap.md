---
title: Roadmap per Entity Framework Core
author: divega
ms.author: divega
ms.date: 02/20/2018
ms.assetid: 834C9729-7F6E-4355-917D-DE3EE9FE149E
ms.technology: entity-framework-core
uid: core/what-is-new/roadmap
ms.openlocfilehash: 5aef679df2ecdfe7f59458c8994d0d17b4a889ff
ms.sourcegitcommit: 2ef0a4a90b01edd22b9206f8729b8de459ef8cab
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 03/20/2018
---
# <a name="entity-framework-core-roadmap"></a>Roadmap per Entity Framework Core

> [!IMPORTANT]
> Si tenga presente che i set di funzionalità e le pianificazioni delle versioni future sono sempre soggette a modifiche e che questa pagina, nonostante l'impegno profuso per mantenerla aggiornata, potrebbe non riflettere sempre i piani più recenti.

La prima anteprima di EF Core 2.1 è stata rilasciata nel mese di febbraio 2018. È ora possibile trovare altre informazioni su questa versione in [Nuove funzionalità di EF Core 2.1](xref:core/what-is-new/ef-core-2.1).

Si prevede di rilasciare altre anteprime di EF Core 2.1 con frequenza mensile e una versione finale nel secondo trimestre di calendario del 2018.

La [pianificazione del rilascio](#release-planning-process) per la versione successiva alla 2.1 non è stata ancora completata.

## <a name="schedule"></a>Pianificazione

La pianificazione per EF Core è sincronizzata con la [pianificazione di .NET Core](https://github.com/dotnet/core/blob/master/roadmap.md) e con quella di [ASP.NET Core](https://github.com/aspnet/Home/wiki/Roadmap).

## <a name="backlog"></a>Backlog

Per gestire l'elenco dettagliato dei problemi e delle funzionalità in una pagina di registrazione dei problemi, viene usato lo strumento [Backlog Milestone](https://github.com/aspnet/EntityFrameworkCore/issues?q=is%3Aopen+is%3Aissue+milestone%3ABacklog+sort%3Areactions-%2B1-desc). I clienti possono commentare e votare le voci di questo elenco.

La tendenza è di lasciare aperti i problemi di cui si prevede di occuparsi in tempi ragionevoli o che possono essere presi in carico da un membro della community. Ciò tuttavia implica l'impegno a risolvere i problemi entro una data precisa solo quando questi vengono assegnati a un'attività cardine specifica nell'ambito del [processo di pianificazione delle versioni](#release-planning-process).

Se non si prevede di implementare una funzionalità, è probabile che il problema corrispondente nell'elenco venga chiuso. Un problema chiuso può essere ripreso in considerazione in seguito se si ottengono nuove informazioni su di esso.

Ciò premesso, al momento non sono disponibili informazioni sufficienti per affermare che una funzionalità specifica verrà risolta entro una data o una versione precisa. Come in tutti i progetti software, priorità, pianificazioni dei rilasci e risorse disponibili possono cambiare in qualsiasi momento.

## <a name="release-planning-process"></a>Processo di pianificazione delle versioni

Viene chiesto spesso in che modo vengano scelte le funzionalità da inserire in una versione specifica. È certo che il backlog non viene convertito automaticamente in piani di rilascio. La presenza di una funzionalità in EF6, poi, non significa che tale funzionalità verrà necessariamente implementata in EF Core.

In questa sede è difficile descrivere in dettaglio l'intero processo di pianificazione di un rilascio, sia perché gran parte di questo è costituito dalla discussione relativa a funzionalità specifiche, alle opportunità che offrono e alla loro priorità, sia perché il processo stesso di solito si evolve a ogni rilascio. È tuttavia relativamente facile riepilogare le domande più frequenti a cui si tenta di rispondere quando si deve decidere su cosa lavorare:

1. **Quanti sviluppatori si pensa che useranno la funzionalità e quanto questa migliorerà le loro applicazioni o la loro esperienza?** Per rispondere a questa domanda viene aggregato feedback proveniente da numerose fonti, una delle quali sono i commenti e i voti ai problemi.

2. **Quali sono le soluzioni alternative utilizzabili finché questa funzionalità non viene implementata?** Molti sviluppatori, ad esempio, sono in grado di eseguire il mapping di una tabella di join per ovviare alla mancanza di supporto molti-a-molti nativo. Non tutti gli sviluppatori sono in grado di farlo, naturalmente, ma molti lo sono e questo è un fattore che conta.

3. **L'implementazione di questa funzionalità consente un'evoluzione dell'architettura di EF Core tale da facilitare l'implementazione di altre funzionalità?** La tendenza è di favorire le funzionalità che possono fungere da componenti di base per altre funzionalità. La suddivisione di tabelle implementata per i tipi i proprietà, ad esempio, consente di avvicinarsi all'implementazione del supporto delle tabelle per tipo.

4. **La funzionalità rappresenta un punto di estensibilità?** La tendenza è di favorire i punti di estendibilità, poiché questi consentono agli sviluppatori di collegare i propri comportamenti in modo più semplice, ottenendo in questo modo alcune delle funzionalità mancanti. Si prevede di implementare questo in parte all'inizio del lavoro sul caricamento lazy.

5. **Qual è la sinergia della funzionalità quando viene usata in combinazione con altri prodotti?** La tendenza è di favorire le funzionalità che consentono l'uso di EF Core con altri prodotti o che migliorano significativamente l'esperienza d'uso di altri prodotti, ad esempio .NET Core, la versione più recente di Visual Studio, Microsoft Azure e così via.

6. **Quali sono le capacità delle persone disponibili a lavorare su una funzionalità e come è possibile sfruttare al meglio queste risorse?** Ogni membro del team EF e anche i collaboratori della community hanno diversi livelli di esperienza in aree diverse ed è quindi necessario pianificare di conseguenza. Anche se si volesse usufruire della collaborazione di tutti su una funzionalità specifica, ad esempio le traslazioni GroupBy o il supporto molti-a-molti, questo non sarebbe pratico.

Come già accennato, questo processo si evolve a ogni rilascio. L'intenzione per il futuro è di aggiungere maggiori opportunità per i membri della community di sviluppatori di offrire il proprio input per i piani di rilascio, ad esempio semplificando l'esame delle bozze proposte delle funzionalità e del piano di rilascio stesso.
