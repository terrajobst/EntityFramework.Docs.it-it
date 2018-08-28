---
title: Automatica rilevare le modifiche apportate - Entity Framework 6
author: divega
ms.date: 2016-10-23
ms.assetid: a8d1488d-9a54-4623-a76b-e81329ff2756
ms.openlocfilehash: bca33e12674c47cc7e047e85b11746c8e39246b4
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 08/27/2018
ms.locfileid: "42998099"
---
# <a name="automatic-detect-changes"></a><span data-ttu-id="6f542-102">Automatica rilevare le modifiche</span><span class="sxs-lookup"><span data-stu-id="6f542-102">Automatic detect changes</span></span>
<span data-ttu-id="6f542-103">Quando si utilizza la maggior parte delle entità POCO la determinazione di come è stata modificata un'entità (e pertanto gli aggiornamenti devono essere inviati al database) viene gestita dall'algoritmo di rilevamento delle modifiche.</span><span class="sxs-lookup"><span data-stu-id="6f542-103">When using most POCO entities the determination of how an entity has changed (and therefore which updates need to be sent to the database) is handled by the Detect Changes algorithm.</span></span> <span data-ttu-id="6f542-104">Rilevare works modifiche rilevando le differenze tra i valori correnti delle proprietà dell'entità e i valori originali delle proprietà che vengono archiviati in uno snapshot quando l'entità è stata eseguita una query o collegato.</span><span class="sxs-lookup"><span data-stu-id="6f542-104">Detect Changes works by detecting the differences between the current property values of the entity and the original property values that are stored in a snapshot when the entity was queried or attached.</span></span> <span data-ttu-id="6f542-105">Le tecniche illustrate in questo argomento si applicano in modo analogo ai modelli creati con Code First ed EF Designer.</span><span class="sxs-lookup"><span data-stu-id="6f542-105">The techniques shown in this topic apply equally to models created with Code First and the EF Designer.</span></span>  

<span data-ttu-id="6f542-106">Per impostazione predefinita, Entity Framework esegue automaticamente di rilevare le modifiche quando vengono chiamati i metodi seguenti:</span><span class="sxs-lookup"><span data-stu-id="6f542-106">By default, Entity Framework performs Detect Changes automatically when the following methods are called:</span></span>  

- <span data-ttu-id="6f542-107">DbSet.Find</span><span class="sxs-lookup"><span data-stu-id="6f542-107">DbSet.Find</span></span>  
- <span data-ttu-id="6f542-108">DbSet.Local</span><span class="sxs-lookup"><span data-stu-id="6f542-108">DbSet.Local</span></span>  
- <span data-ttu-id="6f542-109">DbSet.Add</span><span class="sxs-lookup"><span data-stu-id="6f542-109">DbSet.Add</span></span>  
- <span data-ttu-id="6f542-110">DbSet.AddRange</span><span class="sxs-lookup"><span data-stu-id="6f542-110">DbSet.AddRange</span></span>
- <span data-ttu-id="6f542-111">DbSet.Remove</span><span class="sxs-lookup"><span data-stu-id="6f542-111">DbSet.Remove</span></span>  
- <span data-ttu-id="6f542-112">DbSet.RemoveRange</span><span class="sxs-lookup"><span data-stu-id="6f542-112">DbSet.RemoveRange</span></span>
- <span data-ttu-id="6f542-113">DbSet.Attach</span><span class="sxs-lookup"><span data-stu-id="6f542-113">DbSet.Attach</span></span>  
- <span data-ttu-id="6f542-114">DbContext.SaveChanges</span><span class="sxs-lookup"><span data-stu-id="6f542-114">DbContext.SaveChanges</span></span>  
- <span data-ttu-id="6f542-115">DbContext.GetValidationErrors</span><span class="sxs-lookup"><span data-stu-id="6f542-115">DbContext.GetValidationErrors</span></span>  
- <span data-ttu-id="6f542-116">DbContext.Entry</span><span class="sxs-lookup"><span data-stu-id="6f542-116">DbContext.Entry</span></span>  
- <span data-ttu-id="6f542-117">DbChangeTracker.Entries</span><span class="sxs-lookup"><span data-stu-id="6f542-117">DbChangeTracker.Entries</span></span>  

## <a name="disabling-automatic-detection-of-changes"></a><span data-ttu-id="6f542-118">Disabilitare il rilevamento automatico delle modifiche</span><span class="sxs-lookup"><span data-stu-id="6f542-118">Disabling automatic detection of changes</span></span>  

<span data-ttu-id="6f542-119">Se si stanno registrando una grande quantità di entità nel contesto e si chiama uno di questi metodi molte volte in un ciclo, è possibile ottenere miglioramenti significativi delle prestazioni disattivando il rilevamento delle modifiche per la durata del ciclo.</span><span class="sxs-lookup"><span data-stu-id="6f542-119">If you are tracking a lot of entities in your context and you call one of these methods many times in a loop, then you may get significant performance improvements by turning off detection of changes for the duration of the loop.</span></span> <span data-ttu-id="6f542-120">Ad esempio:</span><span class="sxs-lookup"><span data-stu-id="6f542-120">For example:</span></span>  

``` csharp
using (var context = new BloggingContext())
{
    try
    {
        context.Configuration.AutoDetectChangesEnabled = false;

        // Make many calls in a loop
        foreach (var blog in aLotOfBlogs)
        {
            context.Blogs.Add(blog);
        }
    }
    finally
    {
        context.Configuration.AutoDetectChangesEnabled = true;
    }
}
```  

<span data-ttu-id="6f542-121">Non dimenticare di riabilitare il rilevamento delle modifiche dopo il ciclo, abbiamo utilizzato una blocco try/finally per garantire sempre viene nuovamente abilitata anche se il codice nel ciclo genera un'eccezione.</span><span class="sxs-lookup"><span data-stu-id="6f542-121">Don’t forget to re-enable detection of changes after the loop — We've used a try/finally to ensure it is always re-enabled even if code in the loop throws an exception.</span></span>  

<span data-ttu-id="6f542-122">Un'alternativa alla disabilitazione e riabilitazione di campo viene lasciato il rilevamento automatico delle modifiche disabilitato in qualsiasi momento e di contesto di chiamata. ChangeTracker.DetectChanges in modo esplicito oppure usare attentamente il proxy di rilevamento delle modifiche.</span><span class="sxs-lookup"><span data-stu-id="6f542-122">An alternative to disabling and re-enabling is to leave automatic detection of changes turned off at all times and either call context.ChangeTracker.DetectChanges explicitly or use change tracking proxies diligently.</span></span> <span data-ttu-id="6f542-123">Entrambe queste opzioni sono avanzate e può facilmente introdurre bug nell'applicazione quindi utilizzarli con cautela.</span><span class="sxs-lookup"><span data-stu-id="6f542-123">Both of these options are advanced and can easily introduce subtle bugs into your application so use them with care.</span></span>  

<span data-ttu-id="6f542-124">Se è necessario aggiungere o rimuovere molti oggetti da un contesto, è consigliabile usare DbSet.AddRange e DbSet.RemoveRange.</span><span class="sxs-lookup"><span data-stu-id="6f542-124">If you need to add or remove many objects from a context, consider using DbSet.AddRange and DbSet.RemoveRange.</span></span> <span data-ttu-id="6f542-125">Questi metodi rilevano automaticamente le modifiche una sola volta dopo il completamento di operazioni add o remove.</span><span class="sxs-lookup"><span data-stu-id="6f542-125">This methods automatically detect changes only once after the add or remove operations are completed.</span></span> 
