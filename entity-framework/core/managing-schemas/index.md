---
title: Gestione di schemi di database - EF Core
author: bricelam
ms.author: divega
ms.date: 10/30/2017
ms.technology: entity-framework-core
ms.openlocfilehash: 765c80f43832e51471928d5f653aa12c6bd7c7ac
ms.sourcegitcommit: b467368cc350e6059fdc0949e042a41cb11e61d9
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 11/15/2017
ms.locfileid: "26049383"
---
# <a name="managing-database-schemas"></a><span data-ttu-id="9d4ca-102">Gestione di schemi di database</span><span class="sxs-lookup"><span data-stu-id="9d4ca-102">Managing Database Schemas</span></span>
<span data-ttu-id="9d4ca-103">EF Core offre due metodi principali per mantenere sincronizzati il modello di EF Core e lo schema di database. Per scegliere tra i due, decidere se il modello di EF Core o lo schema del database è l'origine di dati reali.</span><span class="sxs-lookup"><span data-stu-id="9d4ca-103">EF Core provides two primary ways of keeping your EF Core model and database schema in sync. To choose between the two, decide whether your EF Core model or the database schema is the source of truth.</span></span>

<span data-ttu-id="9d4ca-104">Se si sceglie il modello di EF Core come origine dati, usare le [migrazioni][1].</span><span class="sxs-lookup"><span data-stu-id="9d4ca-104">If you want your EF Core model to be the source of truth, use [Migrations][1].</span></span> <span data-ttu-id="9d4ca-105">Quando si apportano modifiche al modello di EF Core questo approccio consente di applicare in modo incrementale le modifiche corrispondenti dello schema al database, in modo che rimanga compatibile con il modello di EF Core.</span><span class="sxs-lookup"><span data-stu-id="9d4ca-105">As you make changes to your EF Core model, this approach incrementally applies the corresponding schema changes to your database so that it remains compatible with your EF Core model.</span></span>

<span data-ttu-id="9d4ca-106">Usare la [decompilazione][2] se si vuole usare lo schema del database come origine di dati reali.</span><span class="sxs-lookup"><span data-stu-id="9d4ca-106">Use [Reverse Engineering][2] if you want your database schema to be the source of truth.</span></span> <span data-ttu-id="9d4ca-107">Questo approccio consente di eseguire lo scaffolding di un elemento DbContext e delle classi del tipo di entità decompilando lo schema del database in un modello di EF Core.</span><span class="sxs-lookup"><span data-stu-id="9d4ca-107">This approach allows you to scaffold a DbContext and the entity type classes by reverse engineering your database schema into an EF Core model.</span></span>

> [!NOTE]
> <span data-ttu-id="9d4ca-108">Anche le [API di creazione ed eliminazione][3] sono in grado di creare lo schema del database dal modello di EF Core.</span><span class="sxs-lookup"><span data-stu-id="9d4ca-108">The [create and drop APIs][3] can also create the database schema from your EF Core model.</span></span> <span data-ttu-id="9d4ca-109">Tuttavia, sono destinate principalmente ai test e ad altri scenari in cui l'eliminazione del database è accettabile.</span><span class="sxs-lookup"><span data-stu-id="9d4ca-109">However, they are primarily for testing, prototyping, and other scenarios where dropping the database is acceptable.</span></span>


  [1]: migrations/index.md
  [2]: scaffolding.md
  [3]: ensure-created.md
