---
title: Chiavi alternative-EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 8a5931d4-b480-4298-af36-0e29d74a37c0
uid: core/modeling/alternate-keys
ms.openlocfilehash: e5bb0602adb465435c8e88d045395a9424b2d9a3
ms.sourcegitcommit: 18ab4c349473d94b15b4ca977df12147db07b77f
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/06/2019
ms.locfileid: "73655780"
---
# <a name="alternate-keys"></a>Chiavi alternative

Una chiave alternativa funge da identificatore univoco alternativo per ogni istanza di entità oltre alla chiave primaria. È possibile utilizzare chiavi alternative come destinazione di una relazione. Quando si utilizza un database relazionale, viene eseguito il mapping al concetto di indice/vincolo univoco nelle colonne chiave alternative e di uno o più vincoli di chiave esterna che fanno riferimento alle colonne.

> [!TIP]  
> Se si vuole semplicemente applicare l'univocità di una colonna, è necessario un indice univoco anziché una chiave alternativa, vedere [indici](indexes.md). In EF i tasti alternativi forniscono funzionalità maggiori rispetto agli indici univoci, perché possono essere usati come destinazione di una chiave esterna.

Quando necessario, vengono in genere introdotte chiavi alternative e non è necessario configurarle manualmente. Per ulteriori informazioni, vedere [convenzioni](#conventions) .

## <a name="conventions"></a>Convenzioni

Per convenzione, viene introdotta una chiave alternativa quando si identifica una proprietà, che non è la chiave primaria, come destinazione di una relazione.

[!code-csharp[Main](../../../samples/core/Modeling/Conventions/AlternateKey.cs?name=AlternateKey&highlight=12)]

## <a name="data-annotations"></a>Annotazioni dei dati

Non è possibile configurare chiavi alternative usando le annotazioni dei dati.

## <a name="fluent-api"></a>API Fluent

È possibile usare l'API Fluent per configurare una singola proprietà in modo che sia una chiave alternativa.

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/AlternateKeySingle.cs?name=AlternateKeySingle&highlight=7,8)]

È anche possibile usare l'API Fluent per configurare più proprietà in modo che siano una chiave alternativa (nota come chiave alternativa composita).

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/AlternateKeyComposite.cs?name=AlternateKeyComposite&highlight=7,8)]
