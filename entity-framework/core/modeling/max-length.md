---
title: Lunghezza massima - EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: c39c5d43-018d-48b8-94f2-b8bc7c686c69
uid: core/modeling/max-length
ms.openlocfilehash: 3220518cb0a409b6e802d2f3a98acdb949ffbf56
ms.sourcegitcommit: 87fcaba46535aa351db4bdb1231bd14b40e459b9
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/22/2019
ms.locfileid: "59929849"
---
# <a name="maximum-length"></a>Lunghezza massima

Configurazione di una lunghezza massima fornisce un hint all'archivio dati sul tipo di dati appropriato da utilizzare per una determinata proprietà. Lunghezza massima si applica solo ai tipi di dati di matrice, ad esempio `string` e `byte[]`.

> [!NOTE]  
> Entity Framework non esegue alcuna convalida della lunghezza massima prima di passare dati al provider. È responsabilità dell'archivio di provider o i dati da convalidare se appropriato. Ad esempio, quando come destinazione di SQL Server, che supera la lunghezza massima genererà un'eccezione come tipo di dati della colonna sottostante non consentirà di archiviare i dati in eccesso.

## <a name="conventions"></a>Convenzioni

Per convenzione, rimarrà stabilite dal provider di database per scegliere un tipo di dati appropriato per le proprietà. Per le proprietà che hanno una lunghezza, il provider di database sceglierà in genere un tipo di dati che consente la lunghezza massima di dati. Ad esempio, Microsoft SQL Server userà `nvarchar(max)` per `string` delle proprietà (o `nvarchar(450)` se la colonna viene utilizzata come chiave).

## <a name="data-annotations"></a>Annotazioni dei dati

È possibile usare le annotazioni dei dati per configurare una lunghezza massima per una proprietà. In questo esempio, destinazione SQL Server potrebbe causare il `nvarchar(500)` tipo di dati in uso.

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/Samples/MaxLength.cs?highlight=14)]

## <a name="fluent-api"></a>API Fluent

È possibile usare l'API Fluent per configurare una lunghezza massima per una proprietà. In questo esempio, destinazione SQL Server potrebbe causare il `nvarchar(500)` tipo di dati in uso.

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Samples/MaxLength.cs?highlight=11-13)]
