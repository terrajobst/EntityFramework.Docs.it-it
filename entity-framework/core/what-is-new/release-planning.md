---
title: Pianificazione della versione EF Core
author: ajcvickers
ms.date: 01/14/2020
uid: core/what-is-new/release_planning.md
ms.openlocfilehash: c60040aa4acc33ba8b5a54b619539b275690f70e
ms.sourcegitcommit: f2a38c086291699422d8b28a72d9611d1b24ad0d
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/16/2020
ms.locfileid: "76125396"
---
# <a name="release-planning-process"></a>Processo di pianificazione delle versioni

Viene chiesto spesso in che modo vengano scelte le funzionalità da inserire in una versione specifica.
È certo che il backlog non viene convertito automaticamente in piani di rilascio.
La presenza di una funzionalità in EF6, poi, non significa che tale funzionalità verrà necessariamente implementata in EF Core.

È difficile descrivere in dettaglio l'intero processo seguito per pianificare un rilascio.
Gran parte del processo implica la discussione delle funzionalità, opportunità e priorità specifiche e il processo stesso si evolve anche per ogni rilascio.
È tuttavia possibile riepilogare le domande più frequenti a cui si tenta di rispondere quando si deve decidere su cosa lavorare:

1. **Quanti sviluppatori si pensa che useranno la funzionalità e quanto questa migliorerà le loro applicazioni o la loro esperienza?** Per rispondere a questa domanda vengono raccolti commenti e suggerimenti provenienti da numerose fonti, una delle quali sono i commenti e i voti per i problemi.

2. **Quali sono le soluzioni alternative utilizzabili finché questa funzionalità non viene implementata?** Molti sviluppatori, ad esempio, possono eseguire il mapping di una tabella di join per ovviare alla mancanza del supporto molti-a-molti nativo. Ovviamente, non tutti gli sviluppatori vogliono farlo, ma molti possono e ciò è un fattore di cui tenere conto per la decisione.

3. **L'implementazione di questa funzionalità consente un'evoluzione dell'architettura di EF Core tale da facilitare l'implementazione di altre funzionalità?** Vengono tendenzialmente favorite le funzionalità che rappresentano la base per altre funzionalità. Ad esempio, le entità contenitore di proprietà possono essere utili per procedere verso l'implementazione del supporto molti-a-molti e i costruttori di entità hanno reso possibile il supporto del caricamento lazy.

4. **La funzionalità rappresenta un punto di estensibilità?** Vengono tendenzialmente favoriti i punti di estendibilità rispetto alle normali funzionalità, perché consentono agli sviluppatori di collegare i propri comportamenti e di compensare eventuali funzionalità mancanti.

5. **Qual è la sinergia della funzionalità quando viene usata in combinazione con altri prodotti?** Vengono favorite le funzionalità che consentono l'uso o che migliorano significativamente l'esperienza d'uso di EF Core con altri prodotti, ad esempio .NET Core, la versione più recente di Visual Studio, Microsoft Azure e così via.

6. **Quali sono le competenze delle persone disponibili a lavorare su una funzionalità e come è possibile sfruttare al meglio queste risorse?** Ogni membro del team EF e i collaboratori della community hanno diversi livelli di esperienza in aree diverse ed è quindi necessario pianificare di conseguenza. Anche se si volesse usufruire della collaborazione di tutti su una funzionalità specifica, ad esempio le traslazioni GroupBy o il supporto molti-a-molti, questo non sarebbe pratico.

Come accennato in precedenza, il processo si evolve a ogni rilascio.
In futuro si tenterà di aggiungere maggiori opportunità per i membri della community per fornire input per i piani di rilascio.
Ad esempio, Microsoft vorrebbe renderne più semplice la revisione delle bozze di progettazione delle funzionalità e del piano di rilascio stesso.

## <a name="backlog"></a>Backlog

Il [Backlog Milestone](https://github.com/aspnet/EntityFrameworkCore/issues?q=is%3Aopen+is%3Aissue+milestone%3ABacklog+sort%3Areactions-%2B1-desc) (attività cardine di backlog) nella pagina di registrazione dei problemi contiene i problemi su cui Microsoft prevede di lavorare prima o poi oppure che potrebbero essere affrontati da qualcuno nella community.
I clienti sono invitati a inviare commenti e voti per questi problemi.
I collaboratori che hanno intenzione di lavorare a questi problemi sono invitati ad avviare prima di tutto una discussione in merito a come approcciarli.

Non esiste mai la garanzia che Microsoft lavori a una determinata funzionalità in una versione specifica di EF Core.
Come in tutti i progetti software, le priorità, le pianificazioni dei rilasci e le risorse disponibili possono cambiare in qualsiasi momento.
Tuttavia, se si intende risolvere un problema in un intervallo di tempo specifico, questo verrà assegnato a un'attività cardine di rilascio anziché all'attività cardine del backlog.

È probabile che un problema venga chiuso se è previsto che non venga mai affrontato.
Ma è possibile che un problema precedentemente chiuso venga ripreso in considerazione se si ricevono nuove informazioni al riguardo.
