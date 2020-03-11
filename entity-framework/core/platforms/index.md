---
title: Implementazioni di .NET supportate - EF Core
author: bricelam
ms.date: 03/03/2020
uid: core/platforms/index
ms.openlocfilehash: 693d4cae85eddf86d01e17084415147c52a008c7
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78413066"
---
# <a name="net-implementations-supported-by-ef-core"></a><span data-ttu-id="e9262-102">Implementazioni di .NET supportate da Entity Framework Core</span><span class="sxs-lookup"><span data-stu-id="e9262-102">.NET implementations supported by EF Core</span></span>

<span data-ttu-id="e9262-103">Microsoft vuole che EF Core sia disponibile per gli sviluppatori in tutte le implementazioni di .NET moderne e sta ancora lavorando per questo obiettivo.</span><span class="sxs-lookup"><span data-stu-id="e9262-103">We want EF Core to be available to developers on all modern .NET implementations, and we're still working towards that goal.</span></span> <span data-ttu-id="e9262-104">Mentre il supporto di EF Core in .NET Core è accompagnato da test automatizzati ed è noto che molte applicazioni lo usano correttamente, esistono alcuni problemi per Mono, Xamarin e UWP.</span><span class="sxs-lookup"><span data-stu-id="e9262-104">While EF Core's support on .NET Core is covered by automated testing and many applications known to be using it successfully, Mono, Xamarin and UWP have some issues.</span></span>

## <a name="overview"></a><span data-ttu-id="e9262-105">Panoramica</span><span class="sxs-lookup"><span data-stu-id="e9262-105">Overview</span></span>

<span data-ttu-id="e9262-106">La tabella seguente offre indicazioni per ogni implementazione .NET:</span><span class="sxs-lookup"><span data-stu-id="e9262-106">The following table provides guidance for each .NET implementation:</span></span>

| <span data-ttu-id="e9262-107">EF Core</span><span class="sxs-lookup"><span data-stu-id="e9262-107">EF Core</span></span>                       | <span data-ttu-id="e9262-108">2.1 e 3.1</span><span class="sxs-lookup"><span data-stu-id="e9262-108">2.1 and 3.1</span></span> |
|:------------------------------|:------------|
| <span data-ttu-id="e9262-109">.NET Standard</span><span class="sxs-lookup"><span data-stu-id="e9262-109">.NET Standard</span></span>                 | <span data-ttu-id="e9262-110">2.0</span><span class="sxs-lookup"><span data-stu-id="e9262-110">2.0</span></span>         |
| <span data-ttu-id="e9262-111">.NET Core</span><span class="sxs-lookup"><span data-stu-id="e9262-111">.NET Core</span></span>                     | <span data-ttu-id="e9262-112">2.0</span><span class="sxs-lookup"><span data-stu-id="e9262-112">2.0</span></span>         |
| <span data-ttu-id="e9262-113">.NET Framework<sup>(1)</sup></span><span class="sxs-lookup"><span data-stu-id="e9262-113">.NET Framework<sup>(1)</sup></span></span>  | <span data-ttu-id="e9262-114">4.7.2</span><span class="sxs-lookup"><span data-stu-id="e9262-114">4.7.2</span></span>       |
| <span data-ttu-id="e9262-115">Mono</span><span class="sxs-lookup"><span data-stu-id="e9262-115">Mono</span></span>                          | <span data-ttu-id="e9262-116">5.4</span><span class="sxs-lookup"><span data-stu-id="e9262-116">5.4</span></span>         |
| <span data-ttu-id="e9262-117">Xamarin.iOS<sup>(2)</sup></span><span class="sxs-lookup"><span data-stu-id="e9262-117">Xamarin.iOS<sup>(2)</sup></span></span>     | <span data-ttu-id="e9262-118">10.14</span><span class="sxs-lookup"><span data-stu-id="e9262-118">10.14</span></span>       |
| <span data-ttu-id="e9262-119">Xamarin.Android<sup>(2)</sup></span><span class="sxs-lookup"><span data-stu-id="e9262-119">Xamarin.Android<sup>(2)</sup></span></span> | <span data-ttu-id="e9262-120">8.0</span><span class="sxs-lookup"><span data-stu-id="e9262-120">8.0</span></span>         |
| <span data-ttu-id="e9262-121">UWP<sup>(3)</sup></span><span class="sxs-lookup"><span data-stu-id="e9262-121">UWP<sup>(3)</sup></span></span>             | <span data-ttu-id="e9262-122">10.0.16299</span><span class="sxs-lookup"><span data-stu-id="e9262-122">10.0.16299</span></span>  |
| <span data-ttu-id="e9262-123">Unity<sup>(4)</sup></span><span class="sxs-lookup"><span data-stu-id="e9262-123">Unity<sup>(4)</sup></span></span>           | <span data-ttu-id="e9262-124">2018.1</span><span class="sxs-lookup"><span data-stu-id="e9262-124">2018.1</span></span>      |

<span data-ttu-id="e9262-125"><sup>(1)</sup> Vedere la sezione [.NET Framework](#net-framework) di seguito.</span><span class="sxs-lookup"><span data-stu-id="e9262-125"><sup>(1)</sup> See the [.NET Framework](#net-framework) section below.</span></span>

<span data-ttu-id="e9262-126"><sup>(2)</sup> Esistono alcuni problemi e limitazioni noti per Xamarin, che possono impedire il corretto funzionamento di alcune applicazioni sviluppate usando EF Core.</span><span class="sxs-lookup"><span data-stu-id="e9262-126"><sup>(2)</sup> There are issues and known limitations with Xamarin which may prevent some applications developed using EF Core from working correctly.</span></span> <span data-ttu-id="e9262-127">Controllare l'elenco dei [problemi attivi](https://github.com/aspnet/entityframeworkCore/issues?q=is%3Aopen+is%3Aissue+label%3Aarea-xamarin) per informazioni sulle soluzioni alternative.</span><span class="sxs-lookup"><span data-stu-id="e9262-127">Check the list of [active issues](https://github.com/aspnet/entityframeworkCore/issues?q=is%3Aopen+is%3Aissue+label%3Aarea-xamarin) for workarounds.</span></span>

<span data-ttu-id="e9262-128"><sup>3 </sup> Si consiglia EF Core 2.0.1 e versioni più recenti.</span><span class="sxs-lookup"><span data-stu-id="e9262-128"><sup>(3)</sup> EF Core 2.0.1 and newer recommended.</span></span> <span data-ttu-id="e9262-129">Installare il [pacchetto .NET Core UWP 6.x](https://www.nuget.org/packages/Microsoft.NETCore.UniversalWindowsPlatform/).</span><span class="sxs-lookup"><span data-stu-id="e9262-129">Install the [.NET Core UWP 6.x package](https://www.nuget.org/packages/Microsoft.NETCore.UniversalWindowsPlatform/).</span></span> <span data-ttu-id="e9262-130">Vedere la sezione [Piattaforma UWP (Universal Windows Platform)](#universal-windows-platform) di questo articolo.</span><span class="sxs-lookup"><span data-stu-id="e9262-130">See the [Universal Windows Platform](#universal-windows-platform) section of this article.</span></span>

<span data-ttu-id="e9262-131"><sup>(4)</sup> Esistono problemi e limitazioni noti con Unity.</span><span class="sxs-lookup"><span data-stu-id="e9262-131"><sup>(4)</sup> There are issues and known limitations with Unity.</span></span> <span data-ttu-id="e9262-132">Controllare l'elenco dei [problemi attivi](https://github.com/aspnet/entityframeworkCore/issues?q=is%3Aopen+is%3Aissue+label%3Aarea-unity).</span><span class="sxs-lookup"><span data-stu-id="e9262-132">Check the list of [active issues](https://github.com/aspnet/entityframeworkCore/issues?q=is%3Aopen+is%3Aissue+label%3Aarea-unity).</span></span>

## <a name="net-framework"></a><span data-ttu-id="e9262-133">.NET Framework</span><span class="sxs-lookup"><span data-stu-id="e9262-133">.NET Framework</span></span>

<span data-ttu-id="e9262-134">Le applicazioni che hanno .NET Framework come destinazione potrebbero richiedere modifiche per funzionare con le librerie .NET Standard:</span><span class="sxs-lookup"><span data-stu-id="e9262-134">Applications that target .NET Framework may need changes to work with .NET Standard libraries:</span></span>

<span data-ttu-id="e9262-135">Modificare il file di progetto e assicurarsi che la voce seguente sia visualizzata nel gruppo di proprietà iniziale:</span><span class="sxs-lookup"><span data-stu-id="e9262-135">Edit the project file and make sure the following entry appears in the initial property group:</span></span>

``` xml
<AutoGenerateBindingRedirects>true</AutoGenerateBindingRedirects>
```

<span data-ttu-id="e9262-136">Per i progetti di test, assicurarsi anche che sia presente la voce seguente:</span><span class="sxs-lookup"><span data-stu-id="e9262-136">For test projects, also make sure the following entry is present:</span></span>

``` xml
<GenerateBindingRedirectsOutputType>true</GenerateBindingRedirectsOutputType>
```

<span data-ttu-id="e9262-137">Se si vuole usare una versione precedente di Visual Studio, assicurarsi di [aggiornare il client NuGet alla versione 3.6.0](https://www.nuget.org/downloads) per lavorare con le librerie .NET Standard 2.0.</span><span class="sxs-lookup"><span data-stu-id="e9262-137">If you want to use an older version of Visual Studio, make sure you [upgrade the NuGet client to version 3.6.0](https://www.nuget.org/downloads) to work with .NET Standard 2.0 libraries.</span></span>

<span data-ttu-id="e9262-138">Si consiglia anche di eseguire la migrazione da packages.config NuGet a PackageReference, se possibile.</span><span class="sxs-lookup"><span data-stu-id="e9262-138">We also recommend migrating from NuGet packages.config to PackageReference if possible.</span></span> <span data-ttu-id="e9262-139">Aggiungere la proprietà seguente al file di progetto:</span><span class="sxs-lookup"><span data-stu-id="e9262-139">Add the following property to your project file:</span></span>

``` xml
<RestoreProjectStyle>PackageReference</RestoreProjectStyle>
```

## <a name="universal-windows-platform"></a><span data-ttu-id="e9262-140">Piattaforma UWP (Universal Windows Platform)</span><span class="sxs-lookup"><span data-stu-id="e9262-140">Universal Windows Platform</span></span>

<span data-ttu-id="e9262-141">Le versioni precedenti di EF Core e .NET UWP includono numerosi problemi di compatibilità, in particolare con le applicazioni compilate con la toolchain .NET Native.</span><span class="sxs-lookup"><span data-stu-id="e9262-141">Earlier versions of EF Core and .NET UWP had numerous compatibility issues, especially with applications compiled with the .NET Native toolchain.</span></span> <span data-ttu-id="e9262-142">La nuova versione di .NET UWP aggiunge il supporto per .NET 2.0 Standard e include .NET 2.0 Native, che consente di risolvere la maggior parte dei problemi di compatibilità segnalati in precedenza.</span><span class="sxs-lookup"><span data-stu-id="e9262-142">The new .NET UWP version adds support for .NET Standard 2.0 and contains .NET Native 2.0, which fixes most of the compatibility issues previously reported.</span></span> <span data-ttu-id="e9262-143">EF Core 2.0.1 è stato testato in modo più approfondito con la piattaforma UWP ma i test non sono automatizzati.</span><span class="sxs-lookup"><span data-stu-id="e9262-143">EF Core 2.0.1 has been tested more thoroughly with UWP but testing is not automated.</span></span>

<span data-ttu-id="e9262-144">Quando si usa EF Core nella piattaforma UWP:</span><span class="sxs-lookup"><span data-stu-id="e9262-144">When using EF Core on UWP:</span></span>

* <span data-ttu-id="e9262-145">Per ottimizzare le prestazioni delle query, evitare i tipi anonimi nelle query LINQ.</span><span class="sxs-lookup"><span data-stu-id="e9262-145">To optimize query performance, avoid anonymous types in LINQ queries.</span></span> <span data-ttu-id="e9262-146">Per distribuire un'applicazione UWP nell'App Store, un'applicazione deve essere compilata con .NET Native.</span><span class="sxs-lookup"><span data-stu-id="e9262-146">Deploying a UWP application to the app store requires an application to be compiled with .NET Native.</span></span> <span data-ttu-id="e9262-147">Le query con tipi anonimi hanno prestazioni peggiori in .NET Native.</span><span class="sxs-lookup"><span data-stu-id="e9262-147">Queries with anonymous types have worse performance on .NET Native.</span></span>

* <span data-ttu-id="e9262-148">Per ottimizzare le prestazioni di `SaveChanges()`, usare [ChangeTrackingStrategy.ChangingAndChangedNotifications](/dotnet/api/microsoft.entityframeworkcore.changetrackingstrategy) e implementare [INotifyPropertyChanged](https://msdn.microsoft.com/library/system.componentmodel.inotifypropertychanged.aspx), [INotifyPropertyChanging ](https://msdn.microsoft.com/library/system.componentmodel.inotifypropertychanging.aspx) e [INotifyCollectionChanged](https://msdn.microsoft.com/library/system.collections.specialized.inotifycollectionchanged.aspx) nei tipi di entità.</span><span class="sxs-lookup"><span data-stu-id="e9262-148">To optimize `SaveChanges()` performance, use [ChangeTrackingStrategy.ChangingAndChangedNotifications](/dotnet/api/microsoft.entityframeworkcore.changetrackingstrategy) and implement [INotifyPropertyChanged](https://msdn.microsoft.com/library/system.componentmodel.inotifypropertychanged.aspx), [INotifyPropertyChanging](https://msdn.microsoft.com/library/system.componentmodel.inotifypropertychanging.aspx), and [INotifyCollectionChanged](https://msdn.microsoft.com/library/system.collections.specialized.inotifycollectionchanged.aspx) in your entity types.</span></span>

## <a name="report-issues"></a><span data-ttu-id="e9262-149">Segnalare i problemi</span><span class="sxs-lookup"><span data-stu-id="e9262-149">Report issues</span></span>

<span data-ttu-id="e9262-150">Per qualsiasi combinazione che non funziona come previsto, si consiglia di segnalare nuovi problemi nello [strumento di gestione dei problemi di EF Core](https://github.com/aspnet/entityframeworkcore/issues/new).</span><span class="sxs-lookup"><span data-stu-id="e9262-150">For any combination that doesn�t work as expected, we encourage creating new issues on the [EF Core issue tracker](https://github.com/aspnet/entityframeworkcore/issues/new).</span></span> <span data-ttu-id="e9262-151">Per problemi specifici di Xamarin, usare lo strumento di gestione dei problemi per [Xamarin.Android](https://github.com/xamarin/xamarin-android/issues/new) o [Xamarin.iOS](https://github.com/xamarin/xamarin-macios/issues/new).</span><span class="sxs-lookup"><span data-stu-id="e9262-151">For Xamarin-specific issues use the issue tracker for [Xamarin.Android](https://github.com/xamarin/xamarin-android/issues/new) or [Xamarin.iOS](https://github.com/xamarin/xamarin-macios/issues/new).</span></span>
