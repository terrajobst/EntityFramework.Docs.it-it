---
title: EF6 ed EF Core - Uso nella stessa applicazione
author: ajcvickers
ms.date: 01/23/2019
ms.assetid: a06e3c35-110c-4294-a1e2-32d2c31c90a7
uid: efcore-and-ef6/side-by-side
ms.openlocfilehash: bcf0a26535c4ec880a9ac25478c987fb683f6d26
ms.sourcegitcommit: 9b562663679854c37c05fca13d93e180213fb4aa
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/07/2020
ms.locfileid: "78419644"
---
# <a name="using-ef-core-and-ef6-in-the-same-application"></a>Using di EF Core e di EF6 nella stessa applicazione

È possibile usare EF Core ed EF6 nella stessa applicazione o libreria installando entrambi i pacchetti NuGet.It is possible to use EF Core and EF6 in the same application or library by installing both NuGet packages.

Poiché alcuni tipi hanno lo stesso nome in EF Core e in EF6 e si distinguono solo per lo spazio dei nomi, l'uso di EF Core ed EF6 nello stesso file di codice può risultare complicato. L'ambiguità può essere facilmente eliminata usando direttive alias dello spazio dei nomi. Ad esempio:

``` csharp
using Microsoft.EntityFrameworkCore; // use DbContext for EF Core
using EF6 = System.Data.Entity; // use EF6.DbContext for the EF6 version
```

Se si deve convertire un'applicazione esistente con più modelli EF, è possibile scegliere di convertirne solo alcuni in EF Core e di continuare a usare EF6 per gli altri.
