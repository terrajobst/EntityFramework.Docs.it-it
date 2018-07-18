---
title: Miglioramento delle prestazioni di avvio con NGen - Entity Framework 6
author: divega
ms.date: 2016-10-23
ms.prod: entity-framework
ms.author: divega
ms.manager: avickers
ms.technology: entity-framework-6
ms.topic: article
ms.assetid: dc6110a0-80a0-4370-8190-cea942841cee
caps.latest.revision: 4
ms.openlocfilehash: cffd2deea3148a16ed704d1e5e7b365eda06f72b
ms.sourcegitcommit: bdd06c9a591ba5e6d6a3ec046c80de98f598f3f3
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/10/2018
ms.locfileid: "39122740"
---
# <a name="improving-startup-performance-with-ngen"></a>Miglioramento delle prestazioni di avvio con NGen
> [!NOTE]
> **Solo EF6 e versioni successive**: funzionalità, API e altri argomenti discussi in questa pagina sono stati introdotti in Entity Framework 6. Se si usa una versione precedente, le informazioni qui riportate, o parte di esse, non sono applicabili.  

.NET Framework supporta la generazione di immagini native per le applicazioni gestite e librerie come un modo per aiutarti a iniziare più rapidamente le applicazioni e anche in alcuni casi usano meno memoria. Le immagini native vengono create alla traduzione di tali assembly di codice gestito nel file che contengono istruzioni macchina native prima che venga eseguita l'applicazione, il compilatore .NET JIT (Just-In-Time) di dover generare le istruzioni native di sblocco fase di esecuzione dell'applicazione.  

Prima della versione 6, librerie del runtime di Entity Framework core facevano parte di .NET Framework e le immagini native generate automaticamente per loro. A partire dalla versione 6 l'intero runtime di Entity Framework è stato combinato nel pacchetto EntityFramework NuGet. Le immagini native sono a questo punto essere generato usando lo strumento da riga di comando NGen.exe per ottenere risultati simili.  

Osservazioni empiriche mostrano che le immagini native degli assembly di runtime di Entity Framework possono essere tagliate compreso tra 1 e 3 secondi di tempo di avvio dell'applicazione.  

## <a name="how-to-use-ngenexe"></a>Come usare NGen.exe  

La funzione di base dello strumento NGen.exe è "installare" (ovvero, per creare e salvare in modo permanente su disco) le immagini native per un assembly e tutte le dipendenze dirette. Ecco come è possibile ottenere questo risultato:  

1. Aprire una finestra del prompt dei comandi come amministratore  
2. Modificare la directory di lavoro corrente nel percorso degli assembly da generare le immagini native per:  

  ``` console
    cd <*Assemblies location*>  
  ```
3. A seconda del sistema operativo e la configurazione dell'applicazione è necessario generare le immagini native per un'architettura a 32 bit, architettura a 64 bit o per entrambi.  

    Per eseguire a 32 bit:  
  ``` console
    %WINDIR%\Microsoft.NET\Framework\v4.0.30319\ngen install <Assembly name>  
  ```
    Per eseguire a 64 bit:
  ``` console
    %WINDIR%\Microsoft.NET\Framework64\v4.0.30319\ngen install <Assembly name>  
  ```

> [!TIP]
> La generazione di immagini native per l'architettura non corretta è un errore molto comune. In caso di dubbio è semplicemente possibile generare le immagini native per tutte le architetture che si applicano al sistema operativo installato nel computer.  

NGen.exe supporta anche altre funzioni come la disinstallazione e la visualizzazione di immagini native installate, Accodamento la generazione di più immagini e così via. Per altri dettagli di utilizzo, vedere la [NGen.exe documentazione](https://msdn.microsoft.com/library/6t9t5wcf.aspx).  

## <a name="when-to-use-ngenexe"></a>Quando usare NGen.exe  

Se si desidera decidere quali assembly per generare immagini native per un'applicazione basata su Entity Framework 6 o versione successiva, è necessario considerare le opzioni seguenti:  

- **L'assembly di runtime EF principale, EntityFramework**: una tipica applicazione basato su Entity Framework esegue una quantità significativa di codice da questo assembly all'avvio o il primo accesso al database. Di conseguenza, produce il principale miglioramento delle prestazioni di avvio di creazione di immagini native di questo assembly.  
- **Qualsiasi assembly del provider di Entity Framework usato dall'applicazione**: tempo di avvio può inoltre beneficiare leggermente la generazione di immagini native di questi. Ad esempio, se l'applicazione usa il provider di Entity Framework per SQL Server si dovranno generare un'immagine nativa per EntityFramework.SqlServer.dll.  
- **Gli assembly dell'applicazione e altre dipendenze**: il [NGen.exe documentazione](https://msdn.microsoft.com/library/6t9t5wcf.aspx) copre criteri generali per la scelta degli assembly per generare l'impatto delle immagini native su sicurezza e le immagini native per cui opzioni, ad esempio "associazione forte," avanzate scenari come l'utilizzo di immagini native durante il debug e profilatura scenari e così via.  

> [!TIP]
> Assicurarsi di misurare l'impatto dell'uso delle immagini native in entrambe le prestazioni di avvio e le prestazioni complessive dell'applicazione con attenzione e confrontarle con i requisiti effettivi. Mentre le immagini native in genere consente di migliorare l'avvio di prestazioni e ridurre l'utilizzo della memoria in alcuni casi, non tutti gli scenari trarranno ugualmente. Ad esempio, in esecuzione in stato stabile (vale a dire, una volta che tutti i metodi utilizzati dall'applicazione sono stati richiamati almeno una volta) codice generato dal compilatore JIT può infatti offrire prestazioni leggermente migliori rispetto alle immagini native.  

## <a name="using-ngenexe-in-a-development-machine"></a>Uso di NGen.exe in un computer di sviluppo  

Durante lo sviluppo .NET JIT compilatore offrono il miglior compromesso complessivo per codice che cambia di frequente. Generazione di immagini native per le dipendenze compilate, ad esempio gli assembly di runtime di Entity Framework può aiutare ad accelerare lo sviluppo e testing da ritagliare pochi secondi all'inizio di ogni esecuzione.  

Un buon punto di trovare gli assembly di runtime di Entity Framework è il percorso del pacchetto NuGet per la soluzione. Ad esempio, per un'applicazione usando EF 6.0.2 con SQL Server e destinate a .NET 4.5 o versioni successive è possibile digitare quanto segue in una finestra del prompt dei comandi (ricordarsi di aprirlo come amministratore):  

``` console
cd <Solution directory>\packages\EntityFramework.6.0.2\lib\net45
%WINDIR%\Microsoft.NET\Framework\v4.0.30319\ngen install EntityFramework.SqlServer.dll
%WINDIR%\Microsoft.NET\Framework64\v4.0.30319\ngen install EntityFramework.SqlServer.dll
```  

> [!NOTE]
> Ciò consente di sfruttare il fatto che l'installazione di immagini native per il provider di Entity Framework per SQL Server per impostazione predefinita installerà anche le immagini native per l'assembly di runtime principale di Entity Framework. Questo procedimento funziona perché NGen.exe può rilevare che file EntityFramework. dll è una dipendenza diretta dell'assembly EntityFramework.SqlServer.dll che si trova nella stessa directory.  

## <a name="creating-native-images-during-setup"></a>Creazione di immagini native durante l'installazione  

Il WiX Toolkit supporta l'accodamento la generazione di immagini native per assembly gestiti durante l'installazione, come illustrato in questo [Guida dettagliata](http://wixtoolset.org/documentation/manual/v3/howtos/files_and_registry/ngen_managed_assemblies.html). Un'altra alternativa consiste nel creare un'attività di configurazione personalizzato che eseguire il comando NGen.exe.  

## <a name="verifying-that-native-images-are-being-used-for-ef"></a>Verifica che le immagini native vengano usate per Entity Framework  

È possibile verificare che una specifica applicazione utilizzi un assembly nativo mediante la ricerca di assembly caricati con estensione ". ni.dll"o". ni.exe". Ad esempio, un'immagine nativa per assembly di runtime principale di Entity Framework verrà chiamata EntityFramework.ni.dll. Un modo semplice per esaminare gli assembly .NET caricamento di un processo consiste nell'utilizzare [Process Explorer](https://technet.microsoft.com/sysinternals/bb896653).  

## <a name="other-things-to-be-aware-of"></a>Altri aspetti da tenere presenti  

**Creazione di un'immagine nativa di un assembly non deve essere confuso con la registrazione dell'assembly nel [GAC (Global Assembly Cache)](https://msdn.microsoft.com/library/yf1d93sz.aspx)**. NGen.exe consente la creazione di immagini di assembly che non si trovano nella Global Assembly Cache e in effetti, più applicazioni che utilizzano una versione specifica di Entity Framework possono condividere la stessa immagine nativa. Mentre Windows 8 può creare automaticamente le immagini native per gli assembly nella Global Assembly Cache, il runtime di Entity Framework è ottimizzato per distribuire insieme all'applicazione e non è consigliabile registrarlo nella CAG come questo ha un impatto negativo sulla risoluzione di assembly e per la manutenzione delle applicazioni tra gli altri aspetti.  
