---
title: Migrazioni in ambienti team-EF Core
author: bricelam
ms.author: bricelam
ms.date: 10/30/2017
uid: core/managing-schemas/migrations/teams
ms.openlocfilehash: 6c17c56277821159962884aef72d46c624442e20
ms.sourcegitcommit: 18ab4c349473d94b15b4ca977df12147db07b77f
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/06/2019
ms.locfileid: "73655540"
---
# <a name="migrations-in-team-environments"></a><span data-ttu-id="e6ec2-102">Migrazioni in ambienti team</span><span class="sxs-lookup"><span data-stu-id="e6ec2-102">Migrations in Team Environments</span></span>

<span data-ttu-id="e6ec2-103">Quando si utilizzano migrazioni in ambienti team, prestare particolare attenzione al file di snapshot del modello.</span><span class="sxs-lookup"><span data-stu-id="e6ec2-103">When working with Migrations in team environments, pay extra attention to the model snapshot file.</span></span> <span data-ttu-id="e6ec2-104">Questo file può indicare se la migrazione del proprio team si unisce in modo corretto o se è necessario risolvere un conflitto creando nuovamente la migrazione prima di condividerla.</span><span class="sxs-lookup"><span data-stu-id="e6ec2-104">This file can tell you if your teammate's migration merges cleanly with yours or if you need to resolve a conflict by re-creating your migration before sharing it.</span></span>

## <a name="merging"></a><span data-ttu-id="e6ec2-105">Unione</span><span class="sxs-lookup"><span data-stu-id="e6ec2-105">Merging</span></span>

<span data-ttu-id="e6ec2-106">Quando si esegue il merge delle migrazioni dai colleghi, è possibile che si verifichino conflitti nel file di snapshot del modello.</span><span class="sxs-lookup"><span data-stu-id="e6ec2-106">When you merge migrations from your teammates, you may get conflicts in your model snapshot file.</span></span> <span data-ttu-id="e6ec2-107">Se entrambe le modifiche non sono correlate, l'Unione è semplice e le due migrazioni possono coesistere.</span><span class="sxs-lookup"><span data-stu-id="e6ec2-107">If both changes are unrelated, the merge is trivial and the two migrations can coexist.</span></span> <span data-ttu-id="e6ec2-108">Ad esempio, è possibile che si verifichi un conflitto di merge nella configurazione del tipo di entità Customer simile alla seguente:</span><span class="sxs-lookup"><span data-stu-id="e6ec2-108">For example, you may get a merge conflict in the customer entity type configuration that looks like this:</span></span>

``` output
<<<<<<< Mine
b.Property<bool>("Deactivated");
=======
b.Property<int>("LoyaltyPoints");
>>>>>>> Theirs
```

<span data-ttu-id="e6ec2-109">Poiché entrambe queste proprietà devono esistere nel modello finale, completare il merge aggiungendo entrambe le proprietà.</span><span class="sxs-lookup"><span data-stu-id="e6ec2-109">Since both of these properties need to exist in the final model, complete the merge by adding both properties.</span></span> <span data-ttu-id="e6ec2-110">In molti casi, il sistema di controllo della versione può unire automaticamente tali modifiche.</span><span class="sxs-lookup"><span data-stu-id="e6ec2-110">In many cases, your version control system may automatically merge such changes for you.</span></span>

``` csharp
b.Property<bool>("Deactivated");
b.Property<int>("LoyaltyPoints");
```

<span data-ttu-id="e6ec2-111">In questi casi, la migrazione e la migrazione del proprio team sono indipendenti tra loro.</span><span class="sxs-lookup"><span data-stu-id="e6ec2-111">In these cases, your migration and your teammate's migration are independent of each other.</span></span> <span data-ttu-id="e6ec2-112">Poiché una di esse potrebbe essere applicata per prima, non è necessario apportare altre modifiche alla migrazione prima di condividerla con il team.</span><span class="sxs-lookup"><span data-stu-id="e6ec2-112">Since either of them could be applied first, you don't need to make any additional changes to your migration before sharing it with your team.</span></span>

## <a name="resolving-conflicts"></a><span data-ttu-id="e6ec2-113">Risoluzione dei conflitti</span><span class="sxs-lookup"><span data-stu-id="e6ec2-113">Resolving conflicts</span></span>

<span data-ttu-id="e6ec2-114">In alcuni casi si verifica un vero conflitto durante l'Unione del modello di snapshot del modello.</span><span class="sxs-lookup"><span data-stu-id="e6ec2-114">Sometimes you encounter a true conflict when merging the model snapshot model.</span></span> <span data-ttu-id="e6ec2-115">È possibile, ad esempio, che l'utente e il compagno di squadra abbiano rinominato la stessa proprietà.</span><span class="sxs-lookup"><span data-stu-id="e6ec2-115">For example, you and your teammate may each have renamed the same property.</span></span>

``` output
<<<<<<< Mine
b.Property<string>("Username");
=======
b.Property<string>("Alias");
>>>>>>> Theirs
```

<span data-ttu-id="e6ec2-116">Se si verifica questo tipo di conflitto, risolverlo creando di nuovo la migrazione.</span><span class="sxs-lookup"><span data-stu-id="e6ec2-116">If you encounter this kind of conflict, resolve it by re-creating your migration.</span></span> <span data-ttu-id="e6ec2-117">Attenersi ai passaggi riportati di seguito.</span><span class="sxs-lookup"><span data-stu-id="e6ec2-117">Follow these steps:</span></span>

1. <span data-ttu-id="e6ec2-118">Interrompere l'Unione e il rollback nella directory di lavoro prima del merge</span><span class="sxs-lookup"><span data-stu-id="e6ec2-118">Abort the merge and rollback to your working directory before the merge</span></span>
2. <span data-ttu-id="e6ec2-119">Rimuovere la migrazione (mantenendo però le modifiche del modello)</span><span class="sxs-lookup"><span data-stu-id="e6ec2-119">Remove your migration (but keep your model changes)</span></span>
3. <span data-ttu-id="e6ec2-120">Unisci le modifiche dei colleghi nella directory di lavoro</span><span class="sxs-lookup"><span data-stu-id="e6ec2-120">Merge your teammate's changes into your working directory</span></span>
4. <span data-ttu-id="e6ec2-121">Aggiungere nuovamente la migrazione</span><span class="sxs-lookup"><span data-stu-id="e6ec2-121">Re-add your migration</span></span>

<span data-ttu-id="e6ec2-122">Al termine di questa operazione, le due migrazioni possono essere applicate nell'ordine corretto.</span><span class="sxs-lookup"><span data-stu-id="e6ec2-122">After doing this, the two migrations can be applied in the correct order.</span></span> <span data-ttu-id="e6ec2-123">La migrazione viene applicata per prima, rinominando la colonna in *alias*, successivamente la migrazione la rinomina a *nome utente*.</span><span class="sxs-lookup"><span data-stu-id="e6ec2-123">Their migration is applied first, renaming the column to *Alias*, thereafter your migration renames it to *Username*.</span></span>

<span data-ttu-id="e6ec2-124">La migrazione può essere condivisa in modo sicuro con il resto del team.</span><span class="sxs-lookup"><span data-stu-id="e6ec2-124">Your migration can safely be shared with the rest of the team.</span></span>
