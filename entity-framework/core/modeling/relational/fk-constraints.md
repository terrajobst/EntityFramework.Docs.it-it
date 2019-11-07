---
title: Vincoli FOREIGN KEY-EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: dbaf4bac-1fd5-46c0-ac57-64d7153bc574
uid: core/modeling/relational/fk-constraints
ms.openlocfilehash: df739f01a799ec8edad4cf44d8cf50edf292992f
ms.sourcegitcommit: 18ab4c349473d94b15b4ca977df12147db07b77f
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/06/2019
ms.locfileid: "73656003"
---
# <a name="foreign-key-constraints"></a>Vincoli della chiave esterna

> [!NOTE]  
> La configurazione di questa sezione è applicabile in generale ai database relazionali. I metodi di estensione descritti diventano disponibili quando si installa un provider di database relazionali (a causa del pacchetto *Microsoft.EntityFrameworkCore.Relational* condiviso).

Un vincolo FOREIGN KEY viene introdotto per ogni relazione nel modello.

## <a name="conventions"></a>Convenzioni

Per convenzione, i vincoli FOREIGN KEY sono denominati `FK_<dependent type name>_<principal type name>_<foreign key property name>`. Per le chiavi esterne composite `<foreign key property name>` diventa un elenco delimitato da caratteri di sottolineatura dei nomi delle proprietà di chiave esterna.

## <a name="data-annotations"></a>Annotazioni dei dati

Impossibile configurare nomi di vincoli di chiave esterna utilizzando le annotazioni dei dati.

## <a name="fluent-api"></a>API Fluent

Per configurare il nome del vincolo FOREIGN KEY per una relazione, è possibile usare l'API Fluent.

[!code-csharp[Main](../../../../samples/core/Modeling/FluentAPI/Relational/RelationshipConstraintName.cs?name=Constraint&highlight=12)]
