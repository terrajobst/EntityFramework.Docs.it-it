---
title: Informazioni di riferimento sulla riga di comando - EF Core
author: bricelam
ms.author: bricelam
ms.date: 11/6/2017
ms.technology: entity-framework-core
ms.openlocfilehash: 076e9251850ba10df323cd25922aa8b95b3a5491
ms.sourcegitcommit: 5e2d97e731f975cf3405ff3deab2a3c75ad1b969
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 11/15/2017
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

Se il progetto è destinato a un altro framework, ad esempio Windows universale o Xamarin, si consiglia di creare un progetto .NET Standard separato e destinarlo in modo trasversale a uno dei framework supportati.

Per specificare .NET Core come destinazione trasversale, ad esempio, fare clic sul progetto con il pulsante destro del mouse e selezionare **Modifica \*.csproj**. Aggiornare la proprietà `TargetFramework` come indicato di seguito (si noti che il nome della proprietà diventa plurale).

``` xml
<TargetFrameworks>netcoreapp2.0;netstandard2.0</TargetFrameworks>
```

Se si usa una libreria di classi .NET Standard, la destinazione trasversale non è necessaria se il progetto di avvio ha come destinazione .NET Framework o .NET Core.

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
