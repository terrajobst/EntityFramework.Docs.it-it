---
title: Tipi di dati - Core a Entity Framework
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: 9d2e647f-29e4-483b-af00-74269eb06e8f
ms.technology: entity-framework-core
uid: core/modeling/relational/data-types
ms.openlocfilehash: fd4668a3f9554eb9d3b1161d5dddce2fcdcac712
ms.sourcegitcommit: 860ec5d047342fbc4063a0de881c9861cc1f8813
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/05/2017
ms.locfileid: "26053501"
---
# <a name="data-types"></a>Riepilogo dei tipi di dati

> [!NOTE]  
> La configurazione di questa sezione è applicabile a database relazionali in generale. I metodi di estensione qui verranno rese disponibili quando si installa un provider di database relazionali (a causa di condiviso *Microsoft.EntityFrameworkCore.Relational* pacchetto).

Tipo di dati fa riferimento al tipo specifico di database della colonna a cui viene eseguito il mapping di una proprietà.

## <a name="conventions"></a>Convenzioni

Per convenzione, il provider di database seleziona un tipo di dati in base al tipo CLR della proprietà. Tiene in considerazione anche altri metadati, ad esempio l'applicazione configurata [lunghezza massima](../max-length.md), se la proprietà fa parte di una chiave primaria e così via.

Ad esempio, SQL Server utilizza `datetime2(7)` per `DateTime` , proprietà e `nvarchar(max)` per `string` proprietà (o `nvarchar(450)` per `string` proprietà utilizzate come chiave).

## <a name="data-annotations"></a>Annotazioni dei dati

È possibile utilizzare le annotazioni dei dati per specificare un tipo di dati esatto per una colonna.

Ad esempio il codice seguente consente di configurare `Url` sotto forma di stringa non unicode con lunghezza massima di `200` e `Rating` come decimale con precisione di `5` e scalabilità di `2`.

[!code-csharp[Main](../../../../samples/core/Modeling/DataAnnotations/Samples/Relational/DataType.cs?name=Entities&highlight=4,6)]

## <a name="fluent-api"></a>Microsoft Office Fluent API

È inoltre possibile utilizzare l'API Fluent per specificare gli stessi tipi di dati per le colonne.

[!code-csharp[Main](../../../../samples/core/Modeling/FluentAPI/Samples/Relational/DataType.cs?name=Model&highlight=9-10)]
