---
title: Versioni e pianificazione di EF Core
author: ajcvickers
ms.date: 03/03/2020
ms.assetid: C21F89EE-FB08-4ED9-A2A0-76CB7656E6E4
uid: core/what-is-new/index
ms.openlocfilehash: 2c41f65d1fead8430a39c6230a0f22506686504e
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78413206"
---
# <a name="ef-core-releases-and-planning"></a>Versioni e pianificazione di EF Core

## <a name="stable-releases"></a>Versioni stabili

| Versione | Framework di destinazione | Supportato fino a | Collegamenti
|:--------|------------------|-----------------|------
| [EF Core 3.1](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore/3.1.2) | .NET Standard 2.0 | 3 dicembre 2022 (LTS) | [Annuncio](https://devblogs.microsoft.com/dotnet/announcing-entity-framework-core-3-1-and-entity-framework-6-4/)
| ~~[EF Core 3.0](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore/3.0.3)~~ | .NET Standard 2.1 | Scaduto il 3 marzo 2020 | [Annuncio](https://devblogs.microsoft.com/dotnet/announcing-ef-core-3-0-and-ef-6-3-general-availability/) / [Modifiche che causano un'interruzione](ef-core-3.0/breaking-changes.md)
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

## <a name="release-planning-and-schedules"></a>Pianificazione delle versioni

Le versioni EF Core sono allineate con la [pianificazione della distribuzione di .NET Core](https://github.com/dotnet/core/blob/master/roadmap.md).

Le versioni patch vengono in genere distribuite mensilmente, ma hanno un lead time lungo.
Stiamo lavorando per migliorare questo aspetto.

Per altre informazioni su come viene deciso quali elementi rilasciare in ogni versione, vedere il [processo di pianificazione delle versioni](release-planning.md).
In genere non viene eseguita una pianificazione dettagliata a lungo termine, oltre la versione principale o secondaria successiva.

## <a name="ef-core-50"></a>EF Core 5.0

La prossima versione stabile pianificata è **EF Core 5.0**, prevista per novembre 2020.

È stato creato un [piano generale per EF Core 5.0](ef-core-5.0/plan.md) seguendo il [processo di pianificazione delle versioni](release-planning.md) documentato.

I commenti e i suggerimenti dei clienti sulla pianificazione sono importanti.
Il modo migliore per indicare l'importanza di un problema consiste nel votare (pollice in su 👍) per tale problema in GitHub.
Questi dati verranno inclusi nel processo di pianificazione per la versione successiva.

### <a name="get-it-now"></a>Scaricare il software

I pacchetti di EF Core 5.0 sono **già disponibili** come [build giornaliere](https://github.com/aspnet/AspNetCore/blob/master/docs/DailyBuilds.md). 

L'uso delle build giornaliere è un ottimo modo per individuare i problemi e fornire commenti e suggerimenti il prima possibile.
Prima viene ricevuto tale feedback, maggiori saranno le probabilità di poter intervenire prima della successiva versione ufficiale.
Microsoft si impegna a fondo per fornire build giornaliere ottimali, eseguendo più di 56.000 test su ciascuna piattaforma per ogni build.

I pacchetti di anteprima saranno disponibili su NuGet più avanti nel corso dell'anno.
