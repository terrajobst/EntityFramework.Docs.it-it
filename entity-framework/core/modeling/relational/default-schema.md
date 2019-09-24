---
title: Schema predefinito-EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: e6e58473-9f5e-4a1f-ac0f-b87d2cbb667e
uid: core/modeling/relational/default-schema
ms.openlocfilehash: ae903ed7200859430aecc55073651236759bc6ce
ms.sourcegitcommit: ec196918691f50cd0b21693515b0549f06d9f39c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/23/2019
ms.locfileid: "71197126"
---
# <a name="default-schema"></a><span data-ttu-id="64497-102">Schema predefinito</span><span class="sxs-lookup"><span data-stu-id="64497-102">Default Schema</span></span>

> [!NOTE]  
> <span data-ttu-id="64497-103">La configurazione di questa sezione è applicabile in generale ai database relazionali.</span><span class="sxs-lookup"><span data-stu-id="64497-103">The configuration in this section is applicable to relational databases in general.</span></span> <span data-ttu-id="64497-104">I metodi di estensione descritti diventano disponibili quando si installa un provider di database relazionali (a causa del pacchetto *Microsoft.EntityFrameworkCore.Relational* condiviso).</span><span class="sxs-lookup"><span data-stu-id="64497-104">The extension methods shown here will become available when you install a relational database provider (due to the shared *Microsoft.EntityFrameworkCore.Relational* package).</span></span>

<span data-ttu-id="64497-105">Lo schema predefinito è lo schema del database in cui verranno creati gli oggetti se uno schema non è configurato in modo esplicito per tale oggetto.</span><span class="sxs-lookup"><span data-stu-id="64497-105">The default schema is the database schema that objects will be created in if a schema is not explicitly configured for that object.</span></span>

## <a name="conventions"></a><span data-ttu-id="64497-106">Convenzioni</span><span class="sxs-lookup"><span data-stu-id="64497-106">Conventions</span></span>

<span data-ttu-id="64497-107">Per convenzione, il provider di database sceglierà lo schema predefinito più appropriato.</span><span class="sxs-lookup"><span data-stu-id="64497-107">By convention, the database provider will choose the most appropriate default schema.</span></span> <span data-ttu-id="64497-108">Ad esempio, Microsoft SQL Server utilizzerà lo `dbo` schema e SQLite non utilizzerà uno schema (poiché gli schemi non sono supportati in SQLite).</span><span class="sxs-lookup"><span data-stu-id="64497-108">For example, Microsoft SQL Server will use the `dbo` schema and SQLite will not use a schema (since schemas are not supported in SQLite).</span></span>

## <a name="data-annotations"></a><span data-ttu-id="64497-109">Annotazioni dei dati</span><span class="sxs-lookup"><span data-stu-id="64497-109">Data Annotations</span></span>

<span data-ttu-id="64497-110">Non è possibile impostare lo schema predefinito utilizzando le annotazioni dei dati.</span><span class="sxs-lookup"><span data-stu-id="64497-110">You can not set the default schema using Data Annotations.</span></span>

## <a name="fluent-api"></a><span data-ttu-id="64497-111">API Fluent</span><span class="sxs-lookup"><span data-stu-id="64497-111">Fluent API</span></span>

<span data-ttu-id="64497-112">Per specificare uno schema predefinito, è possibile usare l'API Fluent.</span><span class="sxs-lookup"><span data-stu-id="64497-112">You can use the Fluent API to specify a default schema.</span></span>

<!-- [!code-csharp[Main](samples/core/relational/Modeling/FluentAPI/Relational/DefaultSchema.cs?highlight=7)] -->
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
