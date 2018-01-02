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
# <a name="compare-ef-core--ef6"></a><span data-ttu-id="740cc-102">Confronto tra EF Core e EF6</span><span class="sxs-lookup"><span data-stu-id="740cc-102">Compare EF Core & EF6</span></span>

<span data-ttu-id="740cc-103">Sono disponibili due versioni di Entity Framework, Entity Framework Core ed Entity Framework 6.</span><span class="sxs-lookup"><span data-stu-id="740cc-103">There are two versions of Entity Framework, Entity Framework Core and Entity Framework 6.</span></span>

## <a name="entity-framework-6"></a><span data-ttu-id="740cc-104">Entity Framework 6</span><span class="sxs-lookup"><span data-stu-id="740cc-104">Entity Framework 6</span></span>

<span data-ttu-id="740cc-105">Entity Framework 6 (EF6) è una tecnologia di accesso ai dati con funzionalità e stabilità comprovate da anni.</span><span class="sxs-lookup"><span data-stu-id="740cc-105">Entity Framework 6 (EF6) is a tried and tested data access technology with many years of features and stabilization.</span></span> <span data-ttu-id="740cc-106">Il primo rilascio risale al 2008, come parte di .NET Framework 3.5 SP1 e Visual Studio 2008 SP1.</span><span class="sxs-lookup"><span data-stu-id="740cc-106">It first released in 2008, as part of .NET Framework 3.5 SP1 and Visual Studio 2008 SP1.</span></span> <span data-ttu-id="740cc-107">A partire dalla versione EF4.1, questa tecnologia è entrata a far parte del [pacchetto NuGet EntityFramework](https://www.nuget.org/packages/EntityFramework/), attualmente il più diffuso di NuGet.org.</span><span class="sxs-lookup"><span data-stu-id="740cc-107">Starting with the EF4.1 release it has shipped as the [EntityFramework NuGet package](https://www.nuget.org/packages/EntityFramework/) - currently the most popular package on NuGet.org.</span></span>

<span data-ttu-id="740cc-108">EF6 continua a essere un prodotto supportato per il quale continueranno a essere generati miglioramenti secondari e correzioni di bug.</span><span class="sxs-lookup"><span data-stu-id="740cc-108">EF6 continues to be a supported product, and will continue to see bug fixes and minor improvements for some time to come.</span></span>

## <a name="entity-framework-core"></a><span data-ttu-id="740cc-109">Entity Framework Core</span><span class="sxs-lookup"><span data-stu-id="740cc-109">Entity Framework Core</span></span>

<span data-ttu-id="740cc-110">Entity Framework Core (EF Core) è una versione leggera, estendibile e multipiattaforma di Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="740cc-110">Entity Framework Core (EF Core) is a lightweight, extensible, and cross-platform version of Entity Framework.</span></span> <span data-ttu-id="740cc-111">EF Core introduce numerosi miglioramenti e nuove funzionalità rispetto a EF6.</span><span class="sxs-lookup"><span data-stu-id="740cc-111">EF Core introduces many improvements and new features when compared with EF6.</span></span> <span data-ttu-id="740cc-112">Allo stesso tempo, EF Core è una base di codice nuova, non altrettanto consolidata rispetto a EF6.</span><span class="sxs-lookup"><span data-stu-id="740cc-112">At the same time, EF Core is a new code base and not as mature as EF6.</span></span>

<span data-ttu-id="740cc-113">EF Core offre lo stesso tipo di esperienza di sviluppo di EF6 e la maggior parte delle API di primo livello rimangono invariate, in modo che risulti molto familiare a chi ha già usato EF6.</span><span class="sxs-lookup"><span data-stu-id="740cc-113">EF Core keeps the developer experience from EF6, and most of the top-level APIs remain the same too, so EF Core will feel very familiar to folks who have used EF6.</span></span> <span data-ttu-id="740cc-114">Allo stesso tempo, EF Core si basa su un set di componenti principali completamente nuovo.</span><span class="sxs-lookup"><span data-stu-id="740cc-114">At the same time, EF Core is built over a completely new set of core components.</span></span> <span data-ttu-id="740cc-115">Ciò significa che non eredita automaticamente tutte le funzionalità da EF6.</span><span class="sxs-lookup"><span data-stu-id="740cc-115">This means EF Core doesn't automatically inherit all the features from EF6.</span></span> <span data-ttu-id="740cc-116">Alcune di queste funzionalità verranno rese disponibili nelle versioni future, ad esempio il caricamento lazy e la resilienza di connessione. Altre funzionalità meno usate non verranno implementate in EF Core.</span><span class="sxs-lookup"><span data-stu-id="740cc-116">Some of these features will show up in future releases (such as lazy loading and connection resiliency), other less commonly used features will not be implemented in EF Core.</span></span>

<span data-ttu-id="740cc-117">La nuova versione Core, leggera ed estendibile, ha reso possibile anche l'aggiunta di alcune funzionalità che non verranno implementate in EF6, come ad esempio le chiavi alternative e la valutazione mista client/database nelle query LINQ.</span><span class="sxs-lookup"><span data-stu-id="740cc-117">The new, extensible, and lightweight core has also allowed us to add some features to EF Core that will not be implemented in EF6 (such as alternate keys and mixed client/database evaluation in LINQ queries).</span></span>
