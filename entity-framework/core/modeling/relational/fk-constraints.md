---
title: Vincoli FOREIGN KEY-EF Core
author: AndriySvyryd
ms.date: 11/21/2019
uid: core/modeling/relational/fk-constraints
ms.openlocfilehash: 2855137adf2ba3c9edaabd15a05f7a209f00f685
ms.sourcegitcommit: 7a709ce4f77134782393aa802df5ab2718714479
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 12/04/2019
ms.locfileid: "74824587"
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
