---
title: Valori predefiniti-EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: e541366a-130f-47dd-9997-1b110a11febe
uid: core/modeling/relational/default-values
ms.openlocfilehash: b6ac283d551e2c6ee119f7de6933363b5d8793a1
ms.sourcegitcommit: 18ab4c349473d94b15b4ca977df12147db07b77f
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/06/2019
ms.locfileid: "73655908"
---
# <a name="default-values"></a>Valori predefiniti

> [!NOTE]  
> La configurazione di questa sezione è applicabile in generale ai database relazionali. I metodi di estensione descritti diventano disponibili quando si installa un provider di database relazionali (a causa del pacchetto *Microsoft.EntityFrameworkCore.Relational* condiviso).

Il valore predefinito di una colonna è il valore che verrà inserito se viene inserita una nuova riga, ma non viene specificato alcun valore per la colonna.

## <a name="conventions"></a>Convenzioni

Per convenzione, un valore predefinito non è configurato.

## <a name="data-annotations"></a>Annotazioni dei dati

Non è possibile impostare un valore predefinito utilizzando le annotazioni dei dati.

## <a name="fluent-api"></a>API Fluent

È possibile usare l'API Fluent per specificare il valore predefinito per una proprietà.

[!code-csharp[Main](../../../../samples/core/Modeling/FluentAPI/Relational/DefaultValue.cs?name=DefaultValue&highlight=9)]

È inoltre possibile specificare un frammento SQL utilizzato per calcolare il valore predefinito.

[!code-csharp[Main](../../../../samples/core/Modeling/FluentAPI/Relational/DefaultValueSql.cs?name=DefaultValueSql&highlight=9)]
