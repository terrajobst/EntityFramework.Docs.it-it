---
title: Chiavi alternative (vincoli univoci) - EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 3d419dcf-2b5d-467c-b408-ea03d830721a
uid: core/modeling/relational/unique-constraints
ms.openlocfilehash: 7ec58ee31aac79e15329dc8542f37fd117772fbe
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 08/27/2018
ms.locfileid: "42994191"
---
# <a name="alternate-keys-unique-constraints"></a><span data-ttu-id="7bbba-102">Chiavi alternative (vincoli univoci)</span><span class="sxs-lookup"><span data-stu-id="7bbba-102">Alternate Keys (Unique Constraints)</span></span>

> [!NOTE]  
> <span data-ttu-id="7bbba-103">La configurazione di questa sezione è applicabile in generale ai database relazionali.</span><span class="sxs-lookup"><span data-stu-id="7bbba-103">The configuration in this section is applicable to relational databases in general.</span></span> <span data-ttu-id="7bbba-104">I metodi di estensione descritti diventano disponibili quando si installa un provider di database relazionali (a causa del pacchetto *Microsoft.EntityFrameworkCore.Relational* condiviso).</span><span class="sxs-lookup"><span data-stu-id="7bbba-104">The extension methods shown here will become available when you install a relational database provider (due to the shared *Microsoft.EntityFrameworkCore.Relational* package).</span></span>

<span data-ttu-id="7bbba-105">Un vincolo unique è stato introdotto per ogni chiave alternativo nel modello.</span><span class="sxs-lookup"><span data-stu-id="7bbba-105">A unique constraint is introduced for each alternate key in the model.</span></span>

## <a name="conventions"></a><span data-ttu-id="7bbba-106">Convenzioni</span><span class="sxs-lookup"><span data-stu-id="7bbba-106">Conventions</span></span>

<span data-ttu-id="7bbba-107">Per convenzione, verrà denominati l'indice e vincolo che sono state aggiunte per una chiave alternativa `AK_<type name>_<property name>`.</span><span class="sxs-lookup"><span data-stu-id="7bbba-107">By convention, the index and constraint that are introduced for an alternate key will be named `AK_<type name>_<property name>`.</span></span> <span data-ttu-id="7bbba-108">Per le chiavi alternative composte `<property name>` diventa un elenco separato da un carattere di sottolineatura di nomi di proprietà.</span><span class="sxs-lookup"><span data-stu-id="7bbba-108">For composite alternate keys `<property name>` becomes an underscore separated list of property names.</span></span>

## <a name="data-annotations"></a><span data-ttu-id="7bbba-109">Annotazioni dei dati</span><span class="sxs-lookup"><span data-stu-id="7bbba-109">Data Annotations</span></span>

<span data-ttu-id="7bbba-110">I vincoli UNIQUE non possono essere configurati utilizzando le annotazioni dei dati.</span><span class="sxs-lookup"><span data-stu-id="7bbba-110">Unique constraints can not be configured using Data Annotations.</span></span>

## <a name="fluent-api"></a><span data-ttu-id="7bbba-111">API Fluent</span><span class="sxs-lookup"><span data-stu-id="7bbba-111">Fluent API</span></span>

<span data-ttu-id="7bbba-112">Per configurare il nome di indice e vincolo per una chiave alternativa, è possibile utilizzare l'API Fluent.</span><span class="sxs-lookup"><span data-stu-id="7bbba-112">You can use the Fluent API to configure the index and constraint name for an alternate key.</span></span>

[!code-csharp[Main](../../../../samples/core/Modeling/FluentAPI/Samples/Relational/AlternateKeyName.cs?name=Model&highlight=9)]
