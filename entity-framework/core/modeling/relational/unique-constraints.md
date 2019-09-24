---
title: Chiavi alternative (vincoli UNIQUE)-EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 3d419dcf-2b5d-467c-b408-ea03d830721a
uid: core/modeling/relational/unique-constraints
ms.openlocfilehash: 7afcb804aeeccbd5e07c228a8fd9850ca00a2919
ms.sourcegitcommit: ec196918691f50cd0b21693515b0549f06d9f39c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/23/2019
ms.locfileid: "71197610"
---
# <a name="alternate-keys-unique-constraints"></a>Chiavi alternative (vincoli UNIQUE)

> [!NOTE]  
> La configurazione di questa sezione è applicabile in generale ai database relazionali. I metodi di estensione descritti diventano disponibili quando si installa un provider di database relazionali (a causa del pacchetto *Microsoft.EntityFrameworkCore.Relational* condiviso).

Un vincolo UNIQUE viene introdotto per ogni chiave alternativa nel modello.

## <a name="conventions"></a>Convenzioni

Per convenzione, l'indice e il vincolo introdotti per una chiave alternativa verranno denominati `AK_<type name>_<property name>`. Per chiavi `<property name>` alternative composite diventa un elenco di nomi di proprietà separati da un carattere di sottolineatura.

## <a name="data-annotations"></a>Annotazioni dei dati

Non è possibile configurare vincoli UNIQUE usando le annotazioni dei dati.

## <a name="fluent-api"></a>API Fluent

È possibile usare l'API Fluent per configurare il nome di indice e vincolo per una chiave alternativa.

[!code-csharp[Main](../../../../samples/core/Modeling/FluentAPI/Relational/AlternateKeyName.cs?name=Model&highlight=9)]
