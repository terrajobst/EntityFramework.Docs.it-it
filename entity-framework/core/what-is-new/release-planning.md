---
title: Pianificazione delle versioni di EF Core
author: ajcvickers
ms.date: 01/28/2020
uid: core/what-is-new/release_planning.md
ms.openlocfilehash: 71045b8d49c319a73f74443612bedd84ee33ab8a
ms.sourcegitcommit: 9b562663679854c37c05fca13d93e180213fb4aa
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/07/2020
ms.locfileid: "78417336"
---
# <a name="release-planning-process"></a>Processo di pianificazione delle versioni

Viene chiesto spesso in che modo vengano scelte le funzionalità da inserire in una versione specifica.
Questo documento descrive il processo che utilizziamo.
Il processo è in continua evoluzione man mano che troviamo modi migliori per pianificare, ma le idee generali rimangono le stesse.

## <a name="different-kinds-of-releases"></a>Diversi tipi di rilasci

Diversi tipi di rilascio contengono diversi tipi di modifiche.
Questo a sua volta significa che la pianificazione del rilascio è diversa per diversi tipi di rilascio.

### <a name="patch-releases"></a>Rilasci di patch

Le versioni patch modificano solo la parte "patch" della versione.
Ad esempio, EF Core 3.1.For example, EF Core 3.1. **1** è una versione che patch problemi rilevati in EF Core 3.1. **0**.

Le versioni delle patch hanno lo scopo di correggere i bug critici dei clienti.
Ciò significa che le versioni delle patch non includono nuove funzionalità.
Le modifiche alle API non sono consentite nei rilasci di patch, tranne in circostanze particolari.

La barra per fare una modifica in un rilascio di patch è molto alta.
Questo perché è fondamentale che le versioni delle patch non introducano nuovi bug.
Pertanto, il processo decisionale enfatizza l'alto valore e basso rischio.

È più probabile che si metta in patch un problema se:
  * Ha un impatto su più clienti
  * Si tratta di una regressione da una versione precedente
  * L'errore causa il danneggiamento dei dati

È meno probabile che si metta in patch un problema se:
  * Esistono soluzioni ragionevoli
  * La correzione ha un alto rischio di rompere qualcos'altro
  * Il bug è in un angolo-caso

Questa barra aumenta gradualmente per tutta la durata di un rilascio di [supporto a lungo termine (LTS).](https://dotnet.microsoft.com/platform/support/policy/dotnet-core) Ciò è dovuto al fatto che i rilasci di LTS enfatizzano la stabilità.

La decisione finale sulla possibilità o meno di correggere un problema è presa dai Direttori di .NET presso Microsoft.

### <a name="minor-releases"></a>Rilasci secondari

Le versioni secondarie modificano solo la parte "minore" della versione.
Ad esempio, EF Core 3.For example, EF Core 3. **1**.0 è una versione che migliora in EF Core 3. **0**.0.

Rilasci secondari:
* Hanno lo scopo di migliorare la qualità e le caratteristiche della release precedente
* In genere contengono correzioni di bug e nuove funzionalità
* Non includere modifiche intenzionali di rilievo
* Ottieni alcune anteprime non definitive inviate a NuGet

### <a name="major-releases"></a>Rilasci principali

Le versioni principali modificano il numero di versione "principale" di EF.
Ad esempio, EF Core **3**.0.0 è una versione principale che fa un grande passo avanti rispetto a EF Core 2.2.x.

Versioni principali:
* Hanno lo scopo di migliorare la qualità e le caratteristiche della release precedente
* In genere contengono correzioni di bug e nuove funzionalità
  * Alcune delle nuove funzionalità possono essere modifiche fondamentali al funzionamento di EF Core
* In genere includono modifiche intenzionali di rilievo
  * Le modifiche di rottura sono necessarie parte dell'evoluzione del core EF man mano che impariamo
  * Tuttavia, pensiamo molto attentamente a fare qualsiasi cambiamento di rottura a causa del potenziale impatto del cliente. In passato potremmo essere stati troppo aggressivi con dei cambiamenti di rottura. In futuro, ci impegneremo a ridurre al minimo le modifiche che interrompono le applicazioni e a ridurre le modifiche che interrompono i provider di database e le estensioni.
* Avere molte anteprime non definitive inviate a NuGet

## <a name="planning-for-majorminor-releases"></a>Pianificazione dei rilasci principali/secondari

### <a name="github-issue-tracking"></a>Monitoraggio dei problemi di GitHub

GitHub[https://github.com/dotnet/efcore](https://github.com/dotnet/efcore)( ) è l'origine della verità per tutta la pianificazione di EF Core.

Problemi su GitHub hanno:

* Uno stato
  * [I](https://github.com/dotnet/efcore/issues) problemi aperti non sono stati risolti.
  * Sono state risolte le questioni [chiuse.](https://github.com/dotnet/efcore/issues?q=is%3Aissue+is%3Aclosed)
    * Tutti i problemi risolti sono [contrassegnati con closed-fixed](https://github.com/dotnet/efcore/issues?q=is%3Aissue+label%3Aclosed-fixed+is%3Aclosed). Un problema contrassegnato con closed-fixed è stato risolto e unito, ma potrebbe non essere stato rilasciato.
    * Altre `closed-` etichette indicano altri motivi per chiudere un problema. Ad esempio, i duplicati sono contrassegnati con closed-duplicate.
* Un tipo
  * [I bug](https://github.com/dotnet/efcore/issues?q=is%3Aissue+is%3Aopen+label%3Atype-bug) rappresentano bug.
  * [I miglioramenti](https://github.com/dotnet/efcore/issues?q=is%3Aissue+is%3Aopen+label%3Atype-enhancement) rappresentano nuove funzionalità o funzionalità migliori nelle funzionalità esistenti.
* Una pietra miliare
  * Il team sta esaminando [problemi che non hanno alcuna pietra miliare.](https://github.com/dotnet/efcore/issues?q=is%3Aopen+is%3Aissue+no%3Amilestone) La decisione su cosa fare della questione non è ancora stata presa o si sta esaminando una modifica della decisione.
  * [Problemi nell'attività cardine Backlog](https://github.com/dotnet/efcore/issues?q=is%3Aopen+is%3Aissue+milestone%3ABacklog) sono elementi che il team di EF prenderà in considerazione l'utilizzo in una versione futura
    * I problemi nel backlog possono essere [contrassegnati con la versione consider-for-next](https://github.com/dotnet/efcore/issues?q=is%3Aissue+is%3Aopen+label%3Aconsider-for-next-release) che indica che questo elemento di lavoro è elevato nell'elenco per la versione successiva.
  * I problemi aperti in un'attività cardine con controllo delle versioni sono elementi su cui il team prevede di lavorare in tale versione. Ad esempio, [questi sono i problemi su cui si prevede di lavorare per EF Core 5.0](https://github.com/dotnet/efcore/issues?q=is%3Aopen+is%3Aissue+milestone%3A5.0.0).
  * I problemi chiusi in un'attività cardine con controllo delle versioni sono problemi completati per tale versione. Si noti che la versione potrebbe non essere ancora stata rilasciata. Ad esempio, [questi sono i problemi completati per EF Core 3.0](https://github.com/dotnet/efcore/issues?q=is%3Aissue+milestone%3A3.0.0+is%3Aclosed).
* Voti!
  * Votare è il modo migliore per indicare che un problema è importante per te.
  * Per votare, basta aggiungere un "pollice in su" 👍 alla questione. Per esempio, [queste sono le questioni più votate](https://github.com/dotnet/efcore/issues?q=is%3Aissue+is%3Aopen+sort%3Areactions-%2B1-desc)
  * Si prega di commentare anche che descrive i motivi specifici che ti servono la funzione se ritieni che questo aggiunge valore. L'aggiunta di un valore pari a "1" o simile non aggiunge valore.

### <a name="the-planning-process"></a>Processo di pianificazione

Il processo di pianificazione è più complesso che prendere le funzionalità più richieste dal backlog.
Questo perché raccogliamo feedback da più parti interessate in diversi modi.
Viene quindi modellato un rilascio in base a:

* Input da parte dei clienti
* Input da parte di altre parti interessate
* Direzione strategica
* Risorse disponibili
* Pianificazione

Alcune delle domande che ci poniamo sono:

1. **Quanti sviluppatori si pensa che useranno la funzionalità e quanto questa migliorerà le loro applicazioni o la loro esperienza?** Per rispondere a questa domanda vengono raccolti commenti e suggerimenti provenienti da numerose fonti, una delle quali sono i commenti e i voti per i problemi. Impegni specifici con clienti importanti è un altro.

2. **Quali sono le soluzioni alternative utilizzabili finché questa funzionalità non viene implementata?** Molti sviluppatori, ad esempio, possono eseguire il mapping di una tabella di join per ovviare alla mancanza del supporto molti-a-molti nativo. Ovviamente, non tutti gli sviluppatori vogliono farlo, ma molti possono e ciò è un fattore di cui tenere conto per la decisione.

3. **L'implementazione di questa funzionalità consente un'evoluzione dell'architettura di EF Core tale da facilitare l'implementazione di altre funzionalità?** Vengono tendenzialmente favorite le funzionalità che rappresentano la base per altre funzionalità. Ad esempio, le entità contenitore di proprietà possono essere utili per procedere verso l'implementazione del supporto molti-a-molti e i costruttori di entità hanno reso possibile il supporto del caricamento lazy.

4. **La funzionalità rappresenta un punto di estensibilità?** Vengono tendenzialmente favoriti i punti di estendibilità rispetto alle normali funzionalità, perché consentono agli sviluppatori di collegare i propri comportamenti e di compensare eventuali funzionalità mancanti.

5. **Qual è la sinergia della funzionalità quando viene usata in combinazione con altri prodotti?** Vengono favorite le funzionalità che consentono l'uso o che migliorano significativamente l'esperienza d'uso di EF Core con altri prodotti, ad esempio .NET Core, la versione più recente di Visual Studio, Microsoft Azure e così via.

6. **Quali sono le competenze delle persone disponibili per lavorare su una funzionalità e come possiamo sfruttare al meglio queste risorse?** Ogni membro del team EF e i collaboratori della community hanno diversi livelli di esperienza in aree diverse ed è quindi necessario pianificare di conseguenza. Anche se si volesse usufruire della collaborazione di tutti su una funzionalità specifica, ad esempio le traslazioni GroupBy o il supporto molti-a-molti, questo non sarebbe pratico.
