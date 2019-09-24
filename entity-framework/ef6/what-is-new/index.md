---
title: Novità - EF6
author: divega
ms.date: 09/12/2019
ms.assetid: 41d1f86b-ce66-4bf2-8963-48514406fb4c
ms.openlocfilehash: 568790d9c9bb7dd2213907bef8fa090710cd3ba0
ms.sourcegitcommit: cbaa6cc89bd71d5e0bcc891e55743f0e8ea3393b
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 09/20/2019
ms.locfileid: "71149121"
---
# <a name="whats-new-in-ef6"></a>Novità di EF6

Per ottenere le funzionalità più recenti e il livello di stabilità più elevato è consigliabile usare l'ultima versione rilasciata di Entity Framework.
È possibile, tuttavia, usare una versione precedente o provare i miglioramenti resi disponibili nella versione provvisoria più recente.
Per installare versioni specifiche di Entity Framework, vedere [Get Entity Framework](~/ef6/fundamentals/install.md) (Ottenere Entity Framework).

## <a name="ef-630"></a>EF 6.3.0

Il runtime di EF 6.3.0 è stato rilasciato in NuGet nel mese di settembre 2019. L'obiettivo principale di questa versione è quello di semplificare la migrazione delle applicazioni esistenti che usano EF 6 a .NET Core 3.0. La community ha anche contribuito a diverse correzioni di bug e miglioramenti. Per informazioni dettagliate, vedere i problemi chiusi in ogni [fase cardine](https://github.com/aspnet/EntityFramework6/milestones?state=closed) della versione 6.3.0. Ecco alcuni dei più importanti:

- Supporto per .NET Core 3.0
  - Il pacchetto EntityFramework ora ha come destinazione .NET Standard 2.1 oltre a .NET Framework 4.x
  - I comandi per le migrazioni sono stati riscritti per l'esecuzione out-of-process e supportano i progetti in stile SDK
- Supporto di HierarchyId di SQL Server
- Compatibilità migliorata con Roslyn e NuGet PackageReference
- Aggiunta di ef6.exe per l'abilitazione, l'aggiunta, l'esecuzione di script e l'applicazione di migrazioni da assembly. Sostituisce migrate.exe

## <a name="past-releases"></a>Versioni precedenti

La pagina [Versioni precedenti](past-releases.md) contiene un archivio di tutte le versioni precedenti di Entity Framework e delle funzionalità principali introdotte in ogni versione.
