---
title: EF6 ed EF Core - Uso nella stessa applicazione
author: ajcvickers
ms.date: 01/23/2019
ms.assetid: a06e3c35-110c-4294-a1e2-32d2c31c90a7
uid: efcore-and-ef6/side-by-side
ms.openlocfilehash: bcf0a26535c4ec880a9ac25478c987fb683f6d26
ms.sourcegitcommit: b3cf5d2e3cb170b9916795d1d8c88678269639b1
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2020
ms.locfileid: "76888135"
---
# <a name="using-ef-core-and-ef6-in-the-same-application"></a><span data-ttu-id="34aaa-102">Using di EF Core e di EF6 nella stessa applicazione</span><span class="sxs-lookup"><span data-stu-id="34aaa-102">Using EF Core and EF6 in the Same Application</span></span>

<span data-ttu-id="34aaa-103">È possibile usare EF Core e EF6 nella stessa applicazione o nella stessa libreria installando entrambi i pacchetti NuGet.</span><span class="sxs-lookup"><span data-stu-id="34aaa-103">It is possible to use EF Core and EF6 in the same application or library by installing both NuGet packages.</span></span>

<span data-ttu-id="34aaa-104">Poiché alcuni tipi hanno lo stesso nome in EF Core e in EF6 e si distinguono solo per lo spazio dei nomi, l'uso di EF Core ed EF6 nello stesso file di codice può risultare complicato.</span><span class="sxs-lookup"><span data-stu-id="34aaa-104">Some types have the same names in EF Core and EF6 and differ only by namespace, which may complicate using both EF Core and EF6 in the same code file.</span></span> <span data-ttu-id="34aaa-105">L'ambiguità può essere facilmente eliminata usando direttive alias dello spazio dei nomi.</span><span class="sxs-lookup"><span data-stu-id="34aaa-105">The ambiguity can be easily removed using namespace alias directives.</span></span> <span data-ttu-id="34aaa-106">Ad esempio:</span><span class="sxs-lookup"><span data-stu-id="34aaa-106">For example:</span></span>

``` csharp
using Microsoft.EntityFrameworkCore; // use DbContext for EF Core
using EF6 = System.Data.Entity; // use EF6.DbContext for the EF6 version
```

<span data-ttu-id="34aaa-107">Se si deve convertire un'applicazione esistente con più modelli EF, è possibile scegliere di convertirne solo alcuni in EF Core e di continuare a usare EF6 per gli altri.</span><span class="sxs-lookup"><span data-stu-id="34aaa-107">If you are porting an existing application that has multiple EF models, you can choose to selectively port some of them to EF Core, and continue using EF6 for the others.</span></span>
