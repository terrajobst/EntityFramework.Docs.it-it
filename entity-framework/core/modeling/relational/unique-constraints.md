---
title: Chiavi alternative (vincoli UNIQUE)-EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 3d419dcf-2b5d-467c-b408-ea03d830721a
uid: core/modeling/relational/unique-constraints
ms.openlocfilehash: 7afcb804aeeccbd5e07c228a8fd9850ca00a2919
ms.sourcegitcommit: ec196918691f50cd0b21693515b0549f06d9f39c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/23/2019
ms.locfileid: "71197610"
---
# <a name="alternate-keys-unique-constraints"></a><span data-ttu-id="38f6c-102">Chiavi alternative (vincoli UNIQUE)</span><span class="sxs-lookup"><span data-stu-id="38f6c-102">Alternate Keys (Unique Constraints)</span></span>

> [!NOTE]  
> <span data-ttu-id="38f6c-103">La configurazione di questa sezione è applicabile in generale ai database relazionali.</span><span class="sxs-lookup"><span data-stu-id="38f6c-103">The configuration in this section is applicable to relational databases in general.</span></span> <span data-ttu-id="38f6c-104">I metodi di estensione descritti diventano disponibili quando si installa un provider di database relazionali (a causa del pacchetto *Microsoft.EntityFrameworkCore.Relational* condiviso).</span><span class="sxs-lookup"><span data-stu-id="38f6c-104">The extension methods shown here will become available when you install a relational database provider (due to the shared *Microsoft.EntityFrameworkCore.Relational* package).</span></span>

<span data-ttu-id="38f6c-105">Un vincolo UNIQUE viene introdotto per ogni chiave alternativa nel modello.</span><span class="sxs-lookup"><span data-stu-id="38f6c-105">A unique constraint is introduced for each alternate key in the model.</span></span>

## <a name="conventions"></a><span data-ttu-id="38f6c-106">Convenzioni</span><span class="sxs-lookup"><span data-stu-id="38f6c-106">Conventions</span></span>

<span data-ttu-id="38f6c-107">Per convenzione, l'indice e il vincolo introdotti per una chiave alternativa verranno denominati `AK_<type name>_<property name>`.</span><span class="sxs-lookup"><span data-stu-id="38f6c-107">By convention, the index and constraint that are introduced for an alternate key will be named `AK_<type name>_<property name>`.</span></span> <span data-ttu-id="38f6c-108">Per chiavi `<property name>` alternative composite diventa un elenco di nomi di proprietà separati da un carattere di sottolineatura.</span><span class="sxs-lookup"><span data-stu-id="38f6c-108">For composite alternate keys `<property name>` becomes an underscore separated list of property names.</span></span>

## <a name="data-annotations"></a><span data-ttu-id="38f6c-109">Annotazioni dei dati</span><span class="sxs-lookup"><span data-stu-id="38f6c-109">Data Annotations</span></span>

<span data-ttu-id="38f6c-110">Non è possibile configurare vincoli UNIQUE usando le annotazioni dei dati.</span><span class="sxs-lookup"><span data-stu-id="38f6c-110">Unique constraints can not be configured using Data Annotations.</span></span>

## <a name="fluent-api"></a><span data-ttu-id="38f6c-111">API Fluent</span><span class="sxs-lookup"><span data-stu-id="38f6c-111">Fluent API</span></span>

<span data-ttu-id="38f6c-112">È possibile usare l'API Fluent per configurare il nome di indice e vincolo per una chiave alternativa.</span><span class="sxs-lookup"><span data-stu-id="38f6c-112">You can use the Fluent API to configure the index and constraint name for an alternate key.</span></span>

[!code-csharp[Main](../../../../samples/core/Modeling/FluentAPI/Relational/AlternateKeyName.cs?name=Model&highlight=9)]
