---
title: Informazioni di riferimento sulla riga di comando - EF Core
author: bricelam
ms.author: bricelam
ms.date: 11/06/2017
ms.openlocfilehash: d43b01fc61bb1c9b678e12e41c27d7efe9a59fa5
ms.sourcegitcommit: 2b787009fd5be5627f1189ee396e708cd130e07b
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 09/13/2018
ms.locfileid: "45490363"
---
<a name="entity-framework-core-tools"></a>Strumenti di Entity Framework Core
===========================
Gli strumenti di Entity Framework Core offrono un supporto durante lo sviluppo delle applicazioni EF Core. Vengono usati principalmente per eseguire lo scaffolding di un elemento DbContext e dei tipi di entità decompilando lo schema di un database e per gestire le migrazioni.

Gli [strumenti della Console di Gestione pacchetti di EF Core][1] offrono una migliore esperienza all'interno di Visual Studio. Per eseguirli, usare la [Console di Gestione pacchetti][2] di NuGet. Questi strumenti funzionano sia con i progetti .NET Framework che con i progetti .NET Core.

Gli [strumenti da riga di comando .NET di EF Core][3] sono un'estensione degli [strumenti dell'interfaccia della riga di comando di .NET Core][4] che sono multipiattaforma e possono essere eseguiti all'esterno di Visual Studio. Tali strumenti richiedono un progetto .NET Core SDK (uno con `Sdk="Microsoft.NET.Sdk"` o simili nel file di progetto).

Entrambi gli strumenti espongono la stessa funzionalità. Se sviluppa in Visual Studio, è consigliabile usare gli strumenti della Console di Gestione pacchetti poiché offrono un'esperienza più integrata.

<a name="frameworks"></a>Framework
----------
Gli strumenti supportano i progetti destinati a .NET Framework o .NET Core.

Se si vuole usare una libreria di classi, prendere in considerazione la possibilità di usare una libreria di classi .NET Framework o .NET Core, se possibile. Ciò consentirà di ridurre al minimo i problemi con gli strumenti .NET. Se invece si vuole usare una libreria di classi .NET Standard, è necessario usare un progetto di avvio destinato a .NET Framework o a .NET Core, in modo che gli strumenti abbiano una piattaforma di destinazione concreta in cui caricare la libreria di classi. Il progetto di avvio può essere un progetto fittizio senza vero codice. È infatti necessario solo per specificare una destinazione per gli strumenti.

Se il progetto è destinato a un altro framework (ad esempio, Windows universale o Xamarin), sarà necessario creare una libreria di classi .NET Standard separata. In questo caso, seguire le linee guida precedenti per creare anche un progetto di avvio che possa essere usato dagli strumenti.

<a name="startup-and-target-projects"></a>Progetti di avvio e destinazione
---------------------------
Ogni volta che si richiama un comando, sono coinvolti due progetti: il progetto di destinazione e il progetto di avvio.

Il progetto di destinazione è dove vengono aggiunti, in alcuni casi rimossi, tutti i file.

Il progetto di avvio è quello emulato dagli strumenti quando si esegue il codice del progetto.

Il progetto di destinazione e il progetto di avvio possono coincidere.


  [1]: powershell.md
  [2]: https://docs.microsoft.com/nuget/tools/package-manager-console
  [3]: dotnet.md
  [4]: https://docs.microsoft.com/dotnet/core/tools/
