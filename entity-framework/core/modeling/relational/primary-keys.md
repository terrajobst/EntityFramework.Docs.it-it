---
title: Chiavi primarie-EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: c78f8f42-564a-45a4-aca7-3ede9f7ed2bc
uid: core/modeling/relational/primary-keys
ms.openlocfilehash: c195cc37859ea0689b5c6fbb84051564cf96c83a
ms.sourcegitcommit: 18ab4c349473d94b15b4ca977df12147db07b77f
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/06/2019
ms.locfileid: "73656046"
---
# <a name="primary-keys"></a><span data-ttu-id="19c64-102">Chiavi primarie</span><span class="sxs-lookup"><span data-stu-id="19c64-102">Primary Keys</span></span>

> [!NOTE]  
> <span data-ttu-id="19c64-103">La configurazione di questa sezione è applicabile in generale ai database relazionali.</span><span class="sxs-lookup"><span data-stu-id="19c64-103">The configuration in this section is applicable to relational databases in general.</span></span> <span data-ttu-id="19c64-104">I metodi di estensione descritti diventano disponibili quando si installa un provider di database relazionali (a causa del pacchetto *Microsoft.EntityFrameworkCore.Relational* condiviso).</span><span class="sxs-lookup"><span data-stu-id="19c64-104">The extension methods shown here will become available when you install a relational database provider (due to the shared *Microsoft.EntityFrameworkCore.Relational* package).</span></span>

<span data-ttu-id="19c64-105">Per la chiave di ogni tipo di entità è stato introdotto un vincolo PRIMARY KEY.</span><span class="sxs-lookup"><span data-stu-id="19c64-105">A primary key constraint is introduced for the key of each entity type.</span></span>

## <a name="conventions"></a><span data-ttu-id="19c64-106">Convenzioni</span><span class="sxs-lookup"><span data-stu-id="19c64-106">Conventions</span></span>

<span data-ttu-id="19c64-107">Per convenzione, la chiave primaria nel database verrà denominata `PK_<type name>`.</span><span class="sxs-lookup"><span data-stu-id="19c64-107">By convention, the primary key in the database will be named `PK_<type name>`.</span></span>

## <a name="data-annotations"></a><span data-ttu-id="19c64-108">Annotazioni dei dati</span><span class="sxs-lookup"><span data-stu-id="19c64-108">Data Annotations</span></span>

<span data-ttu-id="19c64-109">Non è possibile configurare aspetti specifici di un database relazionale di una chiave primaria usando le annotazioni dei dati.</span><span class="sxs-lookup"><span data-stu-id="19c64-109">No relational database specific aspects of a primary key can be configured using Data Annotations.</span></span>

## <a name="fluent-api"></a><span data-ttu-id="19c64-110">API Fluent</span><span class="sxs-lookup"><span data-stu-id="19c64-110">Fluent API</span></span>

<span data-ttu-id="19c64-111">È possibile utilizzare l'API Fluent per configurare il nome del vincolo PRIMARY KEY nel database.</span><span class="sxs-lookup"><span data-stu-id="19c64-111">You can use the Fluent API to configure the name of the primary key constraint in the database.</span></span>

[!code-csharp[Main](../../../../samples/core/Modeling/FluentAPI/Relational/KeyName.cs?name=KeyName&highlight=9)]
