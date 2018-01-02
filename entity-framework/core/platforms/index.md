---
title: Implementazioni di .NET supportate - EF Core
author: rowanmiller
ms.author: divega
ms.date: 08/30/2017
ms.technology: entity-framework-core
uid: core/platforms/index
ms.openlocfilehash: 6b6ea9ca833810169d0d850f3bea8776b761c007
ms.sourcegitcommit: 01a75cd483c1943ddd6f82af971f07abde20912e
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 10/27/2017
---
# <a name="net-implementations-supported-by-ef-core"></a>Implementazioni di .NET supportate da Entity Framework Core

EF Core sarà presto disponibile ovunque sia possibile scrivere codice .NET. La tabella seguente offre indicazioni per ogni implementazione .NET in cui verrà abilitato EF Core.

EF Core 2.0 è destinato a [.NET Standard 2.0](https://docs.microsoft.com/dotnet/standard/net-standard) e di conseguenza richiede implementazioni .NET che lo supportano.

| Implementazione .NET | Status | 1.x richiede | 2.x richiede
|-|-|-|-
| **.NET Core** ([ASP.NET Core](../get-started/aspnetcore/index.md), [Console](../get-started/netcore/index.md), ecc.) | **Completamente supportato e consigliato:** test automatici e correttamente funzionante in numerose applicazioni. | [.NET Core SDK 1.x](https://www.microsoft.com/net/core/) | [.NET Core SDK 2.x](https://www.microsoft.com/net/core/)
| **.NET Framework** (WinForms, WPF, ASP.NET, [Console](../get-started/full-dotnet/index.md), ecc.) | **Completamente supportato e consigliato:** test automatici e correttamente funzionante in numerose applicazioni. Anche EF 6 è disponibile in questa piattaforma (per scegliere la tecnologia più adatta, vedere [Confronto tra EF Core ed EF6](../../efcore-and-ef6/index.md)). | .NET Framework 4.5.1 | .NET Framework 4.6.1
| **Mono & Xamarin** | **In corso: è possibile che si verifichino problemi:** sono stati eseguiti alcuni test dal team EF Core e dai clienti. Sebbene i primi utenti abbiano riportato un funzionamento parzialmente corretto, [sono stati rilevati alcuni problemi](https://github.com/aspnet/entityframework/issues?q=is%3Aopen+is%3Aissue+label%3Aarea-xamarin) e altri verranno probabilmente individuati dai prossimi test. In particolare, alcune limitazioni in Xamarin.iOS possono impedire il corretto funzionamento di alcune applicazioni sviluppate usando EF Core 2.0. | Mono 4.6 <br/> Xamarin.iOS 10 <br/> Xamarin.Mac 3 <br/> Xamarin.Android 7 | Mono 5.4 <br/> Xamarin.iOS 10.14 <br/> Xamarin.Mac 3.8 <br/> Xamarin.Android 7.5
| [**Piattaforma UWP (Universal Windows Platform)**](../get-started/uwp/index.md) | **In corso: è possibile che si verifichino problemi:** sono stati eseguiti alcuni test dal team EF Core e dai clienti. Sono stati segnalati [problemi](https://github.com/aspnet/entityframework/issues?utf8=%E2%9C%93&q=is%3Aopen%20is%3Aissue%20label%3Aarea-uwp%20) con la compilazione tramite toolchain .NET Native solitamente usata per le build di rilascio e richiesta per la distribuzione in Windows Store (se non si usa .NET Native o si vuole semplicemente effettuare delle prove, numerosi di questi problemi non si verificano). | [Pacchetto .NET UWP 5 più recente](https://www.nuget.org/packages/Microsoft.NETCore.UniversalWindowsPlatform/5.4.1) | [Pacchetto .NET UWP 6 più recente](https://www.nuget.org/packages/Microsoft.NETCore.UniversalWindowsPlatform/) <sup>(1)</sup>

<sup>(1)</sup> Questa versione di .NET UWP offre il supporto di .NET Standard 2.0 e include .NET Native 2.0 che risolve la maggior parte dei problemi di compatibilità segnalati. I test hanno tuttavia rilevato [alcuni problemi tuttora esistenti](https://github.com/aspnet/EntityFrameworkCore/issues?q=is%3Aopen+is%3Aissue+milestone%3A2.0.1+label%3Aarea-uwp) con EF Core 2.0 che verranno risolti in una patch futura.

Per qualsiasi combinazione che non funziona come previsto, si consiglia di segnalare nuovi problemi nello [strumento di gestione dei problemi di EF Core](https://github.com/aspnet/entityframeworkcore/issues/new) e, per i problemi correlati a Xamarin, nello [strumento di gestione dei problemi di Xamarin](https://bugzilla.xamarin.com/newbug).
