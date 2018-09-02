---
title: Come scegliere tra EF Core ed EF6
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 288f825b-b3e6-4096-971b-d0a1cb96770e
uid: efcore-and-ef6/choosing
ms.openlocfilehash: 83ae754f899b624d322c48e8de8432b65519546e
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 08/27/2018
ms.locfileid: "42993833"
---
# <a name="ef-core-and-ef6-which-one-is-right-for-you"></a>Come scegliere tra EF Core ed EF6

Le informazioni seguenti consentono di scegliere tra Entity Framework Core ed Entity Framework 6.

## <a name="guidance-for-new-applications"></a>Linee guida per le nuove applicazioni

Può essere utile usare EF Core per le nuove applicazioni per avere a disposizione tutte le funzionalità di EF Core e quando l'applicazione non richiede funzionalità che non sono state ancora implementate in EF Core.

EF6 richiede .NET Framework 4.0 o versione successiva ed è supportato solo in Windows (ovvero EF6 non può essere eseguito in .NET Core e non è supportato in altri sistemi operativi), ma rappresenta ancora una valida scelta per le nuove applicazioni a condizione che tali vincoli siano accettabili e l'applicazione non richieda nuove funzionalità in EF Core non disponibili per EF6.

Per verificare se EF Core può essere una scelta adatta per la propria applicazione, vedere [Confronto delle funzionalità](features.md).

## <a name="guidance-for-existing-ef6-applications"></a>Linee guida per le applicazioni EF6 esistenti

A seguito delle modifiche sostanziali in EF Core, non è consigliabile tentare di spostare un'applicazione da EF6 a EF Core se non in presenza di un valido motivo per apportare la modifica. Se si vuole passare a EF Core per usare le nuove funzionalità, assicurarsi di essere a conoscenza delle relative limitazioni prima di iniziare. Per verificare se EF Core può essere una scelta adatta per la propria applicazione, vedere [Confronto delle funzionalità](features.md).

**Il trasferimento da EF6 a EF Core deve essere visto come una conversione più che come un aggiornamento.** Per altre informazioni, vedere [Porting from EF6 to EF Core](porting/index.md) (Conversione da EF6 a EF Core).
