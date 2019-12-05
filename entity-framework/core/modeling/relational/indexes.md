---
title: Indici (database relazionale)-EF Core
author: AndriySvyryd
ms.author: ansvyryd
ms.date: 11/05/2019
uid: core/modeling/relational/indexes
ms.openlocfilehash: e14615275f85ee9b6b32d080905465d33963feca
ms.sourcegitcommit: 7a709ce4f77134782393aa802df5ab2718714479
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 12/04/2019
ms.locfileid: "74824568"
---
# <a name="indexes-relational-database"></a>Indici (database relazionale)

> [!NOTE]  
> La configurazione di questa sezione è applicabile in generale ai database relazionali. I metodi di estensione descritti diventano disponibili quando si installa un provider di database relazionali (a causa del pacchetto *Microsoft.EntityFrameworkCore.Relational* condiviso).

Un indice in un database relazionale è mappato allo stesso concetto di indice nella parte centrale di Entity Framework.

## <a name="conventions"></a>Convenzioni

Per convenzione, gli indici sono denominati `IX_<type name>_<property name>`. Per gli indici composti `<property name>` diventa un elenco di nomi di proprietà separato da un carattere di sottolineatura.

## <a name="data-annotations"></a>Annotazioni dei dati

Non è possibile configurare gli indici utilizzando le annotazioni dei dati.

## <a name="fluent-api"></a>API Fluent

È possibile usare l'API Fluent per configurare il nome di un indice.

[!code-csharp[Main](../../../../samples/core/Modeling/FluentAPI/Relational/IndexName.cs?name=Model&highlight=9)]

È anche possibile specificare un filtro.

[!code-csharp[Main](../../../../samples/core/Modeling/FluentAPI/Relational/IndexFilter.cs?name=Model&highlight=9)]

Quando si usa il provider di SQL Server EF aggiunge un filtro `'IS NOT NULL'` per tutte le colonne nullable che fanno parte di un indice univoco. Per eseguire l'override di questa convenzione è possibile specificare un valore `null`.

[!code-csharp[Main](../../../../samples/core/Modeling/FluentAPI/Relational/IndexNoFilter.cs?name=Model&highlight=10)]

### <a name="include-columns-in-sql-server-indexes"></a>Includi colonne negli indici SQL Server

È possibile configurare gli [indici con colonne incluse](https://docs.microsoft.com/sql/relational-databases/indexes/create-indexes-with-included-columns) per migliorare significativamente le prestazioni delle query quando tutte le colonne della query sono incluse nell'indice come colonne chiave o non chiave.

[!code-csharp[Main](../../../../samples/core/Modeling/FluentAPI/Relational/IndexInclude.cs?name=Model)]
