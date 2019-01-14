---
title: Roadmap per Entity Framework Core
author: divega
ms.date: 02/20/2018
ms.assetid: 834C9729-7F6E-4355-917D-DE3EE9FE149E
uid: core/what-is-new/roadmap
ms.openlocfilehash: 7eba9e1a8e145ef407f844ff3a3ab3069495b7ae
ms.sourcegitcommit: e66745c9f91258b2cacf5ff263141be3cba4b09e
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 01/06/2019
ms.locfileid: "54058734"
---
# <a name="entity-framework-core-roadmap"></a>Roadmap per Entity Framework Core

> [!IMPORTANT]
> Si tenga presente che i set di funzionalità e le pianificazioni delle versioni future sono sempre soggette a modifiche e che questa pagina, nonostante l'impegno profuso per mantenerla aggiornata, potrebbe non riflettere sempre i piani più recenti.

### <a name="ef-core-30"></a>EF Core 3.0

Dopo il rilascio di EF Core 2.2, l'obiettivo principale è ora EF Core 3.0, che verrà allineato a .NET Core 3.0 e ASP.NET 3.0.

Non sono ancora state completate nuove funzionalità, quindi i  [pacchetti di EF Core 3.0 Preview 1 pubblicati in NuGet Gallery](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore/3.0.0-preview.18572.1)  a dicembre 2018 contengono solo [correzioni di bug, miglioramenti secondari e le modifiche apportate in preparazione del lavoro per la versione 3.0](https://github.com/aspnet/EntityFrameworkCore/issues?q=is%3Aissue+milestone%3A3.0.0+is%3Aclosed+label%3Aclosed-fixed).

In effetti, è ancora necessario mettere a punto la [pianificazione del rilascio](#release-planning-process) per la versione 3.0, per assicurarsi di includere il set di funzionalità appropriato completabile nel tempo previsto.
Altre informazioni verranno condivise non appena la situazione è più chiara, ma ecco alcuni dei temi e delle funzionalità generali previsti:

- **Miglioramenti di LINQ ([#12795](https://github.com/aspnet/EntityFrameworkCore/issues/12795))**: LINQ consente di scrivere query di database senza uscire dal linguaggio preferito, sfruttando i vantaggi delle informazioni complete sui tipi per ottenere IntelliSense e il controllo dei tipi in fase di compilazione.
  LINQ consente però di scrivere anche un numero illimitato di query complesse e ciò ha sempre rappresentato una grande sfida per i provider LINQ.
  Nelle prime versioni di EF Core, questa complicazione è stata risolta in parte, cercando di individuare quali parti di una query possono essere convertite in SQL e consentendo quindi l'esecuzione del resto della query in memoria nel client.
  L'esecuzione sul lato client può essere appropriata in alcuni casi, ma in molti altri casi può causare query inefficienti che potrebbero non essere identificate fino a quando un'applicazione viene distribuita nell'ambiente di produzione.
  In EF Core 3.0 sono previste modifiche sostanziali del funzionamento dell'implementazione di LINQ e delle procedure per testarla.
  Gli obiettivi sono di ottenere una maggiore solidità (ad esempio, per evitare di compromettere il funzionamento delle query con il rilascio di patch), nonché di riuscire a convertire più espressioni correttamente in SQL, per generare query efficienti in un maggior numero di casi e per evitare che query inefficienti non vengano rilevate.

- **Supporto di Cosmos DB ([#8443](https://github.com/aspnet/EntityFrameworkCore/issues/8443))**: è in corso lo sviluppo di un provider Cosmos DB per EF Core, per consentire agli sviluppatori che hanno familiarità con il modello di programmazione di EF di selezionare facilmente Azure Cosmos DB come destinazione per il database dell'applicazione.
  L'obiettivo è quello di rendere alcuni dei vantaggi di Cosmos DB, ad esempio la distribuzione globale, la disponibilità "AlwaysOn", la scalabilità elastica e la bassa latenza, ancora più accessibili per gli sviluppatori .NET.
  Il provider abiliterà la maggior parte delle funzionalità di EF Core, come il rilevamento delle modifiche automatico, LINQ e le conversioni dei valori, con l'API SQL in Cosmos DB. Questo progetto è stato avviato prima di EF Core 2.2 e [sono state pubblicate alcune versioni di anteprima del provider](https://blogs.msdn.microsoft.com/dotnet/2018/10/17/announcing-entity-framework-core-2-2-preview-3/).
  Il nuovo piano prevede di continuare a sviluppare il provider insieme a EF Core 3.0.   

- **Supporto di C# 8.0 ([#12047](https://github.com/aspnet/EntityFrameworkCore/issues/12047))**: lo scopo è consentire ai clienti Microsoft di sfruttare alcune delle [nuove funzionalità previste per C# 8.0](https://blogs.msdn.microsoft.com/dotnet/2018/11/12/building-c-8-0/), ad esempio i flussi asincroni (tra cui await for each) e i tipi riferimento nullable durante l'uso di EF Core.

- **Decompilazione delle viste di database in tipi query ([#1679](https://github.com/aspnet/EntityFrameworkCore/issues/1679))**: in EF Core 2.1 è stato aggiunto il supporto per i tipi query, che possono rappresentare dati leggibili dal database, ma non aggiornabili.
  I tipi query sono particolarmente utili per il mapping delle viste di database, quindi per EF Core 3.0 è prevista l'automatizzazione della creazione dei tipi query per le viste di database.

- **Entità elenco proprietà ([#13610](https://github.com/aspnet/EntityFrameworkCore/issues/13610) e [#9914](https://github.com/aspnet/EntityFrameworkCore/issues/9914))**: questa funzionalità riguarda l'abilitazione di entità che archiviano dati in proprietà indicizzate anziché in proprietà regolari, nonché la possibilità di usare istanze della stessa classe .NET (in modo potenzialmente semplice come `Dictionary<string, object>`) per rappresentare tipi di entità diversi nello stesso modello di EF Core.
  Questa funzionalità è un trampolino di lancio per il supporto delle relazioni molti-a-molti senza un'entità di join, ovvero uno dei miglioramenti più richiesti per EF Core.

- **EF 6.3 in .NET Core ([EF6 #271](https://github.com/aspnet/EntityFramework6/issues/271))**: siamo consapevoli che molte applicazioni esistenti usano versioni precedenti di EF e che la loro conversione per EF Core al solo scopo di sfruttare i vantaggi di .NET Core può talvolta richiedere notevoli sforzi.
  Per questo motivo, la prossima versione di EF 6 verrà adattata per supportare l'esecuzione in .NET Core 3.0.
  Lo scopo è quello di facilitare la conversione delle applicazioni esistenti con modifiche minime.
  Esisteranno alcune limitazioni (ad esempio, saranno necessari nuovi provider e il supporto spaziale con SQL Server non verrà abilitato) e non sono previste nuove funzionalità per EF 6.

Nel frattempo, è possibile usare [questa query nella pagina di registrazione dei problemi](https://github.com/aspnet/EntityFrameworkCore/issues?q=is%3Aopen+is%3Aissue+milestone%3A3.0.0+sort%3Areactions-%2B1-desc) per visualizzare gli elementi di lavoro provvisoriamente assegnati alla versione 3.0.

## <a name="schedule"></a>Pianificazione

La pianificazione per EF Core è sincronizzata con la [pianificazione di .NET Core](https://github.com/dotnet/core/blob/master/roadmap.md) e con quella di [ASP.NET Core](https://github.com/aspnet/Home/wiki/Roadmap).

## <a name="backlog"></a>Backlog

Il [Backlog Milestone](https://github.com/aspnet/EntityFrameworkCore/issues?q=is%3Aopen+is%3Aissue+milestone%3ABacklog+sort%3Areactions-%2B1-desc) (attività cardine di backlog) nella pagina di registrazione dei problemi contiene i problemi su cui Microsoft prevede di lavorare prima o poi oppure che potrebbero essere affrontati da qualcuno nella community.
I clienti sono invitati a inviare commenti e voti per questi problemi.
I collaboratori che hanno intenzione di lavorare a questi problemi sono invitati ad avviare prima di tutto una discussione in merito a come approcciarli.

Non esiste mai la garanzia che Microsoft lavori a una determinata funzionalità in una versione specifica di EF Core.
Come in tutti i progetti software, le priorità, le pianificazioni dei rilasci e le risorse disponibili possono cambiare in qualsiasi momento.
Se Microsoft intende risolvere un problema in un intervallo di tempo specifico, tuttavia, questo verrà assegnato a un'attività cardine di rilascio invece che all'attività cardine di backlog.
I problemi vengono regolarmente spostati tra le attività cardine di backlog e di rilascio nell'ambito del [processo di pianificazione delle versioni](#release-planning-process).

È probabile che un problema venga chiuso se è previsto che non venga mai affrontato.
Ma è possibile che un problema precedentemente chiuso venga ripreso in considerazione se si ricevono nuove informazioni al riguardo.

## <a name="release-planning-process"></a>Processo di pianificazione delle versioni

Viene chiesto spesso in che modo vengano scelte le funzionalità da inserire in una versione specifica.
È certo che il backlog non viene convertito automaticamente in piani di rilascio.
La presenza di una funzionalità in EF6, poi, non significa che tale funzionalità verrà necessariamente implementata in EF Core.

È difficile descrivere in dettaglio l'intero processo seguito per pianificare un rilascio.
Gran parte del processo implica la discussione delle funzionalità, opportunità e priorità specifiche e il processo stesso si evolve anche per ogni rilascio.
È tuttavia possibile riepilogare le domande più frequenti a cui si tenta di rispondere quando si deve decidere su cosa lavorare:

1. **Quanti sviluppatori si pensa che useranno la funzionalità e quanto questa migliorerà le loro applicazioni o la loro esperienza?** Per rispondere a questa domanda vengono raccolti i commenti e suggerimenti provenienti da numerose fonti, una delle quali sono i commenti e i voti per i problemi.

2. **Quali sono le soluzioni alternative utilizzabili finché questa funzionalità non viene implementata?** Molti sviluppatori, ad esempio, possono eseguire il mapping di una tabella di join per ovviare alla mancanza del supporto molti-a-molti nativo. Ovviamente, non tutti gli sviluppatori vogliono farlo, ma molti possono e ciò è un fattore di cui tenere conto per la decisione.

3. **L'implementazione di questa funzionalità consente un'evoluzione dell'architettura di EF Core tale da facilitare l'implementazione di altre funzionalità?** Vengono tendenzialmente favorite le funzionalità che rappresentano la base per altre funzionalità. Ad esempio, le entità contenitore di proprietà possono essere utili per procedere verso l'implementazione del supporto molti-a-molti e i costruttori di entità hanno reso possibile il supporto del caricamento lazy. 

4. **La funzionalità rappresenta un punto di estensibilità?** Vengono tendenzialmente favoriti i punti di estendibilità rispetto alle normali funzionalità, perché consentono agli sviluppatori di collegare i propri comportamenti e di compensare eventuali funzionalità mancanti. 

5. **Qual è la sinergia della funzionalità quando viene usata in combinazione con altri prodotti?** Vengono favorite le funzionalità che consentono l'uso o che migliorano significativamente l'esperienza d'uso di EF Core con altri prodotti, ad esempio .NET Core, la versione più recente di Visual Studio, Microsoft Azure e così via.

6. **Quali sono le competenze delle persone disponibili a lavorare su una funzionalità e come è possibile sfruttare al meglio queste risorse?** Ogni membro del team EF e i collaboratori della community hanno diversi livelli di esperienza in aree diverse ed è quindi necessario pianificare di conseguenza. Anche se si volesse usufruire della collaborazione di tutti su una funzionalità specifica, ad esempio le traslazioni GroupBy o il supporto molti-a-molti, questo non sarebbe pratico.

Come accennato in precedenza, il processo si evolve a ogni rilascio.
In futuro si tenterà di aggiungere maggiori opportunità per i membri della community per fornire input per i piani di rilascio.
Ad esempio, Microsoft vorrebbe renderne più semplice la revisione delle bozze di progettazione delle funzionalità e del piano di rilascio stesso.
