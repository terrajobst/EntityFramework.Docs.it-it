---
title: Schema - Core EF predefinito
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: e6e58473-9f5e-4a1f-ac0f-b87d2cbb667e
ms.technology: entity-framework-core
uid: core/modeling/relational/default-schema
ms.openlocfilehash: 26106deb2d4e35ecf33e97790a83f9af77991aed
ms.sourcegitcommit: 01a75cd483c1943ddd6f82af971f07abde20912e
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 10/27/2017
ms.locfileid: "26052751"
---
# <a name="default-schema"></a><span data-ttu-id="1a4b5-102">Schema predefinito</span><span class="sxs-lookup"><span data-stu-id="1a4b5-102">Default Schema</span></span>

> [!NOTE]  
> <span data-ttu-id="1a4b5-103">La configurazione di questa sezione è applicabile a database relazionali in generale.</span><span class="sxs-lookup"><span data-stu-id="1a4b5-103">The configuration in this section is applicable to relational databases in general.</span></span> <span data-ttu-id="1a4b5-104">I metodi di estensione qui verranno rese disponibili quando si installa un provider di database relazionali (a causa di condiviso *Microsoft.EntityFrameworkCore.Relational* pacchetto).</span><span class="sxs-lookup"><span data-stu-id="1a4b5-104">The extension methods shown here will become available when you install a relational database provider (due to the shared *Microsoft.EntityFrameworkCore.Relational* package).</span></span>

<span data-ttu-id="1a4b5-105">Lo schema predefinito è lo schema del database che verranno creati gli oggetti in se uno schema non è configurato in modo esplicito per l'oggetto.</span><span class="sxs-lookup"><span data-stu-id="1a4b5-105">The default schema is the database schema that objects will be created in if a schema is not explicitly configured for that object.</span></span>

## <a name="conventions"></a><span data-ttu-id="1a4b5-106">Convenzioni</span><span class="sxs-lookup"><span data-stu-id="1a4b5-106">Conventions</span></span>

<span data-ttu-id="1a4b5-107">Per convenzione, il provider del database scegliere lo schema predefinito più appropriato.</span><span class="sxs-lookup"><span data-stu-id="1a4b5-107">By convention, the database provider will choose the most appropriate default schema.</span></span> <span data-ttu-id="1a4b5-108">Ad esempio, Microsoft SQL Server utilizzerà il `dbo` SQLite e lo schema non utilizzerà uno schema (poiché gli schemi non sono supportati in SQLite).</span><span class="sxs-lookup"><span data-stu-id="1a4b5-108">For example, Microsoft SQL Server will use the `dbo` schema and SQLite will not use a schema (since schemas are not supported in SQLite).</span></span>

## <a name="data-annotations"></a><span data-ttu-id="1a4b5-109">Annotazioni dei dati</span><span class="sxs-lookup"><span data-stu-id="1a4b5-109">Data Annotations</span></span>

<span data-ttu-id="1a4b5-110">Non è possibile impostare lo schema predefinito utilizzando le annotazioni dei dati.</span><span class="sxs-lookup"><span data-stu-id="1a4b5-110">You can not set the default schema using Data Annotations.</span></span>

## <a name="fluent-api"></a><span data-ttu-id="1a4b5-111">Microsoft Office Fluent API</span><span class="sxs-lookup"><span data-stu-id="1a4b5-111">Fluent API</span></span>

<span data-ttu-id="1a4b5-112">È possibile utilizzare l'API Fluent per specificare uno schema predefinito.</span><span class="sxs-lookup"><span data-stu-id="1a4b5-112">You can use the Fluent API to specify a default schema.</span></span>

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
