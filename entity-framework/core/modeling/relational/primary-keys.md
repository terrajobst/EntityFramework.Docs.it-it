---
title: Chiavi primarie-EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: c78f8f42-564a-45a4-aca7-3ede9f7ed2bc
uid: core/modeling/relational/primary-keys
ms.openlocfilehash: c195cc37859ea0689b5c6fbb84051564cf96c83a
ms.sourcegitcommit: 18ab4c349473d94b15b4ca977df12147db07b77f
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/06/2019
ms.locfileid: "73656046"
---
# <a name="primary-keys"></a>Chiavi primarie

> [!NOTE]  
> La configurazione di questa sezione è applicabile in generale ai database relazionali. I metodi di estensione descritti diventano disponibili quando si installa un provider di database relazionali (a causa del pacchetto *Microsoft.EntityFrameworkCore.Relational* condiviso).

Per la chiave di ogni tipo di entità è stato introdotto un vincolo PRIMARY KEY.

## <a name="conventions"></a>Convenzioni

Per convenzione, la chiave primaria nel database verrà denominata `PK_<type name>`.

## <a name="data-annotations"></a>Annotazioni dei dati

Non è possibile configurare aspetti specifici di un database relazionale di una chiave primaria usando le annotazioni dei dati.

## <a name="fluent-api"></a>API Fluent

È possibile utilizzare l'API Fluent per configurare il nome del vincolo PRIMARY KEY nel database.

[!code-csharp[Main](../../../../samples/core/Modeling/FluentAPI/Relational/KeyName.cs?name=KeyName&highlight=9)]
