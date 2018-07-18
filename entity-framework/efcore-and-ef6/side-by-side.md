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
# <a name="using-ef-core-and-ef6-in-the-same-application"></a>Using di EF Core e di EF6 nella stessa applicazione

È possibile usare EF Core ed EF6 nella stessa applicazione o libreria .NET Framework installando entrambi i pacchetti NuGet.

Poiché alcuni tipi hanno lo stesso nome in EF Core e in EF6 e si distinguono solo per lo spazio dei nomi, l'uso di EF Core ed EF6 nello stesso file di codice può risultare complicato. L'ambiguità può essere facilmente eliminata usando direttive alias dello spazio dei nomi. Ad esempio:

``` csharp
using Microsoft.EntityFrameworkCore; // use DbContext for EF Core
using EF6 = System.Data.Entity; // use EF6.DbContext for the EF6 version
```

Se si deve convertire un'applicazione esistente con più modelli EF, è possibile scegliere di convertirne solo alcuni in EF Core e di continuare a usare EF6 per gli altri.
