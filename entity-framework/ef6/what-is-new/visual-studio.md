---
title: Versioni di Visual Studio - Entity Framework 6
author: divega
ms.date: 2018-07-05
ms.assetid: 028FF890-4EDB-4F03-AE53-72F9C33EC92F
ms.openlocfilehash: aa5409506eeea599421608ddb2dd891df0413961
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 08/27/2018
ms.locfileid: "42997385"
---
# <a name="visual-studio-releases"></a>Versioni di Visual Studio

È consigliabile usare sempre la versione più recente di Visual Studio perché contiene gli strumenti più recenti per .NET, NuGet ed Entity Framework.
In effetti, i diversi esempi e procedure dettagliate nella documentazione di Entity Framework presuppongono che si usi una versione recente di Visual Studio.

È comunque possibile usare le versioni precedenti di Visual Studio con diverse versioni di Entity Framework, purché tiene conto alcune differenze:

## <a name="visual-studio-2017-157-and-newer"></a>Visual Studio 2017 15.7 e successive

- Questa versione di Visual Studio include la versione più recente di strumenti di Entity Framework e il runtime di Entity Framework 6.2 e non richiede passaggi di installazione aggiuntivi.
Visualizzare [What ' s New](~/ef6/what-is-new/index.md) per altri dettagli su queste versioni.
- Aggiunta di Entity Framework per nuovi progetti usando gli strumenti di Entity Framework aggiungerà automaticamente il pacchetto EF 6.2 NuGet.
Manualmente, è possibile installare o eseguire l'aggiornamento a un pacchetto NuGet di Entity Framework disponibile online.
- Per impostazione predefinita, l'istanza di SQL Server disponibile in questa versione di Visual Studio è un'istanza di Local DB chiamata MSSQLLocalDB.
La sezione server della stringa di connessione è necessario utilizzare è "(localdb)\\MSSQLLocalDB".
Ricordarsi di usare una stringa verbatim preceduta `@` o barre rovesciate doppie "\\\\" quando si specifica una stringa di connessione nel codice c#.  


## <a name="visual-studio-2015-to-visual-studio-2017-156"></a>Visual Studio 2015 a Visual Studio 2017 versione 15.6

- Queste versioni di Visual Studio includono strumenti di Entity Framework e runtime 6.1.3.
Visualizzare [rilascia ultimi](~/ef6/what-is-new/past-releases.md#ef-613) per altri dettagli su queste versioni.
- Aggiunta di Entity Framework per nuovi progetti usando gli strumenti di Entity Framework aggiungerà automaticamente il EF 6.1.3 pacchetto NuGet.
Manualmente, è possibile installare o eseguire l'aggiornamento a un pacchetto NuGet di Entity Framework disponibile online.
- Per impostazione predefinita, l'istanza di SQL Server disponibile in questa versione di Visual Studio è un'istanza di Local DB chiamata MSSQLLocalDB.
La sezione server della stringa di connessione è necessario utilizzare è "(localdb)\\MSSQLLocalDB".
Ricordarsi di usare una stringa verbatim preceduta `@` o barre rovesciate doppie "\\\\" quando si specifica una stringa di connessione nel codice c#.  


## <a name="visual-studio-2013"></a>Visual Studio 2013
- Questa versione di Visual Studio include e versione precedente del runtime e strumenti di Entity Framework.
È consigliabile eseguire l'aggiornamento a Entity Framework Tools 6.1.3, usando [il programma di installazione](https://www.microsoft.com/en-us/download/details.aspx?id=40762) disponibili nel Microsoft Download Center.
Visualizzare [rilascia ultimi](~/ef6/what-is-new/past-releases.md#ef-613) per altri dettagli su queste versioni.
- Entity Framework per nuovi progetti usando gli strumenti di Entity Framework aggiornati automaticamente l'aggiunta di EF 6.1.3 pacchetto NuGet.
Manualmente, è possibile installare o eseguire l'aggiornamento a un pacchetto NuGet di Entity Framework disponibile online.
- Per impostazione predefinita, l'istanza di SQL Server disponibile in questa versione di Visual Studio è un'istanza di Local DB chiamata MSSQLLocalDB.
La sezione server della stringa di connessione è necessario utilizzare è "(localdb)\\MSSQLLocalDB".
Ricordarsi di usare una stringa verbatim preceduta `@` o barre rovesciate doppie "\\\\" quando si specifica una stringa di connessione nel codice c#.  

## <a name="visual-studio-2012"></a>Visual Studio 2012

- Questa versione di Visual Studio include e versione precedente del runtime e strumenti di Entity Framework.
È consigliabile eseguire l'aggiornamento a Entity Framework Tools 6.1.3, usando [il programma di installazione](https://www.microsoft.com/en-us/download/details.aspx?id=40762) disponibili nel Microsoft Download Center.
Visualizzare [rilascia ultimi](~/ef6/what-is-new/past-releases.md#ef-613) per altri dettagli su queste versioni.
- Entity Framework per nuovi progetti usando gli strumenti di Entity Framework aggiornati automaticamente l'aggiunta di EF 6.1.3 pacchetto NuGet.
Manualmente, è possibile installare o eseguire l'aggiornamento a un pacchetto NuGet di Entity Framework disponibile online.
- Per impostazione predefinita, l'istanza di SQL Server disponibile in questa versione di Visual Studio è un'istanza di Local DB chiamata v11.0.
La sezione server della stringa di connessione è necessario utilizzare è "(localdb)\\v11.0".
Ricordarsi di usare una stringa verbatim preceduta `@` o barre rovesciate doppie "\\\\" quando si specifica una stringa di connessione nel codice c#.  

## <a name="visual-studio-2010"></a>Visual Studio 2010

- La versione di strumenti di Entity Framework disponibile con questa versione di Visual Studio non è compatibile con il runtime di Entity Framework 6 e non può essere aggiornata.
- Per impostazione predefinita, gli strumenti di Entity Framework aggiungerà Entity Framework 4.0 per i progetti.
Per creare applicazioni con le versioni più recenti di Entity Framework, è necessario innanzitutto installare il [estensione Gestione pacchetti NuGet](https://marketplace.visualstudio.com/items?itemName=NuGetTeam.NuGetPackageManager).
- Per impostazione predefinita, tutti la generazione di codice nella versione di strumenti di Entity Framework si basa su EntityObject ed Entity Framework 4.
È consigliabile che il Licenziatario sia basata su DbContext ed Entity Framework 5, installando i modelli di generazione di codice di DbContext per la generazione di codice [c#](https://marketplace.visualstudio.com/items?itemName=EntityFrameworkTeam.EF5xDbContextGeneratorforC) oppure [Visual Basic](https://marketplace.visualstudio.com/items?itemName=EntityFrameworkTeam.EF5xDbContextGeneratorforVBNET).
- Dopo aver installato le estensioni di gestione pacchetti NuGet, è possibile manualmente installare o eseguire l'aggiornamento a un pacchetto NuGet di Entity Framework disponibile online e usare Entity Framework 6 Code First, che non richiede una finestra di progettazione.
- Per impostazione predefinita, l'istanza di SQL Server disponibile in questa versione di Visual Studio è SQL Server Express denominata SQLEXPRESS.
La sezione server della stringa di connessione è necessario utilizzare è ". \\SQLEXPRESS ".
Ricordarsi di usare una stringa verbatim preceduta `@` o barre rovesciate doppie "\\\\" quando si specifica una stringa di connessione nel codice c#.
