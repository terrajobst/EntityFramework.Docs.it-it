---
title: Tipi di dati - EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 9d2e647f-29e4-483b-af00-74269eb06e8f
uid: core/modeling/relational/data-types
ms.openlocfilehash: 9060f66c752be01090ce40be6bf3a32f348ce571
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 08/27/2018
ms.locfileid: "42993521"
---
# <a name="data-types"></a>Tipi di dati

> [!NOTE]  
> La configurazione di questa sezione è applicabile in generale ai database relazionali. I metodi di estensione descritti diventano disponibili quando si installa un provider di database relazionali (a causa del pacchetto *Microsoft.EntityFrameworkCore.Relational* condiviso).

Tipo di dati fa riferimento al tipo specifico di database della colonna a cui viene mappata una proprietà.

## <a name="conventions"></a>Convenzioni

Per convenzione, il provider di database consente di selezionare un tipo di dati in base al tipo CLR della proprietà. Tiene in considerazione anche altri metadati, ad esempio l'applicazione configurata [lunghezza massima](../max-length.md), se la proprietà fa parte di una chiave primaria e così via.

Ad esempio, SQL Server Usa `datetime2(7)` per `DateTime` delle proprietà, e `nvarchar(max)` per `string` delle proprietà (o `nvarchar(450)` per `string` proprietà che vengono usate come chiave).

## <a name="data-annotations"></a>Annotazioni dei dati

È possibile usare le annotazioni dei dati per specificare un tipo di dati esatto per una colonna.

Ad esempio il codice seguente consente di configurare `Url` sotto forma di stringa non unicode con lunghezza massima pari a `200` e `Rating` come decimale con precisione di `5` e la scalabilità di `2`.

[!code-csharp[Main](../../../../samples/core/Modeling/DataAnnotations/Samples/Relational/DataType.cs?name=Entities&highlight=4,6)]

## <a name="fluent-api"></a>API Fluent

È anche possibile usare l'API Fluent per specificare gli stessi tipi di dati per le colonne.

[!code-csharp[Main](../../../../samples/core/Modeling/FluentAPI/Samples/Relational/DataType.cs?name=Model&highlight=9-10)]
