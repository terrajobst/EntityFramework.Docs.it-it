---
title: Creazione e configurazione di un modello - EF Core
author: rowanmiller
ms.date: 11/05/2019
ms.assetid: 88253ff3-174e-485c-b3f8-768243d01ee1
uid: core/modeling/index
ms.openlocfilehash: 58be4a45473c6292790da341e360b3340de27be7
ms.sourcegitcommit: 7a709ce4f77134782393aa802df5ab2718714479
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 12/04/2019
ms.locfileid: "74824691"
---
# <a name="creating-and-configuring-a-model"></a>Creazione e configurazione di un modello

Entity Framework usa un insieme di convenzioni per compilare un modello in base alla forma delle classi di entità. È possibile specificare una configurazione aggiuntiva per integrare e/o sostituire gli elementi individuati dalla convenzione.

Questo articolo descrive la configurazione che può essere applicata a un modello destinato a qualsiasi archivio dati e che può essere applicata per qualsiasi database relazionale. I provider possono anche abilitare una configurazione specifica di un archivio dati. Per informazioni sulla configurazione specifica del provider, vedere la sezione  [Provider di database](../providers/index.md) .

> [!TIP]  
> È possibile visualizzare l' [esempio](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples)  di questo articolo in GitHub.

## <a name="use-fluent-api-to-configure-a-model"></a>Usare l'API Fluent per configurare un modello

È possibile eseguire l'override del metodo  `OnModelCreating`  nel contesto derivato e usare  `ModelBuilder API` per configurare il modello. Questo è il metodo di configurazione più efficace e consente di specificare la configurazione senza modificare le classi di entità. La configurazione dell'API Fluent ha la precedenza più elevata e sostituisce le convenzioni e le annotazioni dei dati.

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Required.cs?highlight=11-13)]

## <a name="use-data-annotations-to-configure-a-model"></a>Usare le annotazioni dei dati per configurare un modello

È anche possibile applicare attributi (chiamati annotazioni dei dati) alle classi e alle proprietà. Le annotazioni dei dati sostituiranno le convenzioni, ma saranno a loro volta ignorate dalla configurazione dell'API Fluent.

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/Required.cs?highlight=14)]
