---
title: EF6 ed EF Core - Uso nella stessa applicazione
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: a06e3c35-110c-4294-a1e2-32d2c31c90a7
uid: efcore-and-ef6/side-by-side
ms.openlocfilehash: f6eb4bf7d99fbc61f8ffbd0dc7c6c17789395303
ms.sourcegitcommit: 01a75cd483c1943ddd6f82af971f07abde20912e
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 10/27/2017
---
# <a name="using-ef-core-and-ef6-in-the-same-application"></a><span data-ttu-id="03374-102">Using di EF Core e di EF6 nella stessa applicazione</span><span class="sxs-lookup"><span data-stu-id="03374-102">Using EF Core and EF6 in the Same Application</span></span>

<span data-ttu-id="03374-103">È possibile usare EF Core ed EF6 nella stessa applicazione o libreria .NET Framework installando entrambi i pacchetti NuGet.</span><span class="sxs-lookup"><span data-stu-id="03374-103">It is possible to use EF Core and EF6 in the same .NET Framework application or library by installing both NuGet packages.</span></span> 

<span data-ttu-id="03374-104">Poiché alcuni tipi hanno lo stesso nome in EF Core e in EF6 e si distinguono solo per lo spazio dei nomi, l'uso di EF Core ed EF6 nello stesso file di codice può risultare complicato.</span><span class="sxs-lookup"><span data-stu-id="03374-104">Some types have the same names in EF Core and EF6 and differ only by namespace, which may complicate using both EF Core and EF6 in the same code file.</span></span> <span data-ttu-id="03374-105">L'ambiguità può essere facilmente eliminata usando direttive alias dello spazio dei nomi, ad esempio:</span><span class="sxs-lookup"><span data-stu-id="03374-105">The ambiguity can be easily removed using namespace alias directives, e.g.:</span></span>

``` csharp
using Microsoft.EntityFrameworkCore;
using EF6 = System.Data.Entity; // e.g. EF6.DbContext
```

<span data-ttu-id="03374-106">Se si deve convertire un'applicazione esistente con più modelli EF, è possibile scegliere di convertirne solo alcuni in EF Core e di continuare a usare EF6 per gli altri.</span><span class="sxs-lookup"><span data-stu-id="03374-106">If you are porting an existing application that has multiple EF models, you can choose to selectively port some of them to EF Core, and continue using EF6 for the others.</span></span>
