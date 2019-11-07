---
title: Colonne calcolate-EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: e9d81f06-805d-45c9-97c2-3546df654829
uid: core/modeling/relational/computed-columns
ms.openlocfilehash: 4e92ed6d785f3b7bdf54015101bdcddb9e4e0e1c
ms.sourcegitcommit: 18ab4c349473d94b15b4ca977df12147db07b77f
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/06/2019
ms.locfileid: "73655924"
---
# <a name="computed-columns"></a>Colonne calcolate

> [!NOTE]  
> La configurazione di questa sezione è applicabile in generale ai database relazionali. I metodi di estensione descritti diventano disponibili quando si installa un provider di database relazionali (a causa del pacchetto *Microsoft.EntityFrameworkCore.Relational* condiviso).

Una colonna calcolata è una colonna il cui valore viene calcolato nel database. Per calcolare il valore di una colonna calcolata, è possibile utilizzare altre colonne della tabella.

## <a name="conventions"></a>Convenzioni

Per convenzione, le colonne calcolate non vengono create nel modello.

## <a name="data-annotations"></a>Annotazioni dei dati

Le colonne calcolate non possono essere configurate con le annotazioni dei dati.

## <a name="fluent-api"></a>API Fluent

È possibile utilizzare l'API Fluent per specificare che una proprietà deve essere mappata a una colonna calcolata.

[!code-csharp[Main](../../../../samples/core/Modeling/FluentAPI/Relational/ComputedColumn.cs?name=ComputedColumn&highlight=9)]
