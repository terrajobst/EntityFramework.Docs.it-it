---
title: Migrazioni in ambienti di Team - EF Core
author: bricelam
ms.author: bricelam
ms.date: 10/30/2017
ms.openlocfilehash: e8ff7f468d5ab6dbd6285f1abf9199e413288d10
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 08/27/2018
ms.locfileid: "42997695"
---
<a name="migrations-in-team-environments"></a>Migrazioni in ambienti di Team
===============================
Quando si lavora con le migrazioni in ambienti di team, prestare particolare attenzione a file di snapshot del modello. Questo file può indicare se la migrazione del collega normalmente unite con quelle dell'utente o se è necessario risolvere un conflitto ricreando la migrazione prima di condividerlo.

<a name="merging"></a>Unione
-------
Quando si uniscono le migrazioni dai colleghi del team, è possibile ottenere i conflitti nel file snapshot del modello. Se entrambe le modifiche non sono correlate, l'unione è semplice e due migrazioni possono coesistere. Ad esempio, possibile che si verifichi un conflitto di merge nella configurazione del tipo di entità customer che si presenta come segue:

    <<<<<<< Mine
    b.Property<bool>("Deactivated");
    =======
    b.Property<int>("LoyaltyPoints");
    >>>>>>> Theirs

Poiché entrambe queste proprietà devono esistere nel modello finale, completare il merge mediante l'aggiunta di entrambe le proprietà. In molti casi, il sistema di controllo della versione può unire automaticamente tali modifiche automaticamente.

``` csharp
b.Property<bool>("Deactivated");
b.Property<int>("LoyaltyPoints");
```

In questi casi, la migrazione e la migrazione del collega sono indipendenti tra loro. Poiché una di esse può essere applicata per primo, non è necessario apportare modifiche aggiuntive alla migrazione prima di condividerla con il tuo team.

<a name="resolving-conflicts"></a>Risoluzione dei conflitti
-------------------
In alcuni casi si verifica un conflitto true quando si uniscono il modello di snapshot del modello. Ad esempio, si e si collega ognuno sia rinominato la stessa proprietà.

    <<<<<<< Mine
    b.Property<string>("Username");
    =======
    b.Property<string>("Alias");
    >>>>>>> Theirs

Se si verifica questo tipo di conflitto, risolvere il problema creando nuovamente la migrazione. Attenersi ai passaggi riportati di seguito.

1. Interrompere il merge e il rollback alla directory di lavoro prima del merge
2. Rimuovere la migrazione (ma mantenere le modifiche del modello)
3. Unire le modifiche del collega in directory di lavoro
4. Aggiungere di nuovo la migrazione

Dopo questa operazione, due migrazioni possono essere applicate nell'ordine corretto. La migrazione viene applicata per primo, la colonna da rinominare *Alias*, in seguito la migrazione viene rinominato da *Username*.

La migrazione può essere condiviso in modo sicuro con il resto del team.
