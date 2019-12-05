---
title: Vincoli FOREIGN KEY-EF Core
author: AndriySvyryd
ms.date: 11/21/2019
uid: core/modeling/relational/fk-constraints
ms.openlocfilehash: 2855137adf2ba3c9edaabd15a05f7a209f00f685
ms.sourcegitcommit: 7a709ce4f77134782393aa802df5ab2718714479
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 12/04/2019
ms.locfileid: "74824587"
---
# <a name="foreign-key-constraints"></a><span data-ttu-id="3e1d8-102">Vincoli della chiave esterna</span><span class="sxs-lookup"><span data-stu-id="3e1d8-102">Foreign Key Constraints</span></span>

> [!NOTE]  
> <span data-ttu-id="3e1d8-103">La configurazione di questa sezione è applicabile in generale ai database relazionali.</span><span class="sxs-lookup"><span data-stu-id="3e1d8-103">The configuration in this section is applicable to relational databases in general.</span></span> <span data-ttu-id="3e1d8-104">I metodi di estensione descritti diventano disponibili quando si installa un provider di database relazionali (a causa del pacchetto *Microsoft.EntityFrameworkCore.Relational* condiviso).</span><span class="sxs-lookup"><span data-stu-id="3e1d8-104">The extension methods shown here will become available when you install a relational database provider (due to the shared *Microsoft.EntityFrameworkCore.Relational* package).</span></span>

<span data-ttu-id="3e1d8-105">Un vincolo FOREIGN KEY viene introdotto per ogni relazione nel modello.</span><span class="sxs-lookup"><span data-stu-id="3e1d8-105">A foreign key constraint is introduced for each relationship in the model.</span></span>

## <a name="conventions"></a><span data-ttu-id="3e1d8-106">Convenzioni</span><span class="sxs-lookup"><span data-stu-id="3e1d8-106">Conventions</span></span>

<span data-ttu-id="3e1d8-107">Per convenzione, i vincoli FOREIGN KEY sono denominati `FK_<dependent type name>_<principal type name>_<foreign key property name>`.</span><span class="sxs-lookup"><span data-stu-id="3e1d8-107">By convention, foreign key constraints are named `FK_<dependent type name>_<principal type name>_<foreign key property name>`.</span></span> <span data-ttu-id="3e1d8-108">Per le chiavi esterne composite `<foreign key property name>` diventa un elenco delimitato da caratteri di sottolineatura dei nomi delle proprietà di chiave esterna.</span><span class="sxs-lookup"><span data-stu-id="3e1d8-108">For composite foreign keys `<foreign key property name>` becomes an underscore separated list of foreign key property names.</span></span>

## <a name="data-annotations"></a><span data-ttu-id="3e1d8-109">Annotazioni dei dati</span><span class="sxs-lookup"><span data-stu-id="3e1d8-109">Data Annotations</span></span>

<span data-ttu-id="3e1d8-110">Impossibile configurare nomi di vincoli di chiave esterna utilizzando le annotazioni dei dati.</span><span class="sxs-lookup"><span data-stu-id="3e1d8-110">Foreign key constraint names cannot be configured using data annotations.</span></span>

## <a name="fluent-api"></a><span data-ttu-id="3e1d8-111">API Fluent</span><span class="sxs-lookup"><span data-stu-id="3e1d8-111">Fluent API</span></span>

<span data-ttu-id="3e1d8-112">Per configurare il nome del vincolo FOREIGN KEY per una relazione, è possibile usare l'API Fluent.</span><span class="sxs-lookup"><span data-stu-id="3e1d8-112">You can use the Fluent API to configure the foreign key constraint name for a relationship.</span></span>

[!code-csharp[Main](../../../../samples/core/Modeling/FluentAPI/Relational/RelationshipConstraintName.cs?name=Constraint&highlight=12)]
