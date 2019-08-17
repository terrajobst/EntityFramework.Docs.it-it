---
title: Indici-EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 4581e7ba-5e7f-452c-9937-0aaf790ba10a
uid: core/modeling/relational/indexes
ms.openlocfilehash: dfada7446f812f3c277572cc1338441272e8f448
ms.sourcegitcommit: 7b7f774a5966b20d2aed5435a672a1edbe73b6fb
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 08/17/2019
ms.locfileid: "69565363"
---
# <a name="indexes"></a>Indexes

> [!NOTE]  
> La configurazione di questa sezione è applicabile in generale ai database relazionali. I metodi di estensione descritti diventano disponibili quando si installa un provider di database relazionali (a causa del pacchetto *Microsoft.EntityFrameworkCore.Relational* condiviso).

Un indice in un database relazionale è mappato allo stesso concetto di indice nella parte centrale di Entity Framework.

## <a name="conventions"></a>Convenzioni

Per convenzione, gli indici `IX_<type name>_<property name>`vengono denominati. Per gli indici `<property name>` composti diventa un elenco di nomi di proprietà separati da un carattere di sottolineatura.

## <a name="data-annotations"></a>Annotazioni dei dati

Non è possibile configurare gli indici utilizzando le annotazioni dei dati.

## <a name="fluent-api"></a>API Fluent

È possibile usare l'API Fluent per configurare il nome di un indice.

[!code-csharp[Main](../../../../samples/core/Modeling/FluentAPI/Samples/Relational/IndexName.cs?name=Model&highlight=9)]

È anche possibile specificare un filtro.

[!code-csharp[Main](../../../../samples/core/Modeling/FluentAPI/Samples/Relational/IndexFilter.cs?name=Model&highlight=9)]

Quando si usa il provider di SQL Server EF aggiunge un filtro ' IS NOT NULL ' per tutte le colonne nullable che fanno parte di un indice univoco. Per eseguire l'override di questa convenzione, `null` è possibile specificare un valore.

[!code-csharp[Main](../../../../samples/core/Modeling/FluentAPI/Samples/Relational/IndexNoFilter.cs?name=Model&highlight=10)]

### <a name="include-columns-in-sql-server-indexes"></a>Includi colonne negli indici SQL Server

È possibile configurare gli [indici con colonne incluse](https://docs.microsoft.com/sql/relational-databases/indexes/create-indexes-with-included-columns) per migliorare significativamente le prestazioni delle query quando tutte le colonne della query sono incluse nell'indice come colonne chiave o non chiave.

[!code-csharp[Main](../../../../samples/core/Modeling/FluentAPI/Samples/Relational/ForSqlServerHasIndex.cs?name=Model)]
