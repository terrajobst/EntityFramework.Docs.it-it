---
title: Migrazioni in ambienti Team - Core a Entity Framework
author: bricelam
ms.author: bricelam
ms.date: 10/30/2017
ms.technology: entity-framework-core
ms.openlocfilehash: 40cbc1c1bb0273bf733fadb884bffadcceeb162b
ms.sourcegitcommit: b467368cc350e6059fdc0949e042a41cb11e61d9
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/15/2017
---
<a name="migrations-in-team-environments"></a>Migrazioni in ambienti di Team
===============================
Quando si utilizzano le migrazioni in ambienti di team, prestare particolare attenzione al file di snapshot del modello. Questo file può indicare se la migrazione del collega unisce correttamente con i propri di se è necessario risolvere un conflitto ricreando la migrazione prima di condividerlo.

<a name="merging"></a>Unione
-------
Quando si uniscono le migrazioni da colleghi del team, è possibile ottenere i conflitti nel file di snapshot di modello. Se entrambe le modifiche non sono correlate, possono coesistere due migrazioni di tipo merge è piuttosto semplice. Ad esempio possibile che si verifichi un conflitto di merge nella configurazione del tipo di entità customer simile al seguente:

    <<<<<<< Mine
    b.Property<bool>("Deactivated");
    =======
    b.Property<int>("LoyaltyPoints");
    >>>>>>> Theirs

Poiché entrambe le proprietà devono esistere nel modello finale, è possibile completare il merge mediante l'aggiunta di entrambe le proprietà. In molti casi, il sistema di controllo di versione potrebbe unirà automaticamente tali modifiche automaticamente.

``` csharp
b.Property<bool>("Deactivated");
b.Property<int>("LoyaltyPoints");
```

In questi casi, la migrazione e la migrazione del collega sono indipendenti tra loro. Poiché una di esse può essere applicata per primo, non è necessario apportare eventuali ulteriori modifiche alla migrazione prima di condividerlo con il team.

<a name="resolving-conflicts"></a>Risoluzione dei conflitti
-------------------
In alcuni casi si verifica un conflitto true quando si uniscono i modelli di snapshot. Ad esempio si collega ognuno sia rinominato la stessa proprietà.

    <<<<<<< Mine
    b.Property<string>("Username");
    =======
    b.Property<string>("Alias");
    >>>>>>> Theirs

Se si verifica questo tipo di conflitto, risolvere il problema ricreare la migrazione. Attenersi ai passaggi riportati di seguito.

1. Interrompere il merge e rollback nella directory di lavoro prima dell'unione
2. Rimuovere la migrazione (ma mantenere le modifiche apportate al modello)
3. Unire le modifiche del collega nella directory di lavoro
4. Aggiungere nuovamente la migrazione

Dopo questa operazione, le due migrazioni possono essere applicate nell'ordine corretto. La migrazione viene applicata per primo, rinominare la colonna *Alias*, dopo la migrazione viene rinominato in *Username*.

La migrazione in modo sicuro può essere condivisa con il resto del team.
