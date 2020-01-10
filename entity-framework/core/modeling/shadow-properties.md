---
title: Proprietà Shadow-EF Core
author: AndriySvyryd
ms.date: 01/03/2020
ms.assetid: 75369266-d2b9-4416-b118-ed238f81f599
uid: core/modeling/shadow-properties
ms.openlocfilehash: 229cfd83f75b01dff9ac9ad30ee55c7cc727c19e
ms.sourcegitcommit: 4e86f01740e407ff25e704a11b1f7d7e66bfb2a6
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/09/2020
ms.locfileid: "75781183"
---
# <a name="shadow-properties"></a>Proprietà shadow

Le proprietà shadow sono proprietà che non sono definite nella classe di entità .NET, ma sono definite per il tipo di entità nel modello di EF Core. Il valore e lo stato di queste proprietà vengono mantenuti esclusivamente nello strumento di rilevamento delle modifiche. Le proprietà shadow sono utili quando nel database sono presenti dati che non devono essere esposti sui tipi di entità mappati.

## <a name="foreign-key-shadow-properties"></a>Proprietà Shadow chiave esterna

Le proprietà shadow vengono spesso utilizzate per le proprietà di chiave esterna, in cui la relazione tra due entità è rappresentata da un valore di chiave esterna nel database, ma la relazione viene gestita sui tipi di entità utilizzando le proprietà di navigazione tra l'entità tipi. Per convenzione, EF introdurrà una proprietà shadow quando viene individuata una relazione, ma non viene trovata alcuna proprietà di chiave esterna nella classe di entità dipendente.

La proprietà verrà denominata `<navigation property name><principal key property name>` (la navigazione sull'entità dipendente, che fa riferimento all'entità principale, viene utilizzata per la denominazione). Se il nome della proprietà chiave principale include il nome della proprietà di navigazione, il nome sarà appena `<principal key property name>`. Se non è presente alcuna proprietà di navigazione nell'entità dipendente, il nome del tipo di entità viene usato al suo posto.

Il seguente listato di codice, ad esempio, comporterà l'introduzione di una proprietà shadow `BlogId` all'entità `Post`:

[!code-csharp[Main](../../../samples/core/Modeling/Conventions/ShadowForeignKey.cs?name=Conventions&highlight=21-23)]

## <a name="configuring-shadow-properties"></a>Configurazione delle proprietà shadow

Per configurare le proprietà shadow, è possibile usare l'API Fluent. Una volta chiamato l'overload di stringa di `Property`, è possibile concatenare qualsiasi chiamata di configurazione per altre proprietà. Nell'esempio seguente, poiché `Blog` non dispone di una proprietà CLR denominata `LastUpdated`, viene creata una proprietà shadow:

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/ShadowProperty.cs?name=ShadowProperty&highlight=8)]

Se il nome fornito al metodo `Property` corrisponde al nome di una proprietà esistente (una proprietà shadow o un oggetto definito nella classe di entità), il codice configurerà la proprietà esistente anziché introdurre una nuova proprietà shadow.

## <a name="accessing-shadow-properties"></a>Accesso alle proprietà shadow

I valori delle proprietà shadow possono essere ottenuti e modificati tramite l'API `ChangeTracker`:

``` csharp
context.Entry(myBlog).Property("LastUpdated").CurrentValue = DateTime.Now;
```

È possibile fare riferimento alle proprietà shadow nelle query LINQ tramite il metodo statico `EF.Property`:

``` csharp
var blogs = context.Blogs
    .OrderBy(b => EF.Property<DateTime>(b, "LastUpdated"));
```
