---
title: Proprietà Shadow-EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 75369266-d2b9-4416-b118-ed238f81f599
uid: core/modeling/shadow-properties
ms.openlocfilehash: ab57358dd247e32c4ca0f57d07b4cb98f2b85d29
ms.sourcegitcommit: 18ab4c349473d94b15b4ca977df12147db07b77f
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/06/2019
ms.locfileid: "73655960"
---
# <a name="shadow-properties"></a>Proprietà shadow

Le proprietà shadow sono proprietà che non sono definite nella classe di entità .NET, ma sono definite per il tipo di entità nel modello di EF Core. Il valore e lo stato di queste proprietà vengono mantenuti esclusivamente nello strumento di rilevamento delle modifiche.

Le proprietà shadow sono utili quando nel database sono presenti dati che non devono essere esposti sui tipi di entità mappati. Vengono spesso usate per le proprietà di chiave esterna, in cui la relazione tra due entità è rappresentata da un valore di chiave esterna nel database, ma la relazione viene gestita sui tipi di entità usando le proprietà di navigazione tra i tipi di entità.

I valori delle proprietà shadow possono essere ottenuti e modificati tramite l'API `ChangeTracker`.

``` csharp
context.Entry(myBlog).Property("LastUpdated").CurrentValue = DateTime.Now;
```

È possibile fare riferimento alle proprietà shadow nelle query LINQ tramite il metodo statico `EF.Property`.

``` csharp
var blogs = context.Blogs
    .OrderBy(b => EF.Property<DateTime>(b, "LastUpdated"));
```

## <a name="conventions"></a>Convenzioni

Le proprietà shadow possono essere create per convenzione quando viene individuata una relazione, ma nella classe di entità dipendente non viene trovata alcuna proprietà di chiave esterna. In questo caso verrà introdotta una proprietà di chiave esterna Shadow. La proprietà della chiave esterna shadow verrà denominata `<navigation property name><principal key property name>` (la navigazione sull'entità dipendente, che fa riferimento all'entità principale, viene utilizzata per la denominazione). Se il nome della proprietà chiave principale include il nome della proprietà di navigazione, il nome sarà appena `<principal key property name>`. Se non è presente alcuna proprietà di navigazione nell'entità dipendente, il nome del tipo di entità viene usato al suo posto.

Il seguente listato di codice, ad esempio, comporterà l'introduzione di una proprietà shadow `BlogId` all'entità `Post`.

[!code-csharp[Main](../../../samples/core/Modeling/Conventions/ShadowForeignKey.cs?name=Conventions)]

## <a name="data-annotations"></a>Annotazioni dei dati

Non è possibile creare le proprietà shadow con le annotazioni dei dati.

## <a name="fluent-api"></a>API Fluent

Per configurare le proprietà shadow, è possibile usare l'API Fluent. Una volta chiamato l'overload di stringa di `Property` è possibile concatenare qualsiasi chiamata di configurazione per altre proprietà.

Se il nome fornito al metodo `Property` corrisponde al nome di una proprietà esistente (una proprietà shadow o un oggetto definito nella classe di entità), il codice configurerà la proprietà esistente anziché introdurre una nuova proprietà shadow.

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/ShadowProperty.cs?name=ShadowProperty&highlight=8)]
