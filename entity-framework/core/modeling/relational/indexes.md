---
title: Indici - Core a Entity Framework
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: 4581e7ba-5e7f-452c-9937-0aaf790ba10a
ms.technology: entity-framework-core
uid: core/modeling/relational/indexes
ms.openlocfilehash: f577fccfefc6908edf2ac47ae630323d7a9f5f2b
ms.sourcegitcommit: b2d94cebdc32edad4fecb07e53fece66437d1b04
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/28/2018
---
# <a name="indexes"></a>Indici

> [!NOTE]  
> La configurazione di questa sezione è applicabile in generale ai database relazionali. I metodi di estensione descritti diventano disponibili quando si installa un provider di database relazionali (a causa del pacchetto *Microsoft.EntityFrameworkCore.Relational* condiviso).

Esegue il mapping di un indice in un database relazionale per lo stesso concetto di un indice in base di Entity Framework.

## <a name="conventions"></a>Convenzioni

Per convenzione, gli indici sono denominati `IX_<type name>_<property name>`. Per gli indici composti `<property name>` diventa un elenco separato da un carattere di sottolineatura di nomi di proprietà.

## <a name="data-annotations"></a>Annotazioni dei dati

Gli indici non possono essere configurati utilizzando le annotazioni dei dati.

## <a name="fluent-api"></a>API Fluent

Per configurare il nome di un indice, è possibile utilizzare l'API Fluent.

[!code-csharp[Main](../../../../samples/core/Modeling/FluentAPI/Samples/Relational/IndexName.cs?name=Model&highlight=9)]

È inoltre possibile specificare un filtro.

[!code-csharp[Main](../../../../samples/core/Modeling/FluentAPI/Samples/Relational/IndexFilter.cs?name=Model&highlight=9)]

Quando si utilizza il provider di SQL Server EF aggiunge filtrare un 'IS NOT NULL' per tutte le colonne che ammettono valori null che fanno parte di un indice univoco. Per eseguire l'override di questa convenzione è possibile fornire un `null` valore.

[!code-csharp[Main](../../../../samples/core/Modeling/FluentAPI/Samples/Relational/IndexNoFilter.cs?name=Model&highlight=10)]
