---
title: Implementazioni di .NET supportate - EF Core
author: rowanmiller
ms.date: 08/30/2017
uid: core/platforms/index
ms.openlocfilehash: 8fc25f4a35794162c92fd292990c24e977d1bf1b
ms.sourcegitcommit: 5e11125c9b838ce356d673ef5504aec477321724
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 10/25/2018
ms.locfileid: "50022262"
---
# <a name="net-implementations-supported-by-ef-core"></a>Implementazioni di .NET supportate da Entity Framework Core

EF Core sarà presto disponibile ovunque sia possibile scrivere codice .NET. Mentre il supporto di EF Core in .NET Core e .NET Framework è accompagnato da test automatizzati ed è noto che molte applicazioni lo usano correttamente, esistono alcuni problemi per Mono, Xamarin e UWP.

## <a name="overview"></a>Panoramica

La tabella seguente offre indicazioni per ogni implementazione .NET:

| Implementazione .NET                                                                                                  | Status                                                             | Requisiti di EF Core 1.x                                                                                | Requisiti di EF Core 2.x <sup>(1)</sup>                                                                 |
|:---------------------------------------------------------------------------------------------------------------------|:-------------------------------------------------------------------|:--------------------------------------------------------------------------------------------------------|:--------------------------------------------------------------------------------------------------------|
| **.NET Core** ([ASP.NET Core](../get-started/aspnetcore/index.md), [Console](../get-started/netcore/index.md), ecc.) | Supporto completo e consigliato                                    | [.NET Core SDK 1.x](https://www.microsoft.com/net/core/)                                                | [.NET Core SDK 2.x](https://www.microsoft.com/net/core/)                                                |
| **.NET Framework** (WinForms, WPF, ASP.NET, [Console](../get-started/full-dotnet/index.md), ecc.)                    | Supporto completo e consigliato Disponibile anche EF6 <sup>(2)</sup> | .NET Framework 4.5.1                                                                                    | .NET Framework 4.6.1                                                                                    |
| **Mono & Xamarin**                                                                                                   | In corso <sup>(3)</sup>                                         | Mono 4.6 <br/> Xamarin.iOS 10 <br/> Xamarin.Mac 3 <br/> Xamarin.Android 7                               | Mono 5.4 <br/> Xamarin.iOS 10.14 <br/> Xamarin.Mac 3.8 <br/> Xamarin.Android 7.5                        |
| [**Piattaforma UWP (Universal Windows Platform)**](../get-started/uwp/index.md)                                                        | Consigliato EF Core 2.0.1 <sup>(4)</sup>                           | [Pacchetto .NET Core UWP 5.x](https://www.nuget.org/packages/Microsoft.NETCore.UniversalWindowsPlatform/) | [Pacchetto .NET Core UWP 6.x](https://www.nuget.org/packages/Microsoft.NETCore.UniversalWindowsPlatform/) |

<sup>(1)</sup> EF Core 2.0 è destinato a [.NET Standard 2.0](https://docs.microsoft.com/dotnet/standard/net-standard) e di conseguenza richiede implementazioni .NET che lo supportano.

<sup>(2)</sup> Vedere [Confronto tra EF Core ed EF6](../../efcore-and-ef6/index.md) per scegliere la tecnologia più adatta.

<sup>(3)</sup> Esistono alcuni problemi e limitazioni note per Xamarin, che possono impedire il corretto funzionamento di alcune applicazioni sviluppate usando EF Core 2.0. Controllare l'elenco dei [problemi attivi](https://github.com/aspnet/entityframeworkCore/issues?q=is%3Aopen+is%3Aissue+label%3Aarea-xamarin) per informazioni sulle soluzioni alternative.

<sup>(4)</sup> Vedere la sezione [Piattaforma UWP (Universal Windows Platform)](#universal-windows-platform) di questo articolo.

## <a name="universal-windows-platform"></a>Piattaforma UWP (Universal Windows Platform)

Le versioni precedenti di EF Core e .NET UWP includono numerosi problemi di compatibilità, in particolare con le applicazioni compilate con la toolchain .NET Native. La nuova versione di .NET UWP aggiunge il supporto per .NET 2.0 Standard e include .NET 2.0 Native, che consente di risolvere la maggior parte dei problemi di compatibilità segnalati in precedenza. EF Core 2.0.1 è stato testato in modo più approfondito con la piattaforma UWP ma i test non sono automatizzati.

Quando si usa EF Core nella piattaforma UWP:

* Per ottimizzare le prestazioni delle query, evitare i tipi anonimi nelle query LINQ. Per distribuire un'applicazione UWP nell'App Store, un'applicazione deve essere compilata con .NET Native. Le query con tipi anonimi hanno prestazioni peggiori in .NET Native.

* Per ottimizzare le prestazioni di `SaveChanges()`, usare [ChangeTrackingStrategy.ChangingAndChangedNotifications](/dotnet/api/microsoft.entityframeworkcore.changetrackingstrategy) e implementare [INotifyPropertyChanged](https://msdn.microsoft.com/library/system.componentmodel.inotifypropertychanged.aspx), [INotifyPropertyChanging ](https://msdn.microsoft.com/library/system.componentmodel.inotifypropertychanging.aspx) e [INotifyCollectionChanged](https://msdn.microsoft.com/library/system.collections.specialized.inotifycollectionchanged.aspx) nei tipi di entità.

## <a name="report-issues"></a>Segnalare i problemi

Per qualsiasi combinazione che non funziona come previsto, si consiglia di segnalare nuovi problemi nello [strumento di gestione dei problemi di EF Core](https://github.com/aspnet/entityframeworkcore/issues/new). Per problemi specifici di Xamarin, usare lo strumento di gestione dei problemi per [Xamarin.Android](https://github.com/xamarin/xamarin-android/issues/new) o [Xamarin.iOS](https://github.com/xamarin/xamarin-macios/issues/new).
