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
# <a name="compare-ef-core--ef6"></a><span data-ttu-id="9674c-102">Confronto tra EF Core e EF6</span><span class="sxs-lookup"><span data-stu-id="9674c-102">Compare EF Core & EF6</span></span>

<span data-ttu-id="9674c-103">Sono disponibili due diverse versioni di Entity Framework, Entity Framework Core ed Entity Framework 6.</span><span class="sxs-lookup"><span data-stu-id="9674c-103">There are two different versions of Entity Framework, Entity Framework Core and Entity Framework 6.</span></span>

## <a name="entity-framework-6"></a><span data-ttu-id="9674c-104">Entity Framework 6</span><span class="sxs-lookup"><span data-stu-id="9674c-104">Entity Framework 6</span></span>

<span data-ttu-id="9674c-105">Entity Framework 6 (EF6) è una tecnologia di accesso ai dati con funzionalità e stabilità comprovate da anni.</span><span class="sxs-lookup"><span data-stu-id="9674c-105">Entity Framework 6 (EF6) is a tried and tested data access technology with many years of features and stabilization.</span></span> <span data-ttu-id="9674c-106">Il primo rilascio risale al 2008, come parte di .NET Framework 3.5 SP1 e Visual Studio 2008 SP1.</span><span class="sxs-lookup"><span data-stu-id="9674c-106">It first released in 2008, as part of .NET Framework 3.5 SP1 and Visual Studio 2008 SP1.</span></span> <span data-ttu-id="9674c-107">A partire dalla versione 4.1, Entity Framework è entrato a far parte del [pacchetto NuGet EntityFramework](https://www.nuget.org/packages/EntityFramework/), attualmente uno dei più diffusi di NuGet.org.</span><span class="sxs-lookup"><span data-stu-id="9674c-107">Starting with the EF4.1 release it has shipped as the [EntityFramework NuGet package](https://www.nuget.org/packages/EntityFramework/) - currently one of the most popular packages on NuGet.org.</span></span>

<span data-ttu-id="9674c-108">EF6 continua a essere un prodotto supportato per il quale continueranno a essere generati miglioramenti secondari e correzioni di bug.</span><span class="sxs-lookup"><span data-stu-id="9674c-108">EF6 continues to be a supported product, and will continue to see bug fixes and minor improvements for some time to come.</span></span>

## <a name="entity-framework-core"></a><span data-ttu-id="9674c-109">Entity Framework Core</span><span class="sxs-lookup"><span data-stu-id="9674c-109">Entity Framework Core</span></span>

<span data-ttu-id="9674c-110">Entity Framework Core (EF Core) è una versione leggera, estendibile e multipiattaforma di Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="9674c-110">Entity Framework Core (EF Core) is a lightweight, extensible, and cross-platform version of Entity Framework.</span></span> <span data-ttu-id="9674c-111">EF Core introduce numerosi miglioramenti e nuove funzionalità rispetto a EF6.</span><span class="sxs-lookup"><span data-stu-id="9674c-111">EF Core introduces many improvements and new features when compared with EF6.</span></span> <span data-ttu-id="9674c-112">Allo stesso tempo, EF Core è una base di codice nuova, non altrettanto consolidata rispetto a EF6.</span><span class="sxs-lookup"><span data-stu-id="9674c-112">At the same time, EF Core is a new code base and not as mature as EF6.</span></span>

<span data-ttu-id="9674c-113">EF Core offre lo stesso tipo di esperienza di sviluppo di EF6 e la maggior parte delle API di primo livello rimangono invariate, in modo che risulti molto familiare a chi ha già usato EF6.</span><span class="sxs-lookup"><span data-stu-id="9674c-113">EF Core keeps the developer experience from EF6, and most of the top-level APIs remain the same too, so EF Core will feel very familiar to folks who have used EF6.</span></span> <span data-ttu-id="9674c-114">Allo stesso tempo, EF Core si basa su un set di componenti principali completamente nuovo.</span><span class="sxs-lookup"><span data-stu-id="9674c-114">At the same time, EF Core is built over a completely new set of core components.</span></span> <span data-ttu-id="9674c-115">Ciò significa che non eredita automaticamente tutte le funzionalità da EF6.</span><span class="sxs-lookup"><span data-stu-id="9674c-115">This means EF Core doesn't automatically inherit all the features from EF6.</span></span> <span data-ttu-id="9674c-116">Alcune di queste funzionalità verranno rese disponibili nelle versioni future, altre funzionalità meno usate non verranno implementate in EF Core.</span><span class="sxs-lookup"><span data-stu-id="9674c-116">While some of these features will show up in future releases, other less commonly used features will not be implemented in EF Core.</span></span>

<span data-ttu-id="9674c-117">La nuova versione Core, leggera ed estendibile, ha reso possibile anche l'aggiunta di alcune funzionalità che non verranno implementate in EF6, ad esempio le chiavi alternative, gli aggiornamenti in batch e la valutazione mista client/database nelle query LINQ.</span><span class="sxs-lookup"><span data-stu-id="9674c-117">The new, extensible, and lightweight core has also allowed us to add some features to EF Core that will not be implemented in EF6 (such as alternate keys, batch updates, and mixed client/database evaluation in LINQ queries).</span></span>
