---
title: Chiavi alternative (vincoli univoci) - Core a Entity Framework
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: 3d419dcf-2b5d-467c-b408-ea03d830721a
ms.technology: entity-framework-core
uid: core/modeling/relational/unique-constraints
ms.openlocfilehash: 1b7e2bef6ede95f8c27211ba00dcc6b97cccde9b
ms.sourcegitcommit: 01a75cd483c1943ddd6f82af971f07abde20912e
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 10/27/2017
---
# <a name="alternate-keys-unique-constraints"></a>Chiavi alternative (vincoli univoci)

> [!NOTE]  
> La configurazione di questa sezione è applicabile a database relazionali in generale. I metodi di estensione qui verranno rese disponibili quando si installa un provider di database relazionali (a causa di condiviso *Microsoft.EntityFrameworkCore.Relational* pacchetto).

Un vincolo unique è stato introdotto per ogni chiave alternativa nel modello.

## <a name="conventions"></a>Convenzioni

Per convenzione, l'indice e vincolo introdotte per una chiave alternativa verrà denominati `AK_<type name>_<property name>`. Per le chiavi alternative composte `<property name>` diventa un elenco separato da un carattere di sottolineatura di nomi di proprietà.

## <a name="data-annotations"></a>Annotazioni dei dati

I vincoli UNIQUE non possono essere configurati utilizzando le annotazioni dei dati.

## <a name="fluent-api"></a>Microsoft Office Fluent API

Per configurare il nome di indice e vincolo per una chiave alternativa, è possibile utilizzare l'API Fluent.

[!code-csharp[Main](../../../../samples/core/Modeling/FluentAPI/Samples/Relational/AlternateKeyName.cs?name=Model&highlight=9)]
