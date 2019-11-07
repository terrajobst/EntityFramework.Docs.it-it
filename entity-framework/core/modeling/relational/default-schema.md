---
title: Schema predefinito-EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: e6e58473-9f5e-4a1f-ac0f-b87d2cbb667e
uid: core/modeling/relational/default-schema
ms.openlocfilehash: 1579fed007997aa4cf49b4c1290aee86c81c0000
ms.sourcegitcommit: 18ab4c349473d94b15b4ca977df12147db07b77f
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/06/2019
ms.locfileid: "73655975"
---
# <a name="default-schema"></a>Schema predefinito

> [!NOTE]  
> La configurazione di questa sezione è applicabile in generale ai database relazionali. I metodi di estensione descritti diventano disponibili quando si installa un provider di database relazionali (a causa del pacchetto *Microsoft.EntityFrameworkCore.Relational* condiviso).

Lo schema predefinito è lo schema del database in cui verranno creati gli oggetti se uno schema non è configurato in modo esplicito per tale oggetto.

## <a name="conventions"></a>Convenzioni

Per convenzione, il provider di database sceglierà lo schema predefinito più appropriato. Ad esempio, Microsoft SQL Server utilizzerà lo schema `dbo` e SQLite non utilizzerà uno schema (poiché gli schemi non sono supportati in SQLite).

## <a name="data-annotations"></a>Annotazioni dei dati

Non è possibile impostare lo schema predefinito utilizzando le annotazioni dei dati.

## <a name="fluent-api"></a>API Fluent

Per specificare uno schema predefinito, è possibile usare l'API Fluent.

[!code-csharp[Main](../../../../samples/core/Modeling/FluentAPI/Relational/DefaultSchema.cs?name=DefaultSchema&highlight=7)]
