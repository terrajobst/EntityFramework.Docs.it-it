---
title: EF6 ed EF Core - Uso nella stessa applicazione
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: a06e3c35-110c-4294-a1e2-32d2c31c90a7
uid: efcore-and-ef6/side-by-side
ms.openlocfilehash: ead251c5454473738c2f2bfdac6557aa3e1c5591
ms.sourcegitcommit: bdd06c9a591ba5e6d6a3ec046c80de98f598f3f3
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 07/10/2018
ms.locfileid: "37949076"
---
# <a name="using-ef-core-and-ef6-in-the-same-application"></a><span data-ttu-id="473e0-102">Using di EF Core e di EF6 nella stessa applicazione</span><span class="sxs-lookup"><span data-stu-id="473e0-102">Using EF Core and EF6 in the Same Application</span></span>

<span data-ttu-id="473e0-103">È possibile usare EF Core ed EF6 nella stessa applicazione o libreria .NET Framework installando entrambi i pacchetti NuGet.</span><span class="sxs-lookup"><span data-stu-id="473e0-103">It is possible to use EF Core and EF6 in the same .NET Framework application or library by installing both NuGet packages.</span></span>

<span data-ttu-id="473e0-104">Poiché alcuni tipi hanno lo stesso nome in EF Core e in EF6 e si distinguono solo per lo spazio dei nomi, l'uso di EF Core ed EF6 nello stesso file di codice può risultare complicato.</span><span class="sxs-lookup"><span data-stu-id="473e0-104">Some types have the same names in EF Core and EF6 and differ only by namespace, which may complicate using both EF Core and EF6 in the same code file.</span></span> <span data-ttu-id="473e0-105">L'ambiguità può essere facilmente eliminata usando direttive alias dello spazio dei nomi.</span><span class="sxs-lookup"><span data-stu-id="473e0-105">The ambiguity can be easily removed using namespace alias directives.</span></span> <span data-ttu-id="473e0-106">Ad esempio:</span><span class="sxs-lookup"><span data-stu-id="473e0-106">For example:</span></span>

``` csharp
using Microsoft.EntityFrameworkCore; // use DbContext for EF Core
using EF6 = System.Data.Entity; // use EF6.DbContext for the EF6 version
```

<span data-ttu-id="473e0-107">Se si deve convertire un'applicazione esistente con più modelli EF, è possibile scegliere di convertirne solo alcuni in EF Core e di continuare a usare EF6 per gli altri.</span><span class="sxs-lookup"><span data-stu-id="473e0-107">If you are porting an existing application that has multiple EF models, you can choose to selectively port some of them to EF Core, and continue using EF6 for the others.</span></span>
