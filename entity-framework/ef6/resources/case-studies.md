---
title: Case study per Entity Framework - Entity Framework 6
author: divega
ms.date: 10/23/2016
ms.assetid: cd5d3ae3-717d-4095-a2ef-0e8fd72b1a2f
ms.openlocfilehash: d7982a3f89ac1e57b48039d828f287adf6dc5068
ms.sourcegitcommit: 2b787009fd5be5627f1189ee396e708cd130e07b
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/13/2018
ms.locfileid: "45490883"
---
# <a name="microsoft-case-studies-for-entity-framework"></a>Case study Microsoft per Entity Framework
I case study in questa pagina evidenzia alcuni progetti di produzione reali che hanno usato Entity Framework.
> [!NOTE]
> Le versioni di questi case study dettagliate non sono più disponibili nel sito Web Microsoft. Di conseguenza i collegamenti sono stati rimossi.

## <a name="epicor"></a>Epicor
Epicor è una società di software globale di grandi dimensioni (con oltre 400 sviluppatori) che sviluppa soluzioni di risorsa pianificazione ERP (Enterprise) per le aziende in più di 150 paesi.
Prodotto principale, Epicor 9, si basa su un Service-Oriented Architecture (SOA) utilizzando .NET Framework.
Affrontare numerose richieste dai clienti per fornire supporto per Language Integrated Query (LINQ) e si vuole anche ridurre il carico sui server SQL back-end, il team ha deciso di eseguire l'aggiornamento a Visual Studio 2010 e .NET Framework 4.0.
Usa Entity Framework 4.0, erano in grado di raggiungere questi obiettivi e semplificano anche notevolmente lo sviluppo e manutenzione.
In particolare, il supporto di T4 avanzato di Entity Framework consentita assumere il pieno controllo del codice generato e compilare automaticamente nelle funzionalità di riduzione delle prestazioni, ad esempio le query precompilate e di memorizzazione nella cache.

> "Abbiamo eseguito alcune prove delle prestazioni recentemente con il codice esistente e siamo riusciti a ridurre le richieste a SQL Server 90%.
Ciò è dovuto ADO.NET Entity Framework 4." – Ricerche su prodotti Erik Johnson, Vice President,  

## <a name="veracity-solutions"></a>Soluzioni di veridicità
Acquisire un sistema software di pianificazione degli eventi che avrebbe dovuto essere difficili da gestire ed estendere a lungo, le soluzioni di veridicità usato Visual Studio 2010 nello scrivere nuovamente come un potente e facile da usare Rich Internet Application basato su Silverlight 4.
Utilizzo dei servizi RIA .NET, è stato possibile creare rapidamente un livello di servizio in Entity Framework che evitare la duplicazione del codice e consentito per la logica di autenticazione e la convalida comune tra i livelli.  

> "Eravamo venduta su Entity Framework quando è stata introdotta ed Entity Framework 4 ha dimostrato di essere ancora migliore.
Gli strumenti sono stato migliorato ed è più semplice modificare i file con estensione edmx che definiscono il modello concettuale, un modello di archiviazione e un mapping tra i modelli... Con Entity Framework, posso ottenere tale livello di accesso ai dati funziona in un giorno —. sln e compilare out man mano che Procedo.
Entity Framework è il livello di accesso ai dati standard de facto; Non so perché tutti gli utenti non usarlo." – Joe McBride, Senior Developer

## <a name="nec-display-solutions-of-america"></a>Visualizzazione NEC soluzioni d'America
NEC volevano immettere sul mercato per la pubblicità digitale in base al luogo con una soluzione per trarre vantaggio inserzionisti e i proprietari di rete e aumentare il propria ricavi.
Per farlo, viene avviato un paio di applicazioni web che consentono di automatizzare i processi manuali necessari in una campagna di annuncio tradizionale.
I siti sono stati creati utilizzando ASP.NET, Silverlight 3, AJAX e WCF, insieme a Entity Framework nel livello di accesso ai dati per comunicare con SQL Server 2008.

> "Con SQL Server, è stato ritenuto è stato possibile ottenere la velocità effettiva è necessaria per servire inserzionisti e le reti con le informazioni in tempo reale e l'affidabilità per garantire che le informazioni nelle nostre applicazioni mission-critical sarà sempre disponibile"-Mike Corcoran, Director of IT

## <a name="darwin-dimensions"></a>Dimensioni di Darwin
Usando una vasta gamma di tecnologie Microsoft, il team di Darwin deciso di creare Evolver - portale un avatar online che i consumer può utilizzare per creare gli avatar accattivanti e realistici per l'uso nei giochi, animazioni e le pagine rete basati su social network.
Con i vantaggi di produttività di Entity Framework e il pull nei componenti, ad esempio Windows Workflow Foundation (WF) e Windows Server AppFabric (una cache a scalabilità elevata dell'applicazione in memoria), il team è stato in grado di offrire un prodotto incredibile in 35% fase di sviluppo.
Anche se i membri del team suddividono in più paesi, il team che segue un processo di sviluppo agile con versioni settimanali.

 > "Si tenta di non creare tecnologia per la scelta della tecnologia. Come un tipo di avvio, è essenziale che si sfrutta la tecnologia che consente di risparmiare tempo e denaro.
 .NET è la scelta per lo sviluppo rapido e conveniente." – Zachary Olsen, architetto  

## <a name="silverware"></a>Posate
Con oltre 15 anni di esperienza nello sviluppo di soluzioni per i gruppi di piccole e medie dimensioni ristorante (POS), il team di sviluppo in posate deciso di migliorare il prodotto con altre funzionalità di livello enterprise per attrarre più grande catene di ristoranti.
Usa la versione più recente di Microsoft strumenti di sviluppo, è stato possibile creare la nuova soluzione quattro volte più rapidamente che mai.
Nuove funzionalità principali come LINQ ed Entity Framework ancora più facile spostare da Crystal Reports a SQL Server 2008 e SQL Server Reporting Services (SSRS) per la loro archiviazione dati e creazione di report alle esigenze.

> "Gestione efficace dei dati è fondamentale per il successo di posate – e per questo motivo abbiamo deciso di adottare SQL Reporting." -Nicholas Romanidis, Director of IT / Software Engineering
