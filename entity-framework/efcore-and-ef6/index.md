---
title: Confronto tra EF Core e EF6
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: a6b9cd22-6803-4c6c-a4d4-21147c0a81cb
uid: efcore-and-ef6/index
ms.openlocfilehash: 09ffd8408ea8575ea367eaf2bdab4002db5c619e
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 08/27/2018
ms.locfileid: "42997124"
---
# <a name="compare-ef-core--ef6"></a>Confronto tra EF Core e EF6

Sono disponibili due diverse versioni di Entity Framework, Entity Framework Core ed Entity Framework 6.

## <a name="entity-framework-6"></a>Entity Framework 6

Entity Framework 6 (EF6) è una tecnologia di accesso ai dati con funzionalità e stabilità comprovate da anni. Il primo rilascio risale al 2008, come parte di .NET Framework 3.5 SP1 e Visual Studio 2008 SP1. A partire dalla versione 4.1, Entity Framework è entrato a far parte del [pacchetto NuGet EntityFramework](https://www.nuget.org/packages/EntityFramework/), attualmente uno dei più diffusi di NuGet.org.

EF6 continua a essere un prodotto supportato per il quale continueranno a essere generati miglioramenti secondari e correzioni di bug.

## <a name="entity-framework-core"></a>Entity Framework Core

Entity Framework Core (EF Core) è una versione leggera, estendibile e multipiattaforma di Entity Framework. EF Core introduce numerosi miglioramenti e nuove funzionalità rispetto a EF6. Allo stesso tempo, EF Core è una base di codice nuova, non altrettanto consolidata rispetto a EF6.

EF Core offre lo stesso tipo di esperienza di sviluppo di EF6 e la maggior parte delle API di primo livello rimangono invariate, in modo che risulti molto familiare a chi ha già usato EF6. Allo stesso tempo, EF Core si basa su un set di componenti principali completamente nuovo. Ciò significa che non eredita automaticamente tutte le funzionalità da EF6. Alcune di queste funzionalità verranno rese disponibili nelle versioni future, altre funzionalità meno usate non verranno implementate in EF Core.

La nuova versione Core, leggera ed estendibile, ha reso possibile anche l'aggiunta di alcune funzionalità che non verranno implementate in EF6, ad esempio le chiavi alternative, gli aggiornamenti in batch e la valutazione mista client/database nelle query LINQ.
