---
title: Database SQLite - limitazioni - Provider EF Core
author: rowanmiller
ms.author: divega
ms.date: 04/09/2017
ms.assetid: 94ab4800-c460-4caa-a5e8-acdfee6e6ce2
ms.technology: entity-framework-core
uid: core/providers/sqlite/limitations
ms.openlocfilehash: 08a4b8c26a3678491d412b333a7415cb45d4231f
ms.sourcegitcommit: 01a75cd483c1943ddd6f82af971f07abde20912e
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 10/27/2017
---
# <a name="sqlite-ef-core-database-provider-limitations"></a><span data-ttu-id="cff8b-102">Limitazioni del Provider di SQLite EF Core Database</span><span class="sxs-lookup"><span data-stu-id="cff8b-102">SQLite EF Core Database Provider Limitations</span></span>

<span data-ttu-id="cff8b-103">Il provider di SQLite è un numero di limitazioni di migrazioni.</span><span class="sxs-lookup"><span data-stu-id="cff8b-103">The SQLite provider has a number of migrations limitations.</span></span> <span data-ttu-id="cff8b-104">La maggior parte di queste limitazioni sono il risultato delle limitazioni nel motore di database SQLite sottostante e non sono specifica di Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="cff8b-104">Most of these limitations are a result of limitations in the underlying SQLite database engine and are not specific to EF.</span></span>

## <a name="modeling-limitations"></a><span data-ttu-id="cff8b-105">Limitazioni di modellazione</span><span class="sxs-lookup"><span data-stu-id="cff8b-105">Modeling limitations</span></span>

<span data-ttu-id="cff8b-106">Libreria comune relazionale (condivisa dai provider di database relazionale di Entity Framework) definisce le API per la modellizzazione concetti che sono comuni alla maggior parte dei motori di database relazionale.</span><span class="sxs-lookup"><span data-stu-id="cff8b-106">The common relational library (shared by Entity Framework relational database providers) defines APIs for modelling concepts that are common to most relational database engines.</span></span> <span data-ttu-id="cff8b-107">Due di questi concetti non sono supportati dal provider di SQLite.</span><span class="sxs-lookup"><span data-stu-id="cff8b-107">A couple of these concepts are not supported by the SQLite provider.</span></span>

* <span data-ttu-id="cff8b-108">Schemi</span><span class="sxs-lookup"><span data-stu-id="cff8b-108">Schemas</span></span>
* <span data-ttu-id="cff8b-109">Sequenze</span><span class="sxs-lookup"><span data-stu-id="cff8b-109">Sequences</span></span>

## <a name="migrations-limitations"></a><span data-ttu-id="cff8b-110">Limitazioni delle migrazioni</span><span class="sxs-lookup"><span data-stu-id="cff8b-110">Migrations limitations</span></span>

<span data-ttu-id="cff8b-111">Il motore di database SQLite non supporta un numero di operazioni sullo schema supportate dalla maggioranza degli altri database relazionali.</span><span class="sxs-lookup"><span data-stu-id="cff8b-111">The SQLite database engine does not support a number of schema operations that are supported by the majority of other relational databases.</span></span> <span data-ttu-id="cff8b-112">Se si tenta di applicare una delle operazioni non supportate in un database SQLite un `NotSupportedException` verrà generata.</span><span class="sxs-lookup"><span data-stu-id="cff8b-112">If you attempt to apply one of the unsupported operations to a SQLite database then a `NotSupportedException` will be thrown.</span></span>

| <span data-ttu-id="cff8b-113">Operazione</span><span class="sxs-lookup"><span data-stu-id="cff8b-113">Operation</span></span>            | <span data-ttu-id="cff8b-114">Supportato?</span><span class="sxs-lookup"><span data-stu-id="cff8b-114">Supported?</span></span> |
| -------------------- | ---------- |
| <span data-ttu-id="cff8b-115">AddColumn</span><span class="sxs-lookup"><span data-stu-id="cff8b-115">AddColumn</span></span>            | <span data-ttu-id="cff8b-116">✔</span><span class="sxs-lookup"><span data-stu-id="cff8b-116">✔</span></span>          |
| <span data-ttu-id="cff8b-117">AddForeignKey</span><span class="sxs-lookup"><span data-stu-id="cff8b-117">AddForeignKey</span></span>        | <span data-ttu-id="cff8b-118">✗</span><span class="sxs-lookup"><span data-stu-id="cff8b-118">✗</span></span>          |
| <span data-ttu-id="cff8b-119">AddPrimaryKey</span><span class="sxs-lookup"><span data-stu-id="cff8b-119">AddPrimaryKey</span></span>        | <span data-ttu-id="cff8b-120">✗</span><span class="sxs-lookup"><span data-stu-id="cff8b-120">✗</span></span>          |
| <span data-ttu-id="cff8b-121">AddUniqueConstraint</span><span class="sxs-lookup"><span data-stu-id="cff8b-121">AddUniqueConstraint</span></span>  | <span data-ttu-id="cff8b-122">✗</span><span class="sxs-lookup"><span data-stu-id="cff8b-122">✗</span></span>          |
| <span data-ttu-id="cff8b-123">AlterColumn</span><span class="sxs-lookup"><span data-stu-id="cff8b-123">AlterColumn</span></span>          | <span data-ttu-id="cff8b-124">✗</span><span class="sxs-lookup"><span data-stu-id="cff8b-124">✗</span></span>          |
| <span data-ttu-id="cff8b-125">CreateIndex</span><span class="sxs-lookup"><span data-stu-id="cff8b-125">CreateIndex</span></span>          | <span data-ttu-id="cff8b-126">✔</span><span class="sxs-lookup"><span data-stu-id="cff8b-126">✔</span></span>          |
| <span data-ttu-id="cff8b-127">CreateTable</span><span class="sxs-lookup"><span data-stu-id="cff8b-127">CreateTable</span></span>          | <span data-ttu-id="cff8b-128">✔</span><span class="sxs-lookup"><span data-stu-id="cff8b-128">✔</span></span>          |
| <span data-ttu-id="cff8b-129">DropColumn</span><span class="sxs-lookup"><span data-stu-id="cff8b-129">DropColumn</span></span>           | <span data-ttu-id="cff8b-130">✗</span><span class="sxs-lookup"><span data-stu-id="cff8b-130">✗</span></span>          |
| <span data-ttu-id="cff8b-131">DropForeignKey</span><span class="sxs-lookup"><span data-stu-id="cff8b-131">DropForeignKey</span></span>       | <span data-ttu-id="cff8b-132">✗</span><span class="sxs-lookup"><span data-stu-id="cff8b-132">✗</span></span>          |
| <span data-ttu-id="cff8b-133">DropIndex</span><span class="sxs-lookup"><span data-stu-id="cff8b-133">DropIndex</span></span>            | <span data-ttu-id="cff8b-134">✔</span><span class="sxs-lookup"><span data-stu-id="cff8b-134">✔</span></span>          |
| <span data-ttu-id="cff8b-135">DropPrimaryKey</span><span class="sxs-lookup"><span data-stu-id="cff8b-135">DropPrimaryKey</span></span>       | <span data-ttu-id="cff8b-136">✗</span><span class="sxs-lookup"><span data-stu-id="cff8b-136">✗</span></span>          |
| <span data-ttu-id="cff8b-137">DropTable</span><span class="sxs-lookup"><span data-stu-id="cff8b-137">DropTable</span></span>            | <span data-ttu-id="cff8b-138">✔</span><span class="sxs-lookup"><span data-stu-id="cff8b-138">✔</span></span>          |
| <span data-ttu-id="cff8b-139">DropUniqueConstraint</span><span class="sxs-lookup"><span data-stu-id="cff8b-139">DropUniqueConstraint</span></span> | <span data-ttu-id="cff8b-140">✗</span><span class="sxs-lookup"><span data-stu-id="cff8b-140">✗</span></span>          |
| <span data-ttu-id="cff8b-141">RenameColumn</span><span class="sxs-lookup"><span data-stu-id="cff8b-141">RenameColumn</span></span>         | <span data-ttu-id="cff8b-142">✗</span><span class="sxs-lookup"><span data-stu-id="cff8b-142">✗</span></span>          |
| <span data-ttu-id="cff8b-143">RenameIndex</span><span class="sxs-lookup"><span data-stu-id="cff8b-143">RenameIndex</span></span>          | <span data-ttu-id="cff8b-144">✗</span><span class="sxs-lookup"><span data-stu-id="cff8b-144">✗</span></span>          |
| <span data-ttu-id="cff8b-145">RenameTable</span><span class="sxs-lookup"><span data-stu-id="cff8b-145">RenameTable</span></span>          | <span data-ttu-id="cff8b-146">✔</span><span class="sxs-lookup"><span data-stu-id="cff8b-146">✔</span></span>          |

## <a name="migrations-limitations-workaround"></a><span data-ttu-id="cff8b-147">Soluzione alternativa limitazioni migrazioni</span><span class="sxs-lookup"><span data-stu-id="cff8b-147">Migrations limitations workaround</span></span>

<span data-ttu-id="cff8b-148">È possibile risolvere alcune di queste limitazioni da scrivere manualmente il codice per le migrazioni per eseguire una tabella ricompilare.</span><span class="sxs-lookup"><span data-stu-id="cff8b-148">You can workaround some of these limitations by manually writing code in your migrations to perform a table rebuild.</span></span> <span data-ttu-id="cff8b-149">Una ricompilazione della tabella comporta la ridenominazione della tabella esistente, creando una nuova tabella, la copia dei dati nella nuova tabella ed eliminazione della tabella precedente.</span><span class="sxs-lookup"><span data-stu-id="cff8b-149">A table rebuild involves renaming the existing table, creating a new table, copying data to the new table, and dropping the old table.</span></span> <span data-ttu-id="cff8b-150">È necessario utilizzare il `Sql(string)` metodo per eseguire alcune di queste operazioni.</span><span class="sxs-lookup"><span data-stu-id="cff8b-150">You will need to use the `Sql(string)` method to perform some of these steps.</span></span>

<span data-ttu-id="cff8b-151">Vedere [apportare altri tipi di tabella le modifiche dello Schema](http://sqlite.org/lang_altertable.html#otheralter) nella documentazione di SQLite per altri dettagli.</span><span class="sxs-lookup"><span data-stu-id="cff8b-151">See [Making Other Kinds Of Table Schema Changes](http://sqlite.org/lang_altertable.html#otheralter) in the SQLite documentation for more details.</span></span>

<span data-ttu-id="cff8b-152">In futuro, EF possono supportare alcune di queste operazioni tramite l'approccio di ricompilazione tabella dietro le quinte.</span><span class="sxs-lookup"><span data-stu-id="cff8b-152">In the future, EF may support some of these operations by using the table rebuild approach under the covers.</span></span> <span data-ttu-id="cff8b-153">È possibile [tenere traccia di questa funzionalità nel progetto GitHub](https://github.com/aspnet/EntityFramework/issues/329).</span><span class="sxs-lookup"><span data-stu-id="cff8b-153">You can [track this feature on our GitHub project](https://github.com/aspnet/EntityFramework/issues/329).</span></span>
