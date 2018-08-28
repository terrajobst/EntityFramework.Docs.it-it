---
title: Chiavi alternative (vincoli univoci) - EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 3d419dcf-2b5d-467c-b408-ea03d830721a
uid: core/modeling/relational/unique-constraints
ms.openlocfilehash: 7ec58ee31aac79e15329dc8542f37fd117772fbe
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 08/27/2018
ms.locfileid: "42994191"
---
# <a name="alternate-keys-unique-constraints"></a>Chiavi alternative (vincoli univoci)

> [!NOTE]  
> La configurazione di questa sezione è applicabile in generale ai database relazionali. I metodi di estensione descritti diventano disponibili quando si installa un provider di database relazionali (a causa del pacchetto *Microsoft.EntityFrameworkCore.Relational* condiviso).

Un vincolo unique è stato introdotto per ogni chiave alternativo nel modello.

## <a name="conventions"></a>Convenzioni

Per convenzione, verrà denominati l'indice e vincolo che sono state aggiunte per una chiave alternativa `AK_<type name>_<property name>`. Per le chiavi alternative composte `<property name>` diventa un elenco separato da un carattere di sottolineatura di nomi di proprietà.

## <a name="data-annotations"></a>Annotazioni dei dati

I vincoli UNIQUE non possono essere configurati utilizzando le annotazioni dei dati.

## <a name="fluent-api"></a>API Fluent

Per configurare il nome di indice e vincolo per una chiave alternativa, è possibile utilizzare l'API Fluent.

[!code-csharp[Main](../../../../samples/core/Modeling/FluentAPI/Samples/Relational/AlternateKeyName.cs?name=Model&highlight=9)]
