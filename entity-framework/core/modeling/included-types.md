---
title: Inclusione & esclusi i tipi-EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: cbe6935e-2679-4b77-8914-a8d772240cf1
uid: core/modeling/included-types
ms.openlocfilehash: 1249e71c02e58afe7fe06b3fdcf523dfa0c9b17c
ms.sourcegitcommit: 18ab4c349473d94b15b4ca977df12147db07b77f
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/06/2019
ms.locfileid: "73655731"
---
# <a name="including--excluding-types"></a>Inclusione ed esclusione di tipi

L'inclusione di un tipo nel modello significa che EF dispone di metadati relativi a tale tipo e tenterà di leggere e scrivere istanze da/nel database.

## <a name="conventions"></a>Convenzioni

Per convenzione, i tipi esposti in `DbSet` proprietà del contesto sono inclusi nel modello. Sono inoltre inclusi i tipi indicati nel metodo `OnModelCreating`. Infine, nel modello vengono inclusi anche tutti i tipi individuati in modo ricorsivo per esplorare le proprietà di navigazione dei tipi individuati.

**Ad esempio, nell'elenco di codice seguente vengono individuati tutti e tre i tipi:**

* `Blog` perché è esposto in una proprietà `DbSet` nel contesto

* `Post` perché viene individuato tramite la proprietà di navigazione `Blog.Posts`

* `AuditEntry` perché è indicato in `OnModelCreating`

[!code-csharp[Main](../../../samples/core/Modeling/Conventions/IncludedTypes.cs?name=IncludedTypes&highlight=3,7,16)]

## <a name="data-annotations"></a>Annotazioni dei dati

È possibile utilizzare le annotazioni dei dati per escludere un tipo dal modello.

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/IgnoreType.cs?highlight=20)]

## <a name="fluent-api"></a>API Fluent

È possibile utilizzare l'API Fluent per escludere un tipo dal modello.

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/IgnoreType.cs?highlight=12)]
