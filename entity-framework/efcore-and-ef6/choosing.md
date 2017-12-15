---
title: Come scegliere tra EF Core ed EF6
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: 288f825b-b3e6-4096-971b-d0a1cb96770e
uid: efcore-and-ef6/choosing
ms.openlocfilehash: 9a113e0965fa75a03510199fb75165f6e9be0bbd
ms.sourcegitcommit: 01a75cd483c1943ddd6f82af971f07abde20912e
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 10/27/2017
---
# <a name="ef-core-and-ef6-which-one-is-right-for-you"></a>Come scegliere tra EF Core ed EF6

Le informazioni seguenti consentono di scegliere tra Entity Framework Core ed Entity Framework 6.

## <a name="guidance-for-new-applications"></a>Linee guida per le nuove applicazioni

Poiché EF Core è un prodotto nuovo ancora privo di alcune funzionalità O/RM critiche, EF6 resta per il momento la scelta più appropriata per diverse applicazioni.

**Questi sono i tipi di applicazioni per cui è consigliabile usare EF Core:**

* Nuove applicazioni che non richiedono funzionalità non ancora implementate in EF Core. Per verificare se EF Core può essere una scelta adatta per la propria applicazione, vedere [Confronto delle funzionalità](features.md).

* Applicazioni per .NET Core, ad esempio le applicazioni UWP (Universal Windows Platform) e ASP.NET Core. Queste applicazioni non possono usare EF6 perché è necessario .NET Framework (ad esempio, .NET Framework 4.5).

## <a name="guidance-for-existing-ef6-applications"></a>Linee guida per le applicazioni EF6 esistenti

A seguito delle modifiche sostanziali in EF Core, non è consigliabile tentare di spostare un'applicazione da EF6 a EF Core se non in presenza di un valido motivo per apportare la modifica. Se si vuole passare a EF Core per usare le nuove funzionalità, assicurarsi di essere a conoscenza delle relative limitazioni prima di iniziare. Per verificare se EF Core può essere una scelta adatta per la propria applicazione, vedere [Confronto delle funzionalità](features.md).

**Il trasferimento da EF6 a EF Core deve essere visto come una conversione più che come un aggiornamento.** Per altre informazioni, vedere [Porting from EF6 to EF Core](porting/index.md) (Conversione da EF6 a EF Core).
