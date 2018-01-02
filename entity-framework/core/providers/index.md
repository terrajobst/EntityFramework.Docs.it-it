---
title: Provider di database - EF Core
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: 14fffb6c-a687-4881-a094-af4a1359a296
ms.technology: entity-framework-core
uid: core/providers/index
ms.openlocfilehash: 19c275b7e89c62e79c8bded977e39b2cfb2b439a
ms.sourcegitcommit: 01a75cd483c1943ddd6f82af971f07abde20912e
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 10/27/2017
---
# <a name="database-providers"></a>Provider di database

Entity Framework Core usa un modello di provider per consentire l'accesso a molti database diversi tramite EF. Alcuni concetti sono comuni alla maggior parte dei database e sono inclusi nei componenti primari di EF Core. Tali concetti includono l'espressione di query in LINQ, le transazioni e il rilevamento delle modifiche agli oggetti dopo che sono stati caricati dal database. Alcuni concetti sono specifici di un determinato provider. Il provider SQL Server, ad esempio, consente di configurare le tabelle ottimizzate per la memoria, una funzionalità specifica di SQL Server. Altri concetti sono specifici di una classe di provider. Ad esempio, i provider di EF Core per i database relazionali compilati sulla libreria `Microsoft.EntityFrameworkCore.Relational` comune, che offre le API per la configurazione della tabella e il mapping colonne, i vincoli della chiave esterna e così via.

I provider di EF Core vengono compilati da diverse origini. Non tutti i provider vengono mantenuti nell'ambito del progetto di Entity Framework Core. Quando si prende in considerazione un provider di terze parti, valutarne attentamente gli aspetti relativi a qualità, licenze, supporto, e così via per essere certi che le proprie esigenze vengano soddisfatte.
