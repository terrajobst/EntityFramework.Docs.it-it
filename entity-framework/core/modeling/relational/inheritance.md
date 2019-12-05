---
title: Ereditarietà (database relazionale)-EF Core
description: Come configurare l'ereditarietà del tipo di entità in un database relazionale tramite Entity Framework Core
author: AndriySvyryd
ms.author: ansvyryd
ms.date: 11/06/2019
uid: core/modeling/relational/inheritance
ms.openlocfilehash: 30e25aa2968ceab03404baddb46e0ae59fc3ea6b
ms.sourcegitcommit: 7a709ce4f77134782393aa802df5ab2718714479
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 12/04/2019
ms.locfileid: "74824752"
---
# <a name="inheritance-relational-database"></a>Ereditarietà (database relazionale)

> [!NOTE]  
> La configurazione di questa sezione è applicabile in generale ai database relazionali. I metodi di estensione descritti diventano disponibili quando si installa un provider di database relazionali (a causa del pacchetto *Microsoft.EntityFrameworkCore.Relational* condiviso).

L'ereditarietà nel modello EF viene utilizzata per controllare il modo in cui l'ereditarietà nelle classi di entità viene rappresentata nel database.

> [!NOTE]  
> Attualmente, in EF Core viene implementato solo il modello tabella per gerarchia (TPH). Altri modelli comuni, ad esempio tabella per tipo (TPT) e tabella per tipo concreto (TPC), non sono ancora disponibili.

## <a name="conventions"></a>Convenzioni

Per impostazione predefinita, viene eseguito il mapping dell'ereditarietà utilizzando il modello tabella per gerarchia (TPH). TPH usa una singola tabella per archiviare i dati per tutti i tipi nella gerarchia. Una colonna discriminatore viene utilizzata per identificare il tipo rappresentato da ogni riga.

EF Core configurerà l'ereditarietà solo se due o più tipi ereditati vengono inclusi in modo esplicito nel modello. per ulteriori informazioni, vedere [ereditarietà](../inheritance.md) .

Di seguito è riportato un esempio che mostra uno scenario di ereditarietà semplice e i dati archiviati in una tabella di database relazionale usando il modello TPH. La colonna *discriminatore* identifica il tipo di *Blog* archiviato in ogni riga.

[!code-csharp[Main](../../../../samples/core/Modeling/Conventions/InheritanceDbSets.cs#Model)]

![image](_static/inheritance-tph-data.png)

>[!NOTE]
> Le colonne del database vengono rese automaticamente Nullable quando necessario quando si usa il mapping di TPH.

## <a name="data-annotations"></a>Annotazioni dei dati

Non è possibile utilizzare le annotazioni dei dati per configurare l'ereditarietà.

## <a name="fluent-api"></a>API Fluent

È possibile utilizzare l'API Fluent per configurare il nome e il tipo della colonna discriminatore e i valori utilizzati per identificare ogni tipo nella gerarchia.

[!code-csharp[Main](../../../../samples/core/Modeling/FluentAPI/InheritanceTPHDiscriminator.cs#Inheritance)]

## <a name="configuring-the-discriminator-property"></a>Configurazione della proprietà Discriminator

Negli esempi precedenti, il discriminatore viene creato come [proprietà shadow](xref:core/modeling/shadow-properties) nell'entità di base della gerarchia. Poiché si tratta di una proprietà nel modello, può essere configurata esattamente come le altre proprietà. Ad esempio, per impostare la lunghezza massima quando viene usato il discriminatore predefinito per convenzione:

[!code-csharp[Main](../../../../samples/core/Modeling/FluentAPI/DefaultDiscriminator.cs#DiscriminatorConfiguration)]

È anche possibile eseguire il mapping del discriminatore a una proprietà .NET nell'entità e configurarla. Ad esempio:

[!code-csharp[Main](../../../../samples/core/Modeling/FluentAPI/NonShadowDiscriminator.cs#NonShadowDiscriminator)]

## <a name="shared-columns"></a>Colonne condivise

Quando due tipi di entità di pari livello hanno una proprietà con lo stesso nome, per impostazione predefinita verrà eseguito il mapping a due colonne separate. Tuttavia, se sono compatibili, è possibile eseguirne il mapping alla stessa colonna:

[!code-csharp[Main](../../../../samples/core/Modeling/FluentAPI/SharedTPHColumns.cs#SharedTPHColumns)]