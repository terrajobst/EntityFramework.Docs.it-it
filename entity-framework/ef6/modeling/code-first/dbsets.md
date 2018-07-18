---
title: Definizione DbSet - Entity Framework 6
author: divega
ms.date: 2016-10-23
ms.prod: entity-framework
ms.author: divega
ms.manager: avickers
ms.technology: entity-framework-6
ms.topic: article
ms.assetid: 4528a509-ace7-4dfb-8065-1b833f5e03a0
caps.latest.revision: 3
ms.openlocfilehash: 8a495c6ce74d9a346a84b0e10fb28395f4dce07b
ms.sourcegitcommit: 00cb52625b57c1ea339ded1454179fe89b6bcfea
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/16/2018
ms.locfileid: "39123270"
---
# <a name="defining-dbsets"></a><span data-ttu-id="93b9d-102">La definizione di DbSet</span><span class="sxs-lookup"><span data-stu-id="93b9d-102">Defining DbSets</span></span>
<span data-ttu-id="93b9d-103">Quando lo sviluppo con il flusso di lavoro di Code First per definire una classe DbContext derivata che rappresenta la sessione con il database ed espone un elemento DbSet per ogni tipo nel modello.</span><span class="sxs-lookup"><span data-stu-id="93b9d-103">When developing with the Code First workflow you define a derived DbContext that represents your session with the database and exposes a DbSet for each type in your model.</span></span> <span data-ttu-id="93b9d-104">In questo argomento illustra i vari modi, è possibile definire le proprietà DbSet.</span><span class="sxs-lookup"><span data-stu-id="93b9d-104">This topic covers the various ways you can define the DbSet properties.</span></span>  

## <a name="dbcontext-with-dbset-properties"></a><span data-ttu-id="93b9d-105">DbContext con proprietà DbSet</span><span class="sxs-lookup"><span data-stu-id="93b9d-105">DbContext with DbSet properties</span></span>  

<span data-ttu-id="93b9d-106">Il caso comune illustrato negli esempi di Code First è disporre di un oggetto DbContext con proprietà DbSet pubbliche automatica per i tipi di entità del modello.</span><span class="sxs-lookup"><span data-stu-id="93b9d-106">The common case shown in Code First examples is to have a DbContext with public automatic DbSet properties for the entity types of your model.</span></span> <span data-ttu-id="93b9d-107">Ad esempio:</span><span class="sxs-lookup"><span data-stu-id="93b9d-107">For example:</span></span>  

``` csharp
public class BloggingContext : DbContext
{
    public DbSet<Blog> Blogs { get; set; }
    public DbSet<Post> Posts { get; set; }
}
```  

<span data-ttu-id="93b9d-108">Quando utilizzato in modalità Code First, verrà configurato blog e post come tipi di entità, nonché la configurazione di altri tipi raggiungibili da questi.</span><span class="sxs-lookup"><span data-stu-id="93b9d-108">When used in Code First mode, this will configure Blogs and Posts as entity types, as well as configuring other types reachable from these.</span></span> <span data-ttu-id="93b9d-109">Inoltre DbContext chiamerà automaticamente il setter per ognuna di queste proprietà per impostare un'istanza alla classe DbSet appropriata.</span><span class="sxs-lookup"><span data-stu-id="93b9d-109">In addition DbContext will automatically call the setter for each of these properties to set an instance of the appropriate DbSet.</span></span>  

## <a name="dbcontext-with-idbset-properties"></a><span data-ttu-id="93b9d-110">DbContext con proprietà IDbSet</span><span class="sxs-lookup"><span data-stu-id="93b9d-110">DbContext with IDbSet properties</span></span>  

<span data-ttu-id="93b9d-111">Ci sono situazioni, ad esempio quando la creazione di oggetti fittizi o fakes, in cui è più utile dichiarare le proprietà di set usando un'interfaccia.</span><span class="sxs-lookup"><span data-stu-id="93b9d-111">There are situations, such as when creating mocks or fakes, where it is more useful to declare your set properties using an interface.</span></span> <span data-ttu-id="93b9d-112">In questi casi il IDbSet interfaccia può essere utilizzata al posto di DbSet.</span><span class="sxs-lookup"><span data-stu-id="93b9d-112">In such cases the IDbSet interface can be used in place of DbSet.</span></span> <span data-ttu-id="93b9d-113">Ad esempio:</span><span class="sxs-lookup"><span data-stu-id="93b9d-113">For example:</span></span>  

``` csharp
public class BloggingContext : DbContext
{
    public IDbSet<Blog> Blogs { get; set; }
    public IDbSet<Post> Posts { get; set; }
}
```  

<span data-ttu-id="93b9d-114">In questo contesto funziona esattamente come il contesto che utilizza la classe DbSet alle sue proprietà set.</span><span class="sxs-lookup"><span data-stu-id="93b9d-114">This context works in exactly the same way as the context that uses the DbSet class for its set properties.</span></span>  

## <a name="dbcontext-with-read-only-set-properties"></a><span data-ttu-id="93b9d-115">DbContext con le proprietà del set di sola lettura</span><span class="sxs-lookup"><span data-stu-id="93b9d-115">DbContext with read-only set properties</span></span>  

<span data-ttu-id="93b9d-116">Se non si desidera esporre un Setter pubblico per le proprietà DbSet o IDbSet è invece possibile creare le proprietà di sola lettura e creare manualmente le istanze del set.</span><span class="sxs-lookup"><span data-stu-id="93b9d-116">If you do not wish to expose public setters for your DbSet or IDbSet properties you can instead create read-only properties and create the set instances yourself.</span></span> <span data-ttu-id="93b9d-117">Ad esempio:</span><span class="sxs-lookup"><span data-stu-id="93b9d-117">For example:</span></span>  

``` csharp
public class BloggingContext : DbContext
{
    public DbSet<Blog> Blogs
    {
        get { return Set<Blog>(); }
    }

    public DbSet<Post> Posts
    {
        get { return Set<Post>(); }
    }
}
```  

<span data-ttu-id="93b9d-118">Si noti che l'oggetto DbContext memorizza nella cache l'istanza di DbSet restituita dal metodo Set in modo che ognuna di queste proprietà restituirà la stessa istanza ogni volta che viene chiamata.</span><span class="sxs-lookup"><span data-stu-id="93b9d-118">Note that DbContext caches the instance of DbSet returned from the Set method so that each of these properties will return the same instance every time it is called.</span></span>  

<span data-ttu-id="93b9d-119">Individuazione dei tipi di entità per Code First funziona allo stesso modo come avviene per le proprietà con Setter e getter pubblico.</span><span class="sxs-lookup"><span data-stu-id="93b9d-119">Discovery of entity types for Code First works in the same way here as it does for properties with public getters and setters.</span></span>  