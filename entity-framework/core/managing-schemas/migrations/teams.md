---
title: Migrazioni in ambienti team-EF Core
author: bricelam
ms.author: bricelam
ms.date: 10/30/2017
uid: core/managing-schemas/migrations/teams
ms.openlocfilehash: 6c17c56277821159962884aef72d46c624442e20
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78416770"
---
# <a name="migrations-in-team-environments"></a>Migrazioni in ambienti team

Quando si utilizzano migrazioni in ambienti team, prestare particolare attenzione al file di snapshot del modello. Questo file può indicare se la migrazione del proprio team si unisce in modo corretto o se è necessario risolvere un conflitto creando nuovamente la migrazione prima di condividerla.

## <a name="merging"></a>Unione

Quando si esegue il merge delle migrazioni dai colleghi, è possibile che si verifichino conflitti nel file di snapshot del modello. Se entrambe le modifiche non sono correlate, l'Unione è semplice e le due migrazioni possono coesistere. Ad esempio, è possibile che si verifichi un conflitto di merge nella configurazione del tipo di entità Customer simile alla seguente:

``` output
<<<<<<< Mine
b.Property<bool>("Deactivated");
=======
b.Property<int>("LoyaltyPoints");
>>>>>>> Theirs
```

Poiché entrambe queste proprietà devono esistere nel modello finale, completare il merge aggiungendo entrambe le proprietà. In molti casi, il sistema di controllo della versione può unire automaticamente tali modifiche.

``` csharp
b.Property<bool>("Deactivated");
b.Property<int>("LoyaltyPoints");
```

In questi casi, la migrazione e la migrazione del proprio team sono indipendenti tra loro. Poiché una di esse potrebbe essere applicata per prima, non è necessario apportare altre modifiche alla migrazione prima di condividerla con il team.

## <a name="resolving-conflicts"></a>Risoluzione dei conflitti

In alcuni casi si verifica un vero conflitto durante l'Unione del modello di snapshot del modello. È possibile, ad esempio, che l'utente e il compagno di squadra abbiano rinominato la stessa proprietà.

``` output
<<<<<<< Mine
b.Property<string>("Username");
=======
b.Property<string>("Alias");
>>>>>>> Theirs
```

Se si verifica questo tipo di conflitto, risolverlo creando di nuovo la migrazione. A tale scopo, seguire questa procedura:

1. Interrompere l'Unione e il rollback nella directory di lavoro prima del merge
2. Rimuovere la migrazione (mantenendo però le modifiche del modello)
3. Unisci le modifiche dei colleghi nella directory di lavoro
4. Aggiungere nuovamente la migrazione

Al termine di questa operazione, le due migrazioni possono essere applicate nell'ordine corretto. La migrazione viene applicata per prima, rinominando la colonna in *alias*, successivamente la migrazione la rinomina a *nome utente*.

La migrazione può essere condivisa in modo sicuro con il resto del team.
