---
title: Tipi di dati-EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 9d2e647f-29e4-483b-af00-74269eb06e8f
uid: core/modeling/relational/data-types
ms.openlocfilehash: d667cbcb821e321faed36d097b531c7c55b81248
ms.sourcegitcommit: cbaa6cc89bd71d5e0bcc891e55743f0e8ea3393b
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/20/2019
ms.locfileid: "71149161"
---
# <a name="data-types"></a>Tipi di dati

> [!NOTE]  
> La configurazione di questa sezione è applicabile in generale ai database relazionali. I metodi di estensione descritti diventano disponibili quando si installa un provider di database relazionali (a causa del pacchetto *Microsoft.EntityFrameworkCore.Relational* condiviso).

Il tipo di dati fa riferimento al tipo specifico del database della colonna a cui viene mappata una proprietà.

## <a name="conventions"></a>Convenzioni

Per convenzione, il provider di database seleziona un tipo di dati basato sul tipo .NET della proprietà. Prende in considerazione anche altri metadati, ad esempio la [lunghezza massima](../max-length.md)configurata, se la proprietà fa parte di una chiave primaria e così via.

Ad esempio, SQL Server usa `datetime2(7)` per `DateTime` le proprietà e `nvarchar(max)` per `string` le proprietà ( `nvarchar(450)` o `string` per le proprietà usate come chiave).

## <a name="data-annotations"></a>Annotazioni dei dati

È possibile utilizzare le annotazioni dei dati per specificare un tipo di dati esatto per una colonna.

`Url` Il codice seguente, ad esempio, consente di configurare come stringa non Unicode con `Rating` `200` lunghezza massima e come decimale con `5` precisione e scala `2`di.

[!code-csharp[Main](../../../../samples/core/Modeling/DataAnnotations/Samples/Relational/DataType.cs?name=Entities&highlight=4,6)]

## <a name="fluent-api"></a>API Fluent

È anche possibile usare l'API Fluent per specificare gli stessi tipi di dati per le colonne.

[!code-csharp[Main](../../../../samples/core/Modeling/FluentAPI/Samples/Relational/DataType.cs?name=Model&highlight=9-10)]
