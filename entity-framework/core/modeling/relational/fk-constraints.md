---
title: Vincoli FOREIGN KEY-EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: dbaf4bac-1fd5-46c0-ac57-64d7153bc574
uid: core/modeling/relational/fk-constraints
ms.openlocfilehash: df739f01a799ec8edad4cf44d8cf50edf292992f
ms.sourcegitcommit: 18ab4c349473d94b15b4ca977df12147db07b77f
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/06/2019
ms.locfileid: "73656003"
---
# <a name="foreign-key-constraints"></a><span data-ttu-id="e5f33-102">Vincoli della chiave esterna</span><span class="sxs-lookup"><span data-stu-id="e5f33-102">Foreign Key Constraints</span></span>

> [!NOTE]  
> <span data-ttu-id="e5f33-103">La configurazione di questa sezione è applicabile in generale ai database relazionali.</span><span class="sxs-lookup"><span data-stu-id="e5f33-103">The configuration in this section is applicable to relational databases in general.</span></span> <span data-ttu-id="e5f33-104">I metodi di estensione descritti diventano disponibili quando si installa un provider di database relazionali (a causa del pacchetto *Microsoft.EntityFrameworkCore.Relational* condiviso).</span><span class="sxs-lookup"><span data-stu-id="e5f33-104">The extension methods shown here will become available when you install a relational database provider (due to the shared *Microsoft.EntityFrameworkCore.Relational* package).</span></span>

<span data-ttu-id="e5f33-105">Un vincolo FOREIGN KEY viene introdotto per ogni relazione nel modello.</span><span class="sxs-lookup"><span data-stu-id="e5f33-105">A foreign key constraint is introduced for each relationship in the model.</span></span>

## <a name="conventions"></a><span data-ttu-id="e5f33-106">Convenzioni</span><span class="sxs-lookup"><span data-stu-id="e5f33-106">Conventions</span></span>

<span data-ttu-id="e5f33-107">Per convenzione, i vincoli FOREIGN KEY sono denominati `FK_<dependent type name>_<principal type name>_<foreign key property name>`.</span><span class="sxs-lookup"><span data-stu-id="e5f33-107">By convention, foreign key constraints are named `FK_<dependent type name>_<principal type name>_<foreign key property name>`.</span></span> <span data-ttu-id="e5f33-108">Per le chiavi esterne composite `<foreign key property name>` diventa un elenco delimitato da caratteri di sottolineatura dei nomi delle proprietà di chiave esterna.</span><span class="sxs-lookup"><span data-stu-id="e5f33-108">For composite foreign keys `<foreign key property name>` becomes an underscore separated list of foreign key property names.</span></span>

## <a name="data-annotations"></a><span data-ttu-id="e5f33-109">Annotazioni dei dati</span><span class="sxs-lookup"><span data-stu-id="e5f33-109">Data Annotations</span></span>

<span data-ttu-id="e5f33-110">Impossibile configurare nomi di vincoli di chiave esterna utilizzando le annotazioni dei dati.</span><span class="sxs-lookup"><span data-stu-id="e5f33-110">Foreign key constraint names cannot be configured using data annotations.</span></span>

## <a name="fluent-api"></a><span data-ttu-id="e5f33-111">API Fluent</span><span class="sxs-lookup"><span data-stu-id="e5f33-111">Fluent API</span></span>

<span data-ttu-id="e5f33-112">Per configurare il nome del vincolo FOREIGN KEY per una relazione, è possibile usare l'API Fluent.</span><span class="sxs-lookup"><span data-stu-id="e5f33-112">You can use the Fluent API to configure the foreign key constraint name for a relationship.</span></span>

[!code-csharp[Main](../../../../samples/core/Modeling/FluentAPI/Relational/RelationshipConstraintName.cs?name=Constraint&highlight=12)]
