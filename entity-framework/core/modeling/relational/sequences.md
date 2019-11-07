---
title: Sequenze-EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 94f81a92-3c72-4e14-912a-f99310374e42
uid: core/modeling/relational/sequences
ms.openlocfilehash: b810caaffa329bb5ad6f3486145d0ade9287eada
ms.sourcegitcommit: 18ab4c349473d94b15b4ca977df12147db07b77f
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/06/2019
ms.locfileid: "73656119"
---
# <a name="sequences"></a>Sequenze

> [!NOTE]  
> La configurazione di questa sezione è applicabile in generale ai database relazionali. I metodi di estensione descritti diventano disponibili quando si installa un provider di database relazionali (a causa del pacchetto *Microsoft.EntityFrameworkCore.Relational* condiviso).

Una sequenza genera valori numerici sequenziali nel database. Le sequenze non sono associate a una tabella specifica.

## <a name="conventions"></a>Convenzioni

Per convenzione, le sequenze non vengono introdotte nel modello.

## <a name="data-annotations"></a>Annotazioni dei dati

Non è possibile configurare una sequenza usando le annotazioni dei dati.

## <a name="fluent-api"></a>API Fluent

È possibile utilizzare l'API Fluent per creare una sequenza nel modello.

[!code-csharp[Main](../../../../samples/core/Modeling/FluentAPI/Relational/Sequence.cs?name=Model&highlight=7)]

È inoltre possibile configurare un aspetto aggiuntivo della sequenza, ad esempio il relativo schema, il valore iniziale e l'incremento.

[!code-csharp[Main](../../../../samples/core/Modeling/FluentAPI/Relational/SequenceConfigured.cs?name=Sequence&highlight=7,8,9)]

Una volta introdotta una sequenza, è possibile utilizzarla per generare valori per le proprietà nel modello. Ad esempio, è possibile usare i [valori predefiniti](default-values.md) per inserire il valore successivo dalla sequenza.

[!code-csharp[Main](../../../../samples/core/Modeling/FluentAPI/Relational/SequenceUsed.cs?name=Default&highlight=13)]
