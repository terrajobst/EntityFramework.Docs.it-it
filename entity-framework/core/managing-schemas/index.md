---
title: Gestione di schemi di database - EF Core
author: bricelam
ms.date: 10/30/2017
ms.openlocfilehash: 2da17865cb0192fb3e6e3396e4ca5f31fde9c52a
ms.sourcegitcommit: 18ab4c349473d94b15b4ca977df12147db07b77f
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 11/06/2019
ms.locfileid: "73655650"
---
# <a name="managing-database-schemas"></a><span data-ttu-id="a2b08-102">Gestione di schemi di database</span><span class="sxs-lookup"><span data-stu-id="a2b08-102">Managing Database Schemas</span></span>

<span data-ttu-id="a2b08-103">EF Core offre due metodi principali per mantenere sincronizzati il modello di EF Core e lo schema di database. Per scegliere tra i due, decidere se il modello di EF Core o lo schema del database è l'origine di dati reali.</span><span class="sxs-lookup"><span data-stu-id="a2b08-103">EF Core provides two primary ways of keeping your EF Core model and database schema in sync. To choose between the two, decide whether your EF Core model or the database schema is the source of truth.</span></span>

<span data-ttu-id="a2b08-104">Se si sceglie il modello di EF Core come origine di dati reali, usare le [migrazioni][1].</span><span class="sxs-lookup"><span data-stu-id="a2b08-104">If you want your EF Core model to be the source of truth, use [Migrations][1].</span></span> <span data-ttu-id="a2b08-105">Quando si apportano modifiche al modello di EF Core questo approccio consente di applicare in modo incrementale le modifiche corrispondenti dello schema al database, in modo che rimanga compatibile con il modello di EF Core.</span><span class="sxs-lookup"><span data-stu-id="a2b08-105">As you make changes to your EF Core model, this approach incrementally applies the corresponding schema changes to your database so that it remains compatible with your EF Core model.</span></span>

<span data-ttu-id="a2b08-106">Usare il [reverse engineering][2] se si vuole usare lo schema del database come origine di dati reali.</span><span class="sxs-lookup"><span data-stu-id="a2b08-106">Use [Reverse Engineering][2] if you want your database schema to be the source of truth.</span></span> <span data-ttu-id="a2b08-107">Questo approccio consente di eseguire lo scaffolding di un elemento DbContext e delle classi del tipo di entità decompilando lo schema del database in un modello di EF Core.</span><span class="sxs-lookup"><span data-stu-id="a2b08-107">This approach allows you to scaffold a DbContext and the entity type classes by reverse engineering your database schema into an EF Core model.</span></span>

> [!NOTE]
> <span data-ttu-id="a2b08-108">Anche le [API di creazione ed eliminazione][3] sono in grado di creare lo schema del database dal modello di EF Core.</span><span class="sxs-lookup"><span data-stu-id="a2b08-108">The [create and drop APIs][3] can also create the database schema from your EF Core model.</span></span> <span data-ttu-id="a2b08-109">Tuttavia, sono destinate principalmente ai test e ad altri scenari in cui l'eliminazione del database è accettabile.</span><span class="sxs-lookup"><span data-stu-id="a2b08-109">However, they are primarily for testing, prototyping, and other scenarios where dropping the database is acceptable.</span></span>


  [1]: migrations/index.md
  [2]: scaffolding.md
  [3]: ensure-created.md
