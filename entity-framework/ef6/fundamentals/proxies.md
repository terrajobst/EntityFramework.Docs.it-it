---
title: Utilizzo di proxy - Entity Framework 6
author: divega
ms.date: 2016-10-23
ms.assetid: 869ee4dc-06f1-471d-8e0e-0a1a2bc59c30
ms.openlocfilehash: 7b82dd370e67d1622fc00ff5e5275721d0fc4fe1
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 08/27/2018
ms.locfileid: "42997203"
---
# <a name="working-with-proxies"></a><span data-ttu-id="65928-102">Utilizzo di proxy</span><span class="sxs-lookup"><span data-stu-id="65928-102">Working with proxies</span></span>
<span data-ttu-id="65928-103">Quando si creano istanze dei tipi di entità POCO, Entity Framework crea spesso le istanze di un tipo derivato generata dinamicamente che funge da proxy per l'entità.</span><span class="sxs-lookup"><span data-stu-id="65928-103">When creating instances of POCO entity types, Entity Framework often creates instances of a dynamically generated derived type that acts as a proxy for the entity.</span></span> <span data-ttu-id="65928-104">Questo proxy esegue l'override di alcune proprietà dell'entità da inserire hook per eseguire automaticamente azioni quando si accede alla proprietà virtuale.</span><span class="sxs-lookup"><span data-stu-id="65928-104">This proxy overrides some virtual properties of the entity to insert hooks for performing actions automatically when the property is accessed.</span></span> <span data-ttu-id="65928-105">Ad esempio, questo meccanismo viene usato per supportare il caricamento lazy di relazioni.</span><span class="sxs-lookup"><span data-stu-id="65928-105">For example, this mechanism is used to support lazy loading of relationships.</span></span> <span data-ttu-id="65928-106">Le tecniche illustrate in questo argomento si applicano in modo analogo ai modelli creati con Code First ed EF Designer.</span><span class="sxs-lookup"><span data-stu-id="65928-106">The techniques shown in this topic apply equally to models created with Code First and the EF Designer.</span></span>  

## <a name="disabling-proxy-creation"></a><span data-ttu-id="65928-107">Disabilitare la creazione di proxy</span><span class="sxs-lookup"><span data-stu-id="65928-107">Disabling proxy creation</span></span>  

<span data-ttu-id="65928-108">In alcuni casi è utile impedire la creazione di istanze del proxy di Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="65928-108">Sometimes it is useful to prevent Entity Framework from creating proxy instances.</span></span> <span data-ttu-id="65928-109">Ad esempio, la serializzazione di istanze non proxy è decisamente più facile rispetto la serializzazione di istanze del proxy.</span><span class="sxs-lookup"><span data-stu-id="65928-109">For example, serializing non-proxy instances is considerably easier than serializing proxy instances.</span></span> <span data-ttu-id="65928-110">La creazione di proxy può essere disattivata deselezionando il flag ProxyCreationEnabled.</span><span class="sxs-lookup"><span data-stu-id="65928-110">Proxy creation can be turned off by clearing the ProxyCreationEnabled flag.</span></span> <span data-ttu-id="65928-111">Un'unica posizione è possibile eseguire questa operazione si trova nel costruttore del contesto.</span><span class="sxs-lookup"><span data-stu-id="65928-111">One place you could do this is in the constructor of your context.</span></span> <span data-ttu-id="65928-112">Ad esempio:</span><span class="sxs-lookup"><span data-stu-id="65928-112">For example:</span></span>  

``` csharp
public class BloggingContext : DbContext
{
    public BloggingContext()
    {
        this.Configuration.ProxyCreationEnabled = false;
    }  

    public DbSet<Blog> Blogs { get; set; }
    public DbSet<Post> Posts { get; set; }
}
```  

<span data-ttu-id="65928-113">Si noti che il EF non consentirà di creare proxy per i tipi in cui è presente alcuna operazione per il proxy eseguire operazioni.</span><span class="sxs-lookup"><span data-stu-id="65928-113">Note that the EF will not create proxies for types where there is nothing for the proxy to do.</span></span> <span data-ttu-id="65928-114">Ciò significa che è anche possibile evitare i proxy facendo in modo che i tipi che sono sealed e/o non dispongono di alcuna proprietà virtuale.</span><span class="sxs-lookup"><span data-stu-id="65928-114">This means that you can also avoid proxies by having types that are sealed and/or have no virtual properties.</span></span>  

## <a name="explicitly-creating-an-instance-of-a-proxy"></a><span data-ttu-id="65928-115">In modo esplicito la creazione di un'istanza di un proxy</span><span class="sxs-lookup"><span data-stu-id="65928-115">Explicitly creating an instance of a proxy</span></span>  

<span data-ttu-id="65928-116">Un'istanza del proxy non verrà creata se si crea un'istanza di un'entità utilizzando il nuovo operatore.</span><span class="sxs-lookup"><span data-stu-id="65928-116">A proxy instance will not be created if you create an instance of an entity using the new operator.</span></span> <span data-ttu-id="65928-117">Questo potrebbe non essere un problema, ma se è necessario creare un'istanza del proxy (ad esempio, in modo che funzionerà lazy durante il caricamento o proxy di rilevamento delle modifiche) quindi è possibile farlo usando il metodo Create della DbSet.</span><span class="sxs-lookup"><span data-stu-id="65928-117">This may not be a problem, but if you need to create a proxy instance (for example, so that lazy loading or proxy change tracking will work) then you can do so using the Create method of DbSet.</span></span> <span data-ttu-id="65928-118">Ad esempio:</span><span class="sxs-lookup"><span data-stu-id="65928-118">For example:</span></span>  

``` csharp
using (var context = new BloggingContext())
{
    var blog = context.Blogs.Create();
}
```  

<span data-ttu-id="65928-119">Se si desidera creare un'istanza di un tipo di entità derivati, è possibile utilizzare la versione generica di Create.</span><span class="sxs-lookup"><span data-stu-id="65928-119">The generic version of Create can be used if you want to create an instance of a derived entity type.</span></span> <span data-ttu-id="65928-120">Ad esempio:</span><span class="sxs-lookup"><span data-stu-id="65928-120">For example:</span></span>  

``` csharp
using (var context = new BloggingContext())
{
    var admin = context.Users.Create<Administrator>();
}
```  

<span data-ttu-id="65928-121">Si noti che il metodo Create non aggiungere o collegare l'entità creata nel contesto.</span><span class="sxs-lookup"><span data-stu-id="65928-121">Note that the Create method does not add or attach the created entity to the context.</span></span>  

<span data-ttu-id="65928-122">Si noti che il metodo Create solo creerà un'istanza del tipo di entità se stessa se la creazione di un tipo di proxy per l'entità non avrebbe alcun valore perché verrebbe non eseguono alcuna operazione.</span><span class="sxs-lookup"><span data-stu-id="65928-122">Note that the Create method will just create an instance of the entity type itself if creating a proxy type for the entity would have no value because it would not do anything.</span></span> <span data-ttu-id="65928-123">Ad esempio, se il tipo di entità è bloccato e/o dispone di alcuna proprietà virtuale quindi crea semplicemente creare un'istanza del tipo di entità.</span><span class="sxs-lookup"><span data-stu-id="65928-123">For example, if the entity type is sealed and/or has no virtual properties then Create will just create an instance of the entity type.</span></span>  

## <a name="getting-the-actual-entity-type-from-a-proxy-type"></a><span data-ttu-id="65928-124">Recupero del tipo di entità effettivo da un tipo di proxy</span><span class="sxs-lookup"><span data-stu-id="65928-124">Getting the actual entity type from a proxy type</span></span>  

<span data-ttu-id="65928-125">I tipi proxy hanno nomi con un aspetto simile al seguente:</span><span class="sxs-lookup"><span data-stu-id="65928-125">Proxy types have names that look something like this:</span></span>  

<span data-ttu-id="65928-126">System.Data.Entity.DynamicProxies.Blog_5E43C6C196972BF0754973E48C9C941092D86818CD94005E9A759B70BF6E48E6</span><span class="sxs-lookup"><span data-stu-id="65928-126">System.Data.Entity.DynamicProxies.Blog_5E43C6C196972BF0754973E48C9C941092D86818CD94005E9A759B70BF6E48E6</span></span>  

<span data-ttu-id="65928-127">È possibile trovare il tipo di entità per questo tipo di proxy usando il metodo GetObjectType da ObjectContext.</span><span class="sxs-lookup"><span data-stu-id="65928-127">You can find the entity type for this proxy type using the GetObjectType method from ObjectContext.</span></span> <span data-ttu-id="65928-128">Ad esempio:</span><span class="sxs-lookup"><span data-stu-id="65928-128">For example:</span></span>  

``` csharp
using (var context = new BloggingContext())
{
    var blog = context.Blogs.Find(1);
    var entityType = ObjectContext.GetObjectType(blog.GetType());
}
```  

<span data-ttu-id="65928-129">Si noti che se il tipo passato a GetObjectType è un'istanza di un tipo di entità che non è un tipo di proxy, il tipo di entità viene comunque restituito.</span><span class="sxs-lookup"><span data-stu-id="65928-129">Note that if the type passed to GetObjectType is an instance of an entity type that is not a proxy type then the type of entity is still returned.</span></span> <span data-ttu-id="65928-130">Ciò significa che è sempre possibile usare questo metodo per ottenere il tipo di entità effettivo senza qualsiasi altri controllo per verificare se il tipo è un tipo di proxy o meno.</span><span class="sxs-lookup"><span data-stu-id="65928-130">This means you can always use this method to get the actual entity type without any other checking to see if the type is a proxy type or not.</span></span>  
