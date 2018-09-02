---
title: Funzionamento delle query - EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: de2e34cd-659b-4cab-b5ed-7a979c6bf120
uid: core/querying/overview
ms.openlocfilehash: f1c23471bfbc998b2d4f9dc579d1404d6202e109
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 08/27/2018
ms.locfileid: "42993203"
---
# <a name="how-queries-work"></a>Funzionamento delle query

Entity Framework Core usa query LINQ (Language Integrated Query) per eseguire query sui dati dal database. Le query LINQ consentono di usare C#, o il linguaggio .NET che si preferisce, per generare query fortemente tipizzate in base alle classi di contesto ed entità derivate.

## <a name="the-life-of-a-query"></a>Ciclo di vita di una query

Di seguito è disponibile una panoramica generale del processo di elaborazione di ogni query.

1. La query LINQ viene elaborata da Entity Framework Core per creare una rappresentazione pronta per essere elaborata dal provider di database
   1. Il risultato viene memorizzato nella cache in modo che tale elaborazione non debba essere ripetuta per ogni esecuzione della query
2. Il risultato viene passato al provider di database
   1. Il provider di database identifica le parti della query che possono essere valutate nel database
   2. Queste parti della query vengono convertite nel linguaggio di query specifico del database, ad esempio SQL per un database relazionale
   3. Una o più query vengono inviate al database e il set di risultati viene restituito (i risultati sono valori del database e non istanze di entità)
3. Per ogni elemento nel set di risultati
   1. Se si tratta di una query con rilevamento delle modifiche, EF controlla se i dati rappresentano un'entità già presente nello strumento di rilevamento delle modifiche per l'istanza di contesto
      * In caso affermativo, viene restituita l'entità esistente
      * In caso contrario, viene creata una nuova entità, viene configurato il rilevamento delle modifiche e viene restituita la nuova entità
   2. Se si tratta di una query senza rilevamento delle modifiche, EF controlla se i dati rappresentano un'entità già presente nel set di risultati per questa query
      * In caso affermativo, viene restituita l'entità esistente <sup>(1)</sup>
      * In caso contrario, viene creata e restituita una nuova entità

<sup>(1) </sup> Nessuna query con rilevamento delle modifiche usa riferimenti deboli per tenere traccia delle entità già restituite. Se un risultato precedente con la stessa identità esce dall'ambito e viene eseguita la Garbage Collection, si potrebbe ottenere una nuova istanza di entità.

## <a name="when-queries-are-executed"></a>Quando vengono eseguite le query

Quando si chiamano operatori LINQ, si crea semplicemente una rappresentazione in memoria della query. La query viene inviata al database solo al momento dell'utilizzo dei risultati.

Le operazioni più comuni che causano l'invio della query al database sono:
* Iterazione dei risultati in un ciclo `for`
* Uso di un operatore, ad esempio `ToList`, `ToArray`, `Single`, `Count`
* Data binding dei risultati di una query a un'interfaccia utente

> [!WARNING]  
> **Convalidare sempre l'input dell'utente:** anche se EF offre protezione da attacchi SQL injection, non esegue alcuna convalida generale dell'input. Pertanto se i valori passati alle API, usati nelle query LINQ, assegnati alle proprietà di entità e così via, provengono da un'origine non attendibile, è opportuno prevedere una convalida appropriata in base ai requisiti dell'applicazione. Ciò include qualsiasi input dell'utente usato per costruire query in modo dinamico. Anche quando si usa LINQ, se si accetta l'input dell'utente per la creazione delle espressioni, è necessario assicurarsi che possano essere costruite solo le espressioni previste.
