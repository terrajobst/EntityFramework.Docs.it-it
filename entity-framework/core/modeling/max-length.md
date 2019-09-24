---
title: Lunghezza massima-EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: c39c5d43-018d-48b8-94f2-b8bc7c686c69
uid: core/modeling/max-length
ms.openlocfilehash: b6f0594fed0c491b4f79dcda5273cdebe9ecf35f
ms.sourcegitcommit: ec196918691f50cd0b21693515b0549f06d9f39c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/23/2019
ms.locfileid: "71197219"
---
# <a name="maximum-length"></a>Lunghezza massima

La configurazione di una lunghezza massima fornisce un hint all'archivio dati sul tipo di dati appropriato da usare per una determinata proprietà. La `string` lunghezza massima si applica solo ai tipi di dati della matrice `byte[]`, ad esempio e.

> [!NOTE]  
> Entity Framework non esegue alcuna convalida della lunghezza massima prima di passare i dati al provider. È necessario che il provider o l'archivio dati venga convalidato se appropriato. Se, ad esempio, la destinazione è SQL Server, il superamento della lunghezza massima provocherà un'eccezione poiché il tipo di dati della colonna sottostante non consentirà l'archiviazione di dati in eccesso.

## <a name="conventions"></a>Convenzioni

Per convenzione, viene lasciata al provider di database per scegliere un tipo di dati appropriato per le proprietà. Per le proprietà con lunghezza, il provider di database sceglierà in genere un tipo di dati che consente la lunghezza dei dati più lunga. Ad esempio, Microsoft SQL Server `nvarchar(max)` utilizzerà per `string` le proprietà (o `nvarchar(450)` se la colonna viene utilizzata come chiave).

## <a name="data-annotations"></a>Annotazioni dei dati

È possibile utilizzare le annotazioni dei dati per configurare una lunghezza massima per una proprietà. In questo esempio, la destinazione SQL Server questa operazione comporterebbe `nvarchar(500)` l'utilizzo del tipo di dati.

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/MaxLength.cs?highlight=14)]

## <a name="fluent-api"></a>API Fluent

È possibile usare l'API Fluent per configurare una lunghezza massima per una proprietà. In questo esempio, la destinazione SQL Server questa operazione comporterebbe `nvarchar(500)` l'utilizzo del tipo di dati.

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/MaxLength.cs?highlight=11-13)]
