---
title: Introduzione a .NET Framework - Database esistente - EF Core
author: rowanmiller
ms.date: 08/06/2018
ms.assetid: a29a3d97-b2d8-4d33-9475-40ac67b3b2c6
uid: core/get-started/full-dotnet/existing-db
ms.openlocfilehash: b9e079f88dd35016407b19bb627f8bd46edb3d4c
ms.sourcegitcommit: ad1bdea58ed35d0f19791044efe9f72f94189c18
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 09/28/2018
ms.locfileid: "47447157"
---
# <a name="getting-started-with-ef-core-on-net-framework-with-an-existing-database"></a>Introduzione a EF Core in .NET Framework con un database esistente

In questa esercitazione verrà creata un'applicazione console che esegue l'accesso ai dati di base in un database di Microsoft SQL Server usando Entity Framework. Si creerà un modello di Entity Framework tramite reverse engineering di un database esistente.

[Visualizzare l'esempio di questo articolo su GitHub](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/GetStarted/FullNet/ConsoleApp.ExistingDb).

## <a name="prerequisites"></a>Prerequisiti

* [Visual Studio 2017 versione 15.7 o successive](https://www.visualstudio.com/downloads/)

## <a name="create-blogging-database"></a>Creare il database Blogging

Questa esercitazione usa un database **Blogging** nell'istanza di LocalDb come database esistente. Se il database **Blogging** è già stato creato durante un'altra esercitazione, ignorare questi passaggi.

* Aprire Visual Studio

* **Strumenti > Connetti al database**

* Selezionare **Microsoft SQL Server** e fare clic su **Continua**

* Immettere **(localdb)\mssqllocaldb** in **Nome server**

* Immettere **master** in **Nome database** e fare clic su **OK**

* Il database master viene ora visualizzato sotto **Connessioni dati** in **Esplora server**

* Fare clic con il pulsante destro del mouse sul database in **Esplora server** e scegliere **Nuova query**

* Copiare lo script riportato di seguito nell'editor di query

* Fare clic con il pulsante destro del mouse sull'editor di query e scegliere **Esegui**

[!code-sql[Main](../_shared/create-blogging-database-script.sql)]

## <a name="create-a-new-project"></a>Creare un nuovo progetto

* Aprire Visual Studio 2017

* **File > Nuovo > Progetto**

* Scegliere **Installati > Visual C# > Desktop Windows** dal menu a sinistra

* Selezionare il modello di progetto **App console (.NET Framework)**

* Assicurarsi che il progetto sia destinato a **.NET Framework 4.6.1** o versione successiva

* Denominare il progetto *ConsoleApp.ExistingDb* e fare clic su **OK**

## <a name="install-entity-framework"></a>Installare Entity Framework

Per usare EF Core, installare il pacchetto per i provider di database che si vuole specificare come destinazione. Questa esercitazione usa SQL Server. Per un elenco di provider disponibili, vedere [Provider di database](../../providers/index.md).

* **Strumenti > Gestione pacchetti NuGet > Console di Gestione pacchetti**

* Eseguire `Install-Package Microsoft.EntityFrameworkCore.SqlServer`

Nel passaggio successivo verranno usati alcuni strumenti di Entity Framework per il reverse engineering del database. Installare quindi anche il pacchetto degli strumenti.

* Eseguire `Install-Package Microsoft.EntityFrameworkCore.Tools`

## <a name="reverse-engineer-the-model"></a>Reverse engineering del modello

È ora possibile creare il modello di EF in base a un database esistente.

* **Strumenti –> Gestione pacchetti NuGet –> Console di Gestione pacchetti**

* Eseguire il comando seguente per creare un modello dal database esistente

  ``` powershell
  Scaffold-DbContext "Server=(localdb)\mssqllocaldb;Database=Blogging;Trusted_Connection=True;" Microsoft.EntityFrameworkCore.SqlServer
  ```

> [!TIP]  
> È possibile specificare le tabelle per cui generare le entità aggiungendo l'argomento `-Tables` al comando precedente. Ad esempio `-Tables Blog,Post`.

Il processo di reverse engineering ha creato le classi di entità (`Blog` e `Post`) e un contesto derivato (`BloggingContext`) in base allo schema del database esistente.

Le classi di entità sono semplici oggetti C# che rappresentano i dati di cui verranno eseguite query e che verranno salvati. Di seguito sono riportate le classi di entità `Blog` e `Post`:

 [!code-csharp[Main](../../../../samples/core/GetStarted/FullNet/ConsoleApp.ExistingDb/Blog.cs)]

[!code-csharp[Main](../../../../samples/core/GetStarted/FullNet/ConsoleApp.ExistingDb/Post.cs)]

> [!TIP]  
> Per abilitare il caricamento lazy, è possibile impostare proprietà di navigazione `virtual` (Blog.Post e Post.Blog).

Il contesto rappresenta una sessione con il database. Contiene i metodi che è possibile usare per eseguire query e salvare istanze delle classi di entità.

[!code-csharp[Main](../../../../samples/core/GetStarted/FullNet/ConsoleApp.ExistingDb/BloggingContext.cs)]

## <a name="use-the-model"></a>Usare il modello

È ora possibile usare il modello per eseguire l'accesso ai dati.

* Aprire *Program.cs*

* Sostituire tutti i contenuti del file con il codice seguente

  [!code-csharp[Main](../../../../samples/core/GetStarted/FullNet/ConsoleApp.ExistingDb/Program.cs)] 

* Debug > Avvia senza eseguire debug

  Si può notare che un blog viene salvato nel database e quindi che i dettagli di tutti i blog vengono visualizzati nella console.

  ![immagine](_static/output-existing-db.png)

## <a name="next-steps"></a>Passaggi successivi

Per altre informazioni su come eseguire lo scaffolding di un contesto e delle classi di entità, vedere gli articoli seguenti:
* [Informazioni di riferimento sugli strumenti di Entity Framework Core - Interfaccia della riga di comando di .NET](xref:core/miscellaneous/cli/dotnet#dotnet-ef-dbcontext-scaffold)
* [Informazioni di riferimento sugli strumenti di Entity Framework Core - Console di Gestione pacchetti](xref:core/miscellaneous/cli/powershell#scaffold-dbcontext)