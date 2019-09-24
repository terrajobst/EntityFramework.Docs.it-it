---
title: Mapping colonne-EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 05a47de9-1078-488e-a823-b516a4208f33
uid: core/modeling/relational/columns
ms.openlocfilehash: eaffc0cc1642f64edabeeef974f5f6de7a23b656
ms.sourcegitcommit: ec196918691f50cd0b21693515b0549f06d9f39c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/23/2019
ms.locfileid: "71197207"
---
# <a name="column-mapping"></a>Mapping colonne

> [!NOTE]  
> La configurazione di questa sezione è applicabile in generale ai database relazionali. I metodi di estensione descritti diventano disponibili quando si installa un provider di database relazionali (a causa del pacchetto *Microsoft.EntityFrameworkCore.Relational* condiviso).

Il mapping delle colonne consente di identificare i dati della colonna da cui eseguire le query e salvarli nel database.

## <a name="conventions"></a>Convenzioni

Per convenzione, ogni proprietà viene configurata per eseguire il mapping a una colonna con lo stesso nome della proprietà.

## <a name="data-annotations"></a>Annotazioni dei dati

È possibile utilizzare le annotazioni dei dati per configurare la colonna a cui viene eseguito il mapping di una proprietà.

[!code-csharp[Main](../../../../samples/core/Modeling/DataAnnotations/Relational/Column.cs?highlight=13)]

## <a name="fluent-api"></a>API Fluent

È possibile usare l'API Fluent per configurare la colonna a cui viene eseguito il mapping di una proprietà.

[!code-csharp[Main](../../../../samples/core/Modeling/FluentAPI/Relational/Column.cs?highlight=11-13)]
