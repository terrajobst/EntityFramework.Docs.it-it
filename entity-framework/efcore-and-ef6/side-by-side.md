---
title: EF6 ed EF Core - Uso nella stessa applicazione
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: a06e3c35-110c-4294-a1e2-32d2c31c90a7
uid: efcore-and-ef6/side-by-side
ms.openlocfilehash: 8bf9f51c0e5c4b1b3adf4a6a9a894689dc13d2d9
ms.sourcegitcommit: cbaa6cc89bd71d5e0bcc891e55743f0e8ea3393b
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/20/2019
ms.locfileid: "71149298"
---
# <a name="using-ef-core-and-ef6-in-the-same-application"></a><span data-ttu-id="e399c-102">Using di EF Core e di EF6 nella stessa applicazione</span><span class="sxs-lookup"><span data-stu-id="e399c-102">Using EF Core and EF6 in the Same Application</span></span>

<span data-ttu-id="e399c-103">È possibile usare EF Core e EF6 nella stessa applicazione o nella stessa libreria installando entrambi i pacchetti NuGet.</span><span class="sxs-lookup"><span data-stu-id="e399c-103">It is possible to use EF Core and EF6 in the same application or library by installing both NuGet packages.</span></span>

<span data-ttu-id="e399c-104">Poiché alcuni tipi hanno lo stesso nome in EF Core e in EF6 e si distinguono solo per lo spazio dei nomi, l'uso di EF Core ed EF6 nello stesso file di codice può risultare complicato.</span><span class="sxs-lookup"><span data-stu-id="e399c-104">Some types have the same names in EF Core and EF6 and differ only by namespace, which may complicate using both EF Core and EF6 in the same code file.</span></span> <span data-ttu-id="e399c-105">L'ambiguità può essere facilmente eliminata usando direttive alias dello spazio dei nomi.</span><span class="sxs-lookup"><span data-stu-id="e399c-105">The ambiguity can be easily removed using namespace alias directives.</span></span> <span data-ttu-id="e399c-106">Ad esempio:</span><span class="sxs-lookup"><span data-stu-id="e399c-106">For example:</span></span>

``` csharp
using Microsoft.EntityFrameworkCore; // use DbContext for EF Core
using EF6 = System.Data.Entity; // use EF6.DbContext for the EF6 version
```

<span data-ttu-id="e399c-107">Se si deve convertire un'applicazione esistente con più modelli EF, è possibile scegliere di convertirne solo alcuni in EF Core e di continuare a usare EF6 per gli altri.</span><span class="sxs-lookup"><span data-stu-id="e399c-107">If you are porting an existing application that has multiple EF models, you can choose to selectively port some of them to EF Core, and continue using EF6 for the others.</span></span>
