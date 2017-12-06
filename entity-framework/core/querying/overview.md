---
title: "La modalità query - Core EF"
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: de2e34cd-659b-4cab-b5ed-7a979c6bf120
ms.technology: entity-framework-core
uid: core/querying/overview
ms.openlocfilehash: 7fd2940d559f82016d7a8fc3fdcf3af0d5b8bc8f
ms.sourcegitcommit: 01a75cd483c1943ddd6f82af971f07abde20912e
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 10/27/2017
---
# <a name="how-queries-work"></a>Il funzionamento delle query

Entity Framework Core Usa integrare Query LINQ (Language) per eseguire query sui dati dal database. LINQ consente di usare c# (o il proprio linguaggio .NET) per creare query fortemente tipizzata in base alle classi di contesto ed entità derivate.

## <a name="the-life-of-a-query"></a>Il ciclo di vita di una query

Di seguito è fornita una panoramica di alto livello del processo di che ogni query passa attraverso.

1. La query LINQ viene elaborata da Entity Framework Core per compilare una rappresentazione che è pronta per essere eseguito dal provider di database
   1. Il risultato viene memorizzato nella cache in modo che tale elaborazione non devono essere eseguite ogni volta che viene eseguita la query
2. Il risultato viene passato al provider di database
   1. Il provider di database identifica le parti della query possono essere valutate in database
   2. Queste parti della query vengono convertite in linguaggio di query specifico di database (ad esempio SQL per un database relazionale)
   3. Una o più query vengono inviate al database e il set di risultati restituito (risultati sono i valori del database, non le istanze di entità)
3. Per ogni elemento nel set di risultati
   1. Se si tratta di una query di rilevamento, EF controlla se i dati rappresentano un'entità già presenti nel rilevamento delle modifiche per l'istanza del contesto
      * In questo caso, viene restituito l'entità esistente
      * In caso contrario, viene creata una nuova entità, viene configurato il rilevamento delle modifiche e viene restituita la nuova entità
   2. Se si tratta di una query di rilevamento di no, EF controlla se i dati rappresentano un'entità già presente nel set di risultati per questa query
      * Se in tal caso, viene restituita l'entità esistente <sup>(1)</sup>
      * In caso contrario, una nuova entità viene creata e restituita

<sup>(1) </sup> Nessuna query di rilevamento utilizza riferimenti deboli per tenere traccia delle entità che sono già state restituite. Se un risultato precedente con la stessa identità abbandona l'ambito e l'esecuzione della garbage collection, è possibile ottenere una nuova istanza di entità.

## <a name="when-queries-are-executed"></a>Quando vengono eseguite query

Quando si chiamano operatori LINQ, si sta compilando semplicemente una rappresentazione in memoria della query. La query viene inviata solo al database quando vengono usati i risultati.

Le operazioni più comuni che la query viene inviato al database sono:
* L'iterazione i risultati in un `for` ciclo
* Utilizzo di un operatore, ad esempio `ToList`, `ToArray`, `Single`,`Count`
* L'associazione dati i risultati di una query a un'interfaccia utente

> [!WARNING]  
> **Convalidare sempre l'input dell'utente:** EF mentre forniscono protezione da attacchi SQL injection, non esegue alcuna convalida dell'input generale. Pertanto se i valori passati alle API, utilizzate nelle query LINQ, assegnata alla proprietà dell'entità e così via, provengono da un'origine non attendibile quindi convalida appropriato, per esigenze dell'applicazione deve essere eseguita. Ciò include qualsiasi input dell'utente utilizzato per costruire in modo dinamico delle query. Anche quando si usa LINQ, se si accetta l'input dell'utente per la compilazione di espressioni, che è necessario assicurarsi che solo le espressioni previsto può essere costruito.
