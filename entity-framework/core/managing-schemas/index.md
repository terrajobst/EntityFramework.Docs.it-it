---
title: Gestione di schemi di database - EF Core
author: bricelam
ms.author: divega
ms.date: 10/30/2017
ms.technology: entity-framework-core
ms.openlocfilehash: 765c80f43832e51471928d5f653aa12c6bd7c7ac
ms.sourcegitcommit: b467368cc350e6059fdc0949e042a41cb11e61d9
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 11/15/2017
---
# <a name="managing-database-schemas"></a>Gestione di schemi di database
EF Core offre due metodi principali per mantenere sincronizzati il modello di EF Core e lo schema di database. Per scegliere tra i due, decidere se il modello di EF Core o lo schema del database è l'origine di dati reali.

Se si sceglie il modello di EF Core come origine dati, usare le [migrazioni][1]. Quando si apportano modifiche al modello di EF Core questo approccio consente di applicare in modo incrementale le modifiche corrispondenti dello schema al database, in modo che rimanga compatibile con il modello di EF Core.

Usare la [decompilazione][2] se si vuole usare lo schema del database come origine di dati reali. Questo approccio consente di eseguire lo scaffolding di un elemento DbContext e delle classi del tipo di entità decompilando lo schema del database in un modello di EF Core.

> [!NOTE]
> Anche le [API di creazione ed eliminazione][3] sono in grado di creare lo schema del database dal modello di EF Core. Tuttavia, sono destinate principalmente ai test e ad altri scenari in cui l'eliminazione del database è accettabile.


  [1]: migrations/index.md
  [2]: scaffolding.md
  [3]: ensure-created.md
