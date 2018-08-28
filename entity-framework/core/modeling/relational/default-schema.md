---
title: Schema predefinito - EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: e6e58473-9f5e-4a1f-ac0f-b87d2cbb667e
uid: core/modeling/relational/default-schema
ms.openlocfilehash: 800551bbadd0a9e8b5eb7070a8ccf6ed2407e3d2
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 08/27/2018
ms.locfileid: "42995366"
---
# <a name="default-schema"></a><span data-ttu-id="d60d1-102">Schema predefinito</span><span class="sxs-lookup"><span data-stu-id="d60d1-102">Default Schema</span></span>

> [!NOTE]  
> <span data-ttu-id="d60d1-103">La configurazione di questa sezione è applicabile in generale ai database relazionali.</span><span class="sxs-lookup"><span data-stu-id="d60d1-103">The configuration in this section is applicable to relational databases in general.</span></span> <span data-ttu-id="d60d1-104">I metodi di estensione descritti diventano disponibili quando si installa un provider di database relazionali (a causa del pacchetto *Microsoft.EntityFrameworkCore.Relational* condiviso).</span><span class="sxs-lookup"><span data-stu-id="d60d1-104">The extension methods shown here will become available when you install a relational database provider (due to the shared *Microsoft.EntityFrameworkCore.Relational* package).</span></span>

<span data-ttu-id="d60d1-105">Lo schema predefinito è lo schema del database che verranno creati gli oggetti in se uno schema non è configurato in modo esplicito per quell'oggetto.</span><span class="sxs-lookup"><span data-stu-id="d60d1-105">The default schema is the database schema that objects will be created in if a schema is not explicitly configured for that object.</span></span>

## <a name="conventions"></a><span data-ttu-id="d60d1-106">Convenzioni</span><span class="sxs-lookup"><span data-stu-id="d60d1-106">Conventions</span></span>

<span data-ttu-id="d60d1-107">Per convenzione, il provider di database sceglierà lo schema predefinito più appropriato.</span><span class="sxs-lookup"><span data-stu-id="d60d1-107">By convention, the database provider will choose the most appropriate default schema.</span></span> <span data-ttu-id="d60d1-108">Ad esempio, Microsoft SQL Server userà il `dbo` SQLite e lo schema non userà uno schema (poiché gli schemi non sono supportati in SQLite).</span><span class="sxs-lookup"><span data-stu-id="d60d1-108">For example, Microsoft SQL Server will use the `dbo` schema and SQLite will not use a schema (since schemas are not supported in SQLite).</span></span>

## <a name="data-annotations"></a><span data-ttu-id="d60d1-109">Annotazioni dei dati</span><span class="sxs-lookup"><span data-stu-id="d60d1-109">Data Annotations</span></span>

<span data-ttu-id="d60d1-110">Non è possibile impostare lo schema predefinito tramite le annotazioni dei dati.</span><span class="sxs-lookup"><span data-stu-id="d60d1-110">You can not set the default schema using Data Annotations.</span></span>

## <a name="fluent-api"></a><span data-ttu-id="d60d1-111">API Fluent</span><span class="sxs-lookup"><span data-stu-id="d60d1-111">Fluent API</span></span>

<span data-ttu-id="d60d1-112">È possibile usare l'API Fluent per specificare uno schema predefinito.</span><span class="sxs-lookup"><span data-stu-id="d60d1-112">You can use the Fluent API to specify a default schema.</span></span>

<!-- [!code-csharp[Main](samples/core/relational/Modeling/FluentAPI/Samples/Relational/DefaultSchema.cs?highlight=7)] -->
``` csharp
class MyContext : DbContext
{
    public DbSet<Blog> Blogs { get; set; }

    protected override void OnModelCreating(ModelBuilder modelBuilder)
    {
        modelBuilder.HasDefaultSchema("blogging");
    }
}
```
