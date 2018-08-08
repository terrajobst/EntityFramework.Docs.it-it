---
title: Installazione di EF Core
author: divega
ms.author: divega
ms.date: 08/06/2017
ms.assetid: 608cc774-c570-4809-8a3e-cd2c8446b8b2
ms.technology: entity-framework-core
uid: core/get-started/install/index
ms.openlocfilehash: 00924af2a7beaba8575e12d91678208b517f1a09
ms.sourcegitcommit: 902257be9c63c427dc793750a2b827d6feb8e38c
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 08/07/2018
ms.locfileid: "39614272"
---
# <a name="installing-ef-core"></a>Installazione di EF Core

## <a name="prerequisites"></a>Prerequisiti

Per sviluppare applicazioni .NET Core 2.1 (incluse le applicazioni ASP.NET Core 2.1 destinate a .NET Core) è necessario scaricare e installare una versione di [.NET Core 2.1 SDK](https://www.microsoft.com/net/download/core) appropriata per la piattaforma in uso. **Questo vale anche se è stato già installato Visual Studio 2017 versione 15.7.**

Per usare EF Core 2.1 o qualsiasi altra libreria .NET Standard 2.0 con una piattaforma .NET diversa da .NET 2.1 (ad esempio con .NET Framework 4.6.1 o versione successiva) è necessaria una versione di NuGet che supporta .NET Standard 2.0 e i framework compatibili. Ecco alcuni metodi per ottenere quanto sopra:

* Installare Visual Studio 2017 versione 15.7
* Se si usa Visual Studio 2015, [scaricare il client NuGet e aggiornarlo alla versione 3.6.0](https://www.nuget.org/downloads)

Per essere compatibili con le librerie .NET Standard 2.0, i progetti creati con versioni precedenti di Visual Studio e destinati a .NET Framework potrebbero richiedere modifiche aggiuntive:

* Modificare il file di progetto e assicurarsi che la voce seguente sia visualizzata nel gruppo di proprietà iniziale:
  ``` xml
  <AutoGenerateBindingRedirects>true</AutoGenerateBindingRedirects>
  ```

* Per i progetti di test, assicurarsi anche che sia presente la voce seguente:
  ``` xml
  <GenerateBindingRedirectsOutputType>true</GenerateBindingRedirectsOutputType>
  ```

## <a name="getting-the-bits"></a>Ottenimento dei componenti
Il metodo consigliato per l'aggiunta delle librerie di runtime di Entity Framework Core a un'applicazione è l'installazione di un provider di database EF Core da NuGet.

Oltre alle librerie di runtime è possibile installare strumenti che semplificano l'esecuzione di varie attività associate a EF Core nel progetto in fase di elaborazione, quali la creazione e l'applicazione di migrazioni e la creazione di un modello a partire da un database esistente.

> [!TIP]  
> Se si deve aggiornare un'applicazione che usa un provider di database di terze parti, verificare sempre se è disponibile un aggiornamento del provider compatibile con la versione di Entity Framework Core che si vuole usare. Ad esempio, i provider di database per le versioni precedenti non sono compatibili con la versione 2.1 del runtime di Entity Framework Core.  

> [!TIP]  
> Le applicazioni destinate ad ASP.NET Core 2.1 possono usare EF Core 2.1 senza dipendenze aggiuntive oltre ai provider di database di terze parti. Le applicazioni destinate a versioni precedenti di ASP.NET Core possono usare EF Core 2.1 dopo l'aggiornamento ad ASP.NET Core 2.1.

<a name="cli"></a>
### <a name="cross-platform-development-using-the-net-core-command-line-interface-cli"></a>Sviluppo multipiattaforma con l'interfaccia della riga di comando (CLI) .NET Core

Per sviluppare applicazioni destinate a [.NET Core](https://www.microsoft.com/net/download/core), è possibile scegliere di usare i [comandi dell'interfaccia della riga di comando `dotnet`](https://docs.microsoft.com/dotnet/core/tools/) in combinazione con l'editor di testo di preferenza oppure un ambiente di sviluppo integrato (IDE) come Visual Studio, Visual Studio per Mac o Visual Studio Code.

> [!IMPORTANT]  
> Le applicazioni destinate a .NET Core richiedono versioni specifiche di Visual Studio. Ad esempio, lo sviluppo per .NET Core 1.x richiede Visual Studio 2017, mentre lo sviluppo per .NET Core 2.1 richiede Visual Studio 2017 versione 15.7.

Per installare o aggiornare il provider SQL Server in un'applicazione .NET Core multipiattaforma, passare alla directory dell'applicazione ed eseguire le operazioni seguenti dalla riga di comando:

``` Console
dotnet add package Microsoft.EntityFrameworkCore.SqlServer
```

È possibile indicare l'installazione di una versione specifica nel comando `dotnet add package` mediante il modificatore `-v`. Ad esempio, per installare i pacchetti di EF Core 2.1 aggiungere `-v 2.1.0` al comando.

EF Core include un set di [comandi aggiuntivi per l'infrastruttura CLI `dotnet`](../../miscellaneous/cli/dotnet.md), a partire da `dotnet ef`. Gli strumenti dell'interfaccia della riga di comando di .NET Core per Entity Framework Core richiedono un pacchetto denominato `Microsoft.EntityFrameworkCore.Design`. È possibile aggiungerlo al progetto usando:

 ``` Console    
dotnet add package Microsoft.EntityFrameworkCore.Design 
``` 

> [!IMPORTANT]      
> Usare sempre le versioni dei pacchetti degli strumenti corrispondenti alla versione principale dei pacchetti di runtime.

<a name="visual-studio"></a>
### <a name="visual-studio-development"></a>Sviluppo in Visual Studio

In Visual Studio è possibile sviluppare molti tipi diversi di applicazioni destinate a .NET Core, .NET Framework o altre piattaforme supportate da EF Core.

Esistono due modi per installare un provider di database EF Core nell'applicazione da Visual Studio:

#### <a name="using-nugets-package-manager-user-interfacehttpsdocsmicrosoftcomnugettoolspackage-manager-ui"></a>Mediante [l'interfaccia Gestione pacchetti NuGet](https://docs.microsoft.com/nuget/tools/package-manager-ui)

* Selezionare il menu **Progetto > Gestisci pacchetti NuGet**

* Fare clic sulla scheda **Sfoglia** o sulla scheda **Aggiornamenti**

* Selezionare il pacchetto `Microsoft.EntityFrameworkCore.SqlServer` e la versione desiderata e confermare

#### <a name="using-nugets-package-manager-console-pmchttpsdocsmicrosoftcomnugettoolspackage-manager-console"></a>Mediante la [Console di Gestione pacchetti (PMC)](https://docs.microsoft.com/nuget/tools/package-manager-console) di NuGet

* Selezionare il menu **Strumenti > Gestione pacchetti NuGet > Console di Gestione pacchetti**

* Nella Console di Gestione pacchetti, digitare ed eseguire il comando seguente:

  ``` PowerShell  
  Install-Package Microsoft.EntityFrameworkCore.SqlServer
  ```
* In alternativa è possibile usare il comando `Update-Package` per aggiornare un pacchetto già installato a una versione più recente

* Per specificare una versione specifica, è possibile usare il modificatore `-Version`. Ad esempio, per installare i pacchetti di EF Core 2.1 aggiungere `-Version 2.1.0` ai comandi.

#### <a name="tools"></a>Strumenti

È anche disponibile una versione PowerShell dei [comandi EF Core eseguiti nella Console di Gestione pacchetti](../../miscellaneous/cli/powershell.md) in Visual Studio, con funzionalità simili a quelle dei comandi `dotnet ef`. 

> [!TIP]  
> Sebbene sia possibile usare i comandi `dotnet ef` della Console di Gestione pacchetti in Visual Studio, è molto più comodo usare la versione PowerShell:
> * I comandi funzionano automaticamente con il progetto corrente selezionato nella Console di Gestione pacchetti, senza richiedere il passaggio manuale da una directory all'altra.  
> * Dopo il completamento del comando, i file generati vengono aperti automaticamente in Visual Studio.

> [!IMPORTANT]  
> **Pacchetti deprecati in EF Core 2.1**: se si aggiorna un'applicazione esistente a EF Core 2.1, potrebbe essere necessario rimuovere manualmente alcuni riferimenti a pacchetti EF Core meno recenti:
> * I pacchetti della fase di progettazione per provider di database quali `Microsoft.EntityFrameworkCore.SqlServer.Design` non sono più necessari né supportati in EF Core 2.1, ma non vengono rimossi automaticamente quando si aggiornano gli altri pacchetti.
> * Gli strumenti dell'interfaccia della riga di comando di .NET sono ora inclusi in .NET SDK, in modo che il riferimento a tale pacchetto possa essere rimosso dal file *csproj*:
>   ```
>   <DotNetCliToolReference Include="Microsoft.EntityFrameworkCore.Tools.DotNet" Version="2.0.0" />
>   ```
