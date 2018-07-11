---
title: Migrazioni in ambienti di Team - EF Core
author: bricelam
ms.author: bricelam
ms.date: 10/30/2017
ms.technology: entity-framework-core
ms.openlocfilehash: cf76df32099c25f33d5d94edf6bccec099cf714a
ms.sourcegitcommit: de491b0988eab91b84dcbd941b7551e597e9c051
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/11/2018
ms.locfileid: "38228380"
---
<a name="migrations-in-team-environments"></a><span data-ttu-id="49b92-102">Migrazioni in ambienti di Team</span><span class="sxs-lookup"><span data-stu-id="49b92-102">Migrations in Team Environments</span></span>
===============================
<span data-ttu-id="49b92-103">Quando si lavora con le migrazioni in ambienti di team, prestare particolare attenzione a file di snapshot del modello.</span><span class="sxs-lookup"><span data-stu-id="49b92-103">When working with Migrations in team environments, pay extra attention to the model snapshot file.</span></span> <span data-ttu-id="49b92-104">Questo file può indicare se la migrazione del collega normalmente unite con quelle dell'utente o se è necessario risolvere un conflitto ricreando la migrazione prima di condividerlo.</span><span class="sxs-lookup"><span data-stu-id="49b92-104">This file can tell you if your teammate's migration merges cleanly with yours or if you need to resolve a conflict by re-creating your migration before sharing it.</span></span>

<a name="merging"></a><span data-ttu-id="49b92-105">Unione</span><span class="sxs-lookup"><span data-stu-id="49b92-105">Merging</span></span>
-------
<span data-ttu-id="49b92-106">Quando si uniscono le migrazioni dai colleghi del team, è possibile ottenere i conflitti nel file snapshot del modello.</span><span class="sxs-lookup"><span data-stu-id="49b92-106">When you merge migrations from your teammates, you may get conflicts in your model snapshot file.</span></span> <span data-ttu-id="49b92-107">Se entrambe le modifiche non sono correlate, l'unione è semplice e due migrazioni possono coesistere.</span><span class="sxs-lookup"><span data-stu-id="49b92-107">If both changes are unrelated, the merge is trivial and the two migrations can coexist.</span></span> <span data-ttu-id="49b92-108">Ad esempio, possibile che si verifichi un conflitto di merge nella configurazione del tipo di entità customer che si presenta come segue:</span><span class="sxs-lookup"><span data-stu-id="49b92-108">For example, you may get a merge conflict in the customer entity type configuration that looks like this:</span></span>

    <<<<<<< Mine
    b.Property<bool>("Deactivated");
    =======
    b.Property<int>("LoyaltyPoints");
    >>>>>>> Theirs

<span data-ttu-id="49b92-109">Poiché entrambe queste proprietà devono esistere nel modello finale, completare il merge mediante l'aggiunta di entrambe le proprietà.</span><span class="sxs-lookup"><span data-stu-id="49b92-109">Since both of these properties need to exist in the final model, complete the merge by adding both properties.</span></span> <span data-ttu-id="49b92-110">In molti casi, il sistema di controllo della versione può unire automaticamente tali modifiche automaticamente.</span><span class="sxs-lookup"><span data-stu-id="49b92-110">In many cases, your version control system may automatically merge such changes for you.</span></span>

``` csharp
b.Property<bool>("Deactivated");
b.Property<int>("LoyaltyPoints");
```

<span data-ttu-id="49b92-111">In questi casi, la migrazione e la migrazione del collega sono indipendenti tra loro.</span><span class="sxs-lookup"><span data-stu-id="49b92-111">In these cases, your migration and your teammate's migration are independent of each other.</span></span> <span data-ttu-id="49b92-112">Poiché una di esse può essere applicata per primo, non è necessario apportare modifiche aggiuntive alla migrazione prima di condividerla con il tuo team.</span><span class="sxs-lookup"><span data-stu-id="49b92-112">Since either of them could be applied first, you don't need to make any additional changes to your migration before sharing it with your team.</span></span>

<a name="resolving-conflicts"></a><span data-ttu-id="49b92-113">Risoluzione dei conflitti</span><span class="sxs-lookup"><span data-stu-id="49b92-113">Resolving conflicts</span></span>
-------------------
<span data-ttu-id="49b92-114">In alcuni casi si verifica un conflitto true quando si uniscono il modello di snapshot del modello.</span><span class="sxs-lookup"><span data-stu-id="49b92-114">Sometimes you encounter a true conflict when merging the model snapshot model.</span></span> <span data-ttu-id="49b92-115">Ad esempio, si e si collega ognuno sia rinominato la stessa proprietà.</span><span class="sxs-lookup"><span data-stu-id="49b92-115">For example, you and your teammate may each have renamed the same property.</span></span>

    <<<<<<< Mine
    b.Property<string>("Username");
    =======
    b.Property<string>("Alias");
    >>>>>>> Theirs

<span data-ttu-id="49b92-116">Se si verifica questo tipo di conflitto, risolvere il problema creando nuovamente la migrazione.</span><span class="sxs-lookup"><span data-stu-id="49b92-116">If you encounter this kind of conflict, resolve it by re-creating your migration.</span></span> <span data-ttu-id="49b92-117">Attenersi ai passaggi riportati di seguito.</span><span class="sxs-lookup"><span data-stu-id="49b92-117">Follow these steps:</span></span>

1. <span data-ttu-id="49b92-118">Interrompere il merge e il rollback alla directory di lavoro prima del merge</span><span class="sxs-lookup"><span data-stu-id="49b92-118">Abort the merge and rollback to your working directory before the merge</span></span>
2. <span data-ttu-id="49b92-119">Rimuovere la migrazione (ma mantenere le modifiche del modello)</span><span class="sxs-lookup"><span data-stu-id="49b92-119">Remove your migration (but keep your model changes)</span></span>
3. <span data-ttu-id="49b92-120">Unire le modifiche del collega in directory di lavoro</span><span class="sxs-lookup"><span data-stu-id="49b92-120">Merge your teammate's changes into your working directory</span></span>
4. <span data-ttu-id="49b92-121">Aggiungere di nuovo la migrazione</span><span class="sxs-lookup"><span data-stu-id="49b92-121">Re-add your migration</span></span>

<span data-ttu-id="49b92-122">Dopo questa operazione, due migrazioni possono essere applicate nell'ordine corretto.</span><span class="sxs-lookup"><span data-stu-id="49b92-122">After doing this, the two migrations can be applied in the correct order.</span></span> <span data-ttu-id="49b92-123">La migrazione viene applicata per primo, la colonna da rinominare *Alias*, in seguito la migrazione viene rinominato da *Username*.</span><span class="sxs-lookup"><span data-stu-id="49b92-123">Their migration is applied first, renaming the column to *Alias*, thereafter your migration renames it to *Username*.</span></span>

<span data-ttu-id="49b92-124">La migrazione può essere condiviso in modo sicuro con il resto del team.</span><span class="sxs-lookup"><span data-stu-id="49b92-124">Your migration can safely be shared with the rest of the team.</span></span>
