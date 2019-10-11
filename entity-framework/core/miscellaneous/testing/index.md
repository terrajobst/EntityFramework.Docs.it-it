---
title: Testare i componenti usando EF Core - EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 1603be0c-69bc-4dd9-9a08-3d0129cdc6c1
uid: core/miscellaneous/testing/index
ms.openlocfilehash: 8de7df80ce91c4d94133a96d759dd552d0ba1884
ms.sourcegitcommit: 708b18520321c587b2046ad2ea9fa7c48aeebfe5
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 10/09/2019
ms.locfileid: "72181295"
---
# <a name="testing-components-using-ef-core"></a>Test dei componenti usando EF Core

È consigliabile testare i componenti tramite un elemento che si avvicina alla connessione al database reale senza il sovraccarico delle operazioni I/O del database effettivo.

A tale scopo sono disponibili due opzioni principali:
 * La [modalità InMemory SQLite](sqlite.md) consente di scrivere test efficienti in base a un provider che si comporta come un database relazionale.
 * Il [provider InMemory](in-memory.md) è un provider semplice con dipendenze minime, ma non sempre si comporta come un database relazionale.
