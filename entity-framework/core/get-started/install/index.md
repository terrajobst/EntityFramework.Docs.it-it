---
title: Installazione di Entity Framework Core
author: divega
ms.date: 08/06/2017
ms.assetid: 608cc774-c570-4809-8a3e-cd2c8446b8b2
uid: core/get-started/install/index
ms.openlocfilehash: 7831e6a54e885cf0b89ef3eef2cd81a9292df606
ms.sourcegitcommit: 0d36e8ff0892b7f034b765b15e041f375f88579a
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 09/09/2018
ms.locfileid: "44250322"
---
# <a name="installing-entity-framework-core"></a>Installazione di Entity Framework Core

## <a name="prerequisites"></a>Prerequisiti

* Per sviluppare app destinate a .NET Core 2.1, installare [.NET Core 2.1 SDK](https://www.microsoft.com/net/download/core). Il SDK deve essere installato anche se è presente la versione più recente di Visual Studio 2017.

* Per usare Visual Studio per lo sviluppo di app destinate a .NET Core 2.1, installare Visual Studio 2017 versione 15.7 o successiva.

* Per usare Entity Framework 2.1 nelle applicazioni ASP.NET Core, usare ASP.NET Core 2.1. Le applicazioni che usano versioni precedenti di ASP.NET Core devono essere aggiornate alla versione 2.1.

* È possibile usare Visual Studio 2015 per le app destinate a .NET Framework 4.6.1 o versioni successive. Tuttavia è necessaria una versione di NuGet che supporta .NET Standard 2.0 e i framework compatibili. Per ottenere questa configurazione in Visual Studio 2015 [aggiornare il client NuGet alla versione 3.6.0](https://www.nuget.org/downloads).

## <a name="get-the-entity-framework-core-runtime"></a>Ottenere il runtime Entity Framework Core

Per aggiungere le librerie di runtime di Entity Framework Core a un'applicazione, installare il pacchetto NuGet del provider di database da usare. Per l'elenco dei provider supportati e dei nomi dei pacchetti NuGet corrispondenti, vedere [Provider di database](../../providers/index.md).

Per installare o aggiornare i pacchetti NuGet usare l'interfaccia della riga di comando di .NET Core, la finestra di dialogo Gestione pacchetti di Visual Studio o la console di gestione pacchetti di Visual Studio.

Per le applicazioni ASP.NET Core 2.1 i provider di SQL Server e in memoria vengono inclusi automaticamente, per cui non è necessario installarli separatamente.

> [!TIP]  
> Se si deve aggiornare un'applicazione che usa un provider di database di terze parti, verificare sempre se è disponibile un aggiornamento del provider compatibile con la versione di Entity Framework Core che si vuole usare. Ad esempio, i provider di database per le versioni precedenti non sono compatibili con la versione 2.1 del runtime di Entity Framework Core.  

### <a name="net-core-cli"></a>Interfaccia della riga di comando di .NET Core

Il seguente comando dell'interfaccia della riga di comando di .NET Core installa o aggiorna il provider SQL Server:

``` Console
dotnet add package Microsoft.EntityFrameworkCore.SqlServer
```

È possibile indicare una versione specifica nel comando `dotnet add package` mediante il modificatore `-v`. Ad esempio, per installare i pacchetti di EF Core 2.1.0 aggiungere `-v 2.1.0` al comando.

### <a name="visual-studio-nuget-package-manager-dialog"></a>Finestra di dialogo Gestione pacchetti NuGet in Visual Studio

* Nel menu selezionare **Progetto > Gestisci pacchetti NuGet**

* Fare clic sulla scheda **Sfoglia** o sulla scheda **Aggiornamenti**

* Per installare o aggiornare il provider SQL Server selezionare il pacchetto `Microsoft.EntityFrameworkCore.SqlServer` e confermare.

Per altre informazioni, vedere [Finestra di dialogo Gestione pacchetti NuGet](https://docs.microsoft.com/nuget/tools/package-manager-ui).

### <a name="visual-studio-nuget-package-manager-console"></a>Console di Gestione pacchetti NuGet di Visual Studio

* Nel menu selezionare **Strumenti > Gestione pacchetti NuGet > Console di Gestione pacchetti**

* Per installare il provider SQL Server eseguire il comando seguente nella console di Gestione pacchetti:

  ``` PowerShell  
  Install-Package Microsoft.EntityFrameworkCore.SqlServer
  ```
* Per aggiornare il provider, usare il comando `Update-Package`.

* Per indicare una versione specifica usare il modificatore `-Version`. Ad esempio, per installare i pacchetti di EF Core 2.1.0 aggiungere `-Version 2.1.0` ai comandi

Per altre informazioni, vedere [Console di Gestione pacchetti](https://docs.microsoft.com/nuget/tools/package-manager-console).

## <a name="get-entity-framework-core-tools"></a>Ottenere strumenti di Entity Framework Core

Oltre alle librerie di runtime, è possibile installare nel progetto strumenti che svolgono attività associate a EF Core in fase di progettazione. È ad esempio possibile creare migrazioni, applicare migrazioni e creare un modello basato su un database esistente.

Sono disponibili due set di strumenti:
* Gli [strumenti dell'interfaccia della riga di comando (CLI)](../../miscellaneous/cli/dotnet.md) di .NET Core possono essere usati in Windows, Linux o macOS. Questi comandi iniziano con `dotnet ef`. 
* Gli [strumenti della console di Gestione pacchetti](../../miscellaneous/cli/powershell.md) vengono eseguiti in Visual Studio 2017 su Windows. Questi comandi iniziano con un verbo, ad esempio `Add-Migration`, `Update-Database`.

Sebbene sia possibile usare i comandi `dotnet ef` della console di Gestione pacchetti, quando si usa Visual Studio risulta più pratico usare gli strumenti della console di Gestione pacchetti:
* Tali strumenti funzionano automaticamente con il progetto corrente selezionato nella console di Gestione pacchetti, senza richiedere il passaggio manuale da una directory all'altra.  
* Dopo il completamento del comando, i file generati vengono aperti automaticamente in Visual Studio.

<a name="cli"></a>

### <a name="get-the-cli-tools"></a>Ottenere gli strumenti dell'interfaccia della riga di comando (CLI)

I comandi `dotnet ef` sono inclusi in .NET Core SDK, ma per abilitarli è necessario installare il pacchetto `Microsoft.EntityFrameworkCore.Design`:

 ``` Console    
dotnet add package Microsoft.EntityFrameworkCore.Design 
``` 

Per le app ASP.NET Core 2.1 questo pacchetto è incluso automaticamente.

Come descritto in precedenza in [Prerequisiti](#prerequisites), è necessario installare anche .NET Core 2.1 SDK.

> [!IMPORTANT]      
> Usare sempre le versioni dei pacchetti degli strumenti corrispondenti alla versione principale dei pacchetti di runtime.

### <a name="get-the-package-manager-console-tools"></a>Ottenere gli strumenti della console di Gestione pacchetti

Per ottenere gli strumenti della console di Gestione pacchetti per EF Core, installare il pacchetto `Microsoft.EntityFrameworkCore.Tools`:

 ``` Console    
dotnet add package Microsoft.EntityFrameworkCore.Tools
``` 

Per le app ASP.NET Core 2.1 questo pacchetto è incluso automaticamente.

## <a name="upgrading-to-ef-core-21"></a>Aggiornamento a EF Core 2.1

Se si aggiorna un'applicazione esistente a EF Core 2.1, potrebbe essere necessario rimuovere manualmente alcuni riferimenti a pacchetti EF Core meno recenti:

* I pacchetti della fase di progettazione per provider di database quali `Microsoft.EntityFrameworkCore.SqlServer.Design` non sono più necessari né supportati in EF Core 2.1, ma non vengono rimossi automaticamente quando si aggiornano gli altri pacchetti.

* Gli strumenti dell'interfaccia della riga di comando di .NET sono ora inclusi in .NET SDK, in modo che il riferimento a tale pacchetto possa essere rimosso dal file *csproj*:

  ```
  <DotNetCliToolReference Include="Microsoft.EntityFrameworkCore.Tools.DotNet" Version="2.0.0" />
  ```

Nel caso di applicazioni destinate a .NET Framework e create in versioni precedenti di Visual Studio, assicurarsi che siano compatibili con le librerie .NET Standard 2.0:

  * Modificare il file di progetto e assicurarsi che la voce seguente sia visualizzata nel gruppo di proprietà iniziale:

    ``` xml
    <AutoGenerateBindingRedirects>true</AutoGenerateBindingRedirects>
    ```

  * Per i progetti di test, assicurarsi anche che sia presente la voce seguente:

    ``` xml
    <GenerateBindingRedirectsOutputType>true</GenerateBindingRedirectsOutputType>
    ```
