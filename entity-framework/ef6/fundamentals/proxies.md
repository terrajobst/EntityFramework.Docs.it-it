---
title: Utilizzo di proxy - Entity Framework 6
author: divega
ms.date: 10/23/2016
ms.assetid: 869ee4dc-06f1-471d-8e0e-0a1a2bc59c30
ms.openlocfilehash: 8f7d2e8b41ece28efe8d1df3b0679e6e4510d64a
ms.sourcegitcommit: 2b787009fd5be5627f1189ee396e708cd130e07b
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/13/2018
ms.locfileid: "45489817"
---
# <a name="working-with-proxies"></a>Utilizzo di proxy
Quando si creano istanze dei tipi di entità POCO, Entity Framework crea spesso le istanze di un tipo derivato generata dinamicamente che funge da proxy per l'entità. Questo proxy esegue l'override di alcune proprietà dell'entità da inserire hook per eseguire automaticamente azioni quando si accede alla proprietà virtuale. Ad esempio, questo meccanismo viene usato per supportare il caricamento lazy di relazioni. Le tecniche illustrate in questo argomento si applicano in modo analogo ai modelli creati con Code First ed EF Designer.  

## <a name="disabling-proxy-creation"></a>Disabilitare la creazione di proxy  

In alcuni casi è utile impedire la creazione di istanze del proxy di Entity Framework. Ad esempio, la serializzazione di istanze non proxy è decisamente più facile rispetto la serializzazione di istanze del proxy. La creazione di proxy può essere disattivata deselezionando il flag ProxyCreationEnabled. Un'unica posizione è possibile eseguire questa operazione si trova nel costruttore del contesto. Ad esempio:  

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

Si noti che il EF non consentirà di creare proxy per i tipi in cui è presente alcuna operazione per il proxy eseguire operazioni. Ciò significa che è anche possibile evitare i proxy facendo in modo che i tipi che sono sealed e/o non dispongono di alcuna proprietà virtuale.  

## <a name="explicitly-creating-an-instance-of-a-proxy"></a>In modo esplicito la creazione di un'istanza di un proxy  

Un'istanza del proxy non verrà creata se si crea un'istanza di un'entità utilizzando il nuovo operatore. Questo potrebbe non essere un problema, ma se è necessario creare un'istanza del proxy (ad esempio, in modo che funzionerà lazy durante il caricamento o proxy di rilevamento delle modifiche) quindi è possibile farlo usando il metodo Create della DbSet. Ad esempio:  

``` csharp
using (var context = new BloggingContext())
{
    var blog = context.Blogs.Create();
}
```  

Se si desidera creare un'istanza di un tipo di entità derivati, è possibile utilizzare la versione generica di Create. Ad esempio:  

``` csharp
using (var context = new BloggingContext())
{
    var admin = context.Users.Create<Administrator>();
}
```  

Si noti che il metodo Create non aggiungere o collegare l'entità creata nel contesto.  

Si noti che il metodo Create solo creerà un'istanza del tipo di entità se stessa se la creazione di un tipo di proxy per l'entità non avrebbe alcun valore perché verrebbe non eseguono alcuna operazione. Ad esempio, se il tipo di entità è bloccato e/o dispone di alcuna proprietà virtuale quindi crea semplicemente creare un'istanza del tipo di entità.  

## <a name="getting-the-actual-entity-type-from-a-proxy-type"></a>Recupero del tipo di entità effettivo da un tipo di proxy  

I tipi proxy hanno nomi con un aspetto simile al seguente:  

System.Data.Entity.DynamicProxies.Blog_5E43C6C196972BF0754973E48C9C941092D86818CD94005E9A759B70BF6E48E6  

È possibile trovare il tipo di entità per questo tipo di proxy usando il metodo GetObjectType da ObjectContext. Ad esempio:  

``` csharp
using (var context = new BloggingContext())
{
    var blog = context.Blogs.Find(1);
    var entityType = ObjectContext.GetObjectType(blog.GetType());
}
```  

Si noti che se il tipo passato a GetObjectType è un'istanza di un tipo di entità che non è un tipo di proxy, il tipo di entità viene comunque restituito. Ciò significa che è sempre possibile usare questo metodo per ottenere il tipo di entità effettivo senza qualsiasi altri controllo per verificare se il tipo è un tipo di proxy o meno.  
