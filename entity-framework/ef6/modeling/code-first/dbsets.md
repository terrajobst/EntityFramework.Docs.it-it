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
# <a name="defining-dbsets"></a>La definizione di DbSet
Quando lo sviluppo con il flusso di lavoro di Code First per definire una classe DbContext derivata che rappresenta la sessione con il database ed espone un elemento DbSet per ogni tipo nel modello. In questo argomento illustra i vari modi, è possibile definire le proprietà DbSet.  

## <a name="dbcontext-with-dbset-properties"></a>DbContext con proprietà DbSet  

Il caso comune illustrato negli esempi di Code First è disporre di un oggetto DbContext con proprietà DbSet pubbliche automatica per i tipi di entità del modello. Ad esempio:  

``` csharp
public class BloggingContext : DbContext
{
    public DbSet<Blog> Blogs { get; set; }
    public DbSet<Post> Posts { get; set; }
}
```  

Quando utilizzato in modalità Code First, verrà configurato blog e post come tipi di entità, nonché la configurazione di altri tipi raggiungibili da questi. Inoltre DbContext chiamerà automaticamente il setter per ognuna di queste proprietà per impostare un'istanza alla classe DbSet appropriata.  

## <a name="dbcontext-with-idbset-properties"></a>DbContext con proprietà IDbSet  

Ci sono situazioni, ad esempio quando la creazione di oggetti fittizi o fakes, in cui è più utile dichiarare le proprietà di set usando un'interfaccia. In questi casi il IDbSet interfaccia può essere utilizzata al posto di DbSet. Ad esempio:  

``` csharp
public class BloggingContext : DbContext
{
    public IDbSet<Blog> Blogs { get; set; }
    public IDbSet<Post> Posts { get; set; }
}
```  

In questo contesto funziona esattamente come il contesto che utilizza la classe DbSet alle sue proprietà set.  

## <a name="dbcontext-with-read-only-set-properties"></a>DbContext con le proprietà del set di sola lettura  

Se non si desidera esporre un Setter pubblico per le proprietà DbSet o IDbSet è invece possibile creare le proprietà di sola lettura e creare manualmente le istanze del set. Ad esempio:  

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

Si noti che l'oggetto DbContext memorizza nella cache l'istanza di DbSet restituita dal metodo Set in modo che ognuna di queste proprietà restituirà la stessa istanza ogni volta che viene chiamata.  

Individuazione dei tipi di entità per Code First funziona allo stesso modo come avviene per le proprietà con Setter e getter pubblico.  
