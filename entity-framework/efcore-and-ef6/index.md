---
title: Confronto tra EF Core e EF6
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: a6b9cd22-6803-4c6c-a4d4-21147c0a81cb
uid: efcore-and-ef6/index
ms.openlocfilehash: 4442e6931327f6a07d98aee47211ddbeed51a467
ms.sourcegitcommit: 01a75cd483c1943ddd6f82af971f07abde20912e
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 10/27/2017
---
# <a name="compare-ef-core--ef6"></a>Confronto tra EF Core e EF6

Sono disponibili due versioni di Entity Framework, Entity Framework Core ed Entity Framework 6.

## <a name="entity-framework-6"></a>Entity Framework 6

Entity Framework 6 (EF6) è una tecnologia di accesso ai dati con funzionalità e stabilità comprovate da anni. Il primo rilascio risale al 2008, come parte di .NET Framework 3.5 SP1 e Visual Studio 2008 SP1. A partire dalla versione EF4.1, questa tecnologia è entrata a far parte del [pacchetto NuGet EntityFramework](https://www.nuget.org/packages/EntityFramework/), attualmente il più diffuso di NuGet.org.

EF6 continua a essere un prodotto supportato per il quale continueranno a essere generati miglioramenti secondari e correzioni di bug.

## <a name="entity-framework-core"></a>Entity Framework Core

Entity Framework Core (EF Core) è una versione leggera, estendibile e multipiattaforma di Entity Framework. EF Core introduce numerosi miglioramenti e nuove funzionalità rispetto a EF6. Allo stesso tempo, EF Core è una base di codice nuova, non altrettanto consolidata rispetto a EF6.

EF Core offre lo stesso tipo di esperienza di sviluppo di EF6 e la maggior parte delle API di primo livello rimangono invariate, in modo che risulti molto familiare a chi ha già usato EF6. Allo stesso tempo, EF Core si basa su un set di componenti principali completamente nuovo. Ciò significa che non eredita automaticamente tutte le funzionalità da EF6. Alcune di queste funzionalità verranno rese disponibili nelle versioni future, ad esempio il caricamento lazy e la resilienza di connessione. Altre funzionalità meno usate non verranno implementate in EF Core.

La nuova versione Core, leggera ed estendibile, ha reso possibile anche l'aggiunta di alcune funzionalità che non verranno implementate in EF6, come ad esempio le chiavi alternative e la valutazione mista client/database nelle query LINQ.
