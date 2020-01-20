---
title: Versioni e pianificazione di EF Core
author: ajcvickers
ms.date: 01/14/2020
ms.assetid: C21F89EE-FB08-4ED9-A2A0-76CB7656E6E4
uid: core/what-is-new/index
ms.openlocfilehash: 8d74c24021fd62c5c5d944eaf3973b344fdb1e9c
ms.sourcegitcommit: f2a38c086291699422d8b28a72d9611d1b24ad0d
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 01/16/2020
ms.locfileid: "76124405"
---
# <a name="ef-core-releases-and-planning"></a>Versioni e pianificazione di EF Core

## <a name="stable-releases"></a>Versioni stabili

| Versione | Framework di destinazione | Supportato fino a | Collegamenti
|:--------|------------------|-----------------|------
| [EF Core 3.1](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore/3.1.1) | .NET Standard 2.0 | 3 dicembre 2022 (LTS) | [Annuncio](https://devblogs.microsoft.com/dotnet/announcing-entity-framework-core-3-1-and-entity-framework-6-4/)
| [EF Core 3.0](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore/3.0.1) | .NET Standard 2.1 | 3 marzo 2020 | [Annuncio](https://devblogs.microsoft.com/dotnet/announcing-ef-core-3-0-and-ef-6-3-general-availability/) / [Modifiche che causano un'interruzione](ef-core-3.0/breaking-changes.md)
| ~~[EF Core 2.2](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore/2.2.6)~~ | .NET Standard 2.0 | Scaduto il 23 dicembre 2019 | [Annuncio](https://devblogs.microsoft.com/dotnet/announcing-entity-framework-core-2-2/)
| [EF Core 2.1](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore/2.1.14) | .NET Standard 2.0 | 21 agosto 2021 (LTS) | [Annuncio](https://devblogs.microsoft.com/dotnet/announcing-entity-framework-core-2-1/)
| ~~[EF Core 2.0](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore/2.0.3)~~ | .NET Standard 2.0 | Scaduto il 1° ottobre 2018 | [Annuncio](https://devblogs.microsoft.com/dotnet/announcing-entity-framework-core-2-0/)
| ~~[EF Core 1.1](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore/1.1.6)~~ | .NET Standard 1.3 | Scaduto il 27 giugno 2019 | [Annuncio](https://devblogs.microsoft.com/dotnet/announcing-entity-framework-core-1-1/)
| ~~[EF Core 1.0](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore/1.0.6)~~ | .NET Standard 1.3 | Scaduto il 27 giugno 2019 | [Annuncio](https://devblogs.microsoft.com/dotnet/entity-framework-core-1-0-0-available/)

Per informazioni sulle specifiche piattaforme supportate da ogni versione di EF Core, vedere [Piattaforme supportate](../platforms/index.md).

Per informazioni sulla scadenza del supporto e sulle versioni con supporto a lungo termine (LTS), vedere [Criteri di supporto per .NET](https://dotnet.microsoft.com/platform/support/policy/dotnet-core).

## <a name="guidance-on-updating-to-new-releases"></a>Linee guida per l'aggiornamento alle nuove versioni

* Le versioni supportate includono patch per la sicurezza e altri bug critici. Usare sempre la patch più recente di una determinata versione. Ad esempio, per EF Core 2.1, usare la 2.1.14.
* Gli aggiornamenti delle versioni principali (ad esempio, da EF Core 2 a EF Core 3) spesso presentano modifiche che causano un'interruzione. Quando si effettua l'aggiornamento tra versioni principali, è consigliabile eseguire test approfonditi. Usare i collegamenti precedenti relativi alle modifiche che causano un'interruzione per informazioni aggiuntive sulla gestione di tali modifiche.
* Gli aggiornamenti delle versioni secondarie in genere non contengono modifiche che causano un'interruzione. È comunque consigliabile eseguire test approfonditi poiché le nuove funzionalità possono introdurre regressioni.

## <a name="ef-core-50"></a>EF Core 5.0

Le versioni EF Core sono allineate con la [pianificazione della distribuzione di .NET Core](https://github.com/dotnet/core/blob/master/roadmap.md). La prossima versione stabile pianificata è **EF Core 5.0**, prevista per novembre 2020.

È stato creato un [piano generale per EF Core 5.0](ef-core-5.0/plan.md) seguendo il [processo di pianificazione delle versioni](release-planning.md) documentato.

I commenti e i suggerimenti dei clienti sulla pianificazione sono importanti. Il modo migliore per indicare l'importanza di un problema consiste nel votare (Pollice in su) per tale problema in GitHub. Questi dati verranno inclusi nel processo di pianificazione per la versione successiva.

### <a name="get-it-now"></a>Scaricare il software

I pacchetti di EF Core 5.0 sono **già disponibili** come [build giornaliere](https://github.com/aspnet/AspNetCore/blob/master/docs/DailyBuilds.md). 

L'uso delle build giornaliere è un ottimo modo per individuare i problemi e fornire commenti e suggerimenti il prima possibile. Prima viene ricevuto tale feedback, maggiori saranno le probabilità di poter intervenire prima della successiva versione ufficiale. Microsoft si impegna a fondo per fornire build giornaliere ottimali, eseguendo più di 55.000 test su ciascuna piattaforma per ogni build.

I pacchetti di anteprima saranno disponibili su NuGet più avanti nel corso dell'anno.
