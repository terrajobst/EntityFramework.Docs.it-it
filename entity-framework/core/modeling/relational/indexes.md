---
title: Indici - EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 4581e7ba-5e7f-452c-9937-0aaf790ba10a
uid: core/modeling/relational/indexes
ms.openlocfilehash: 605b30ce710d9034deab97f695496ec66a576565
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 08/27/2018
ms.locfileid: "42993217"
---
# <a name="indexes"></a>Indici

> [!NOTE]  
> La configurazione di questa sezione è applicabile in generale ai database relazionali. I metodi di estensione descritti diventano disponibili quando si installa un provider di database relazionali (a causa del pacchetto *Microsoft.EntityFrameworkCore.Relational* condiviso).

Un indice in un database relazionale viene eseguito il mapping allo stesso concetto come indice nel nucleo di Entity Framework.

## <a name="conventions"></a>Convenzioni

Per convenzione, gli indici sono denominati `IX_<type name>_<property name>`. Per gli indici composti `<property name>` diventa un elenco separato da un carattere di sottolineatura di nomi di proprietà.

## <a name="data-annotations"></a>Annotazioni dei dati

Gli indici non possono essere configurati utilizzando le annotazioni dei dati.

## <a name="fluent-api"></a>API Fluent

È possibile usare l'API Fluent per configurare il nome di un indice.

[!code-csharp[Main](../../../../samples/core/Modeling/FluentAPI/Samples/Relational/IndexName.cs?name=Model&highlight=9)]

È anche possibile specificare un filtro.

[!code-csharp[Main](../../../../samples/core/Modeling/FluentAPI/Samples/Relational/IndexFilter.cs?name=Model&highlight=9)]

Quando si utilizza il provider di SQL Server EF aggiunge filtrare un 'IS NOT NULL' per tutte le colonne nullable che fanno parte di un indice univoco. Eseguire l'override di questa convenzione è possibile fornire un `null` valore.

[!code-csharp[Main](../../../../samples/core/Modeling/FluentAPI/Samples/Relational/IndexNoFilter.cs?name=Model&highlight=10)]
