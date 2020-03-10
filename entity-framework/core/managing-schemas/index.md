---
title: Gestione di schemi di database - EF Core
author: bricelam
ms.date: 10/30/2017
ms.openlocfilehash: 2da17865cb0192fb3e6e3396e4ca5f31fde9c52a
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78412736"
---
# <a name="managing-database-schemas"></a>Gestione di schemi di database

EF Core offre due metodi principali per mantenere sincronizzati il modello di EF Core e lo schema di database. Per scegliere tra i due, decidere se il modello di EF Core o lo schema del database è l'origine di dati reali.

Se si sceglie il modello di EF Core come origine di dati reali, usare le [migrazioni][1]. Quando si apportano modifiche al modello di EF Core questo approccio consente di applicare in modo incrementale le modifiche corrispondenti dello schema al database, in modo che rimanga compatibile con il modello di EF Core.

Usare il [reverse engineering][2] se si vuole usare lo schema del database come origine di dati reali. Questo approccio consente di eseguire lo scaffolding di un elemento DbContext e delle classi del tipo di entità decompilando lo schema del database in un modello di EF Core.

> [!NOTE]
> Anche le [API di creazione ed eliminazione][3] sono in grado di creare lo schema del database dal modello di EF Core. Tuttavia, sono destinate principalmente ai test e ad altri scenari in cui l'eliminazione del database è accettabile.


  [1]: migrations/index.md
  [2]: scaffolding.md
  [3]: ensure-created.md
