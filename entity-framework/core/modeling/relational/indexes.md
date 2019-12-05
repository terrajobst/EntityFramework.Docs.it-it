---
title: Indici (database relazionale)-EF Core
author: AndriySvyryd
ms.author: ansvyryd
ms.date: 11/05/2019
uid: core/modeling/relational/indexes
ms.openlocfilehash: e14615275f85ee9b6b32d080905465d33963feca
ms.sourcegitcommit: 7a709ce4f77134782393aa802df5ab2718714479
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 12/04/2019
ms.locfileid: "74824568"
---
# <a name="indexes-relational-database"></a><span data-ttu-id="c0012-102">Indici (database relazionale)</span><span class="sxs-lookup"><span data-stu-id="c0012-102">Indexes (Relational Database)</span></span>

> [!NOTE]  
> <span data-ttu-id="c0012-103">La configurazione di questa sezione è applicabile in generale ai database relazionali.</span><span class="sxs-lookup"><span data-stu-id="c0012-103">The configuration in this section is applicable to relational databases in general.</span></span> <span data-ttu-id="c0012-104">I metodi di estensione descritti diventano disponibili quando si installa un provider di database relazionali (a causa del pacchetto *Microsoft.EntityFrameworkCore.Relational* condiviso).</span><span class="sxs-lookup"><span data-stu-id="c0012-104">The extension methods shown here will become available when you install a relational database provider (due to the shared *Microsoft.EntityFrameworkCore.Relational* package).</span></span>

<span data-ttu-id="c0012-105">Un indice in un database relazionale è mappato allo stesso concetto di indice nella parte centrale di Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="c0012-105">An index in a relational database maps to the same concept as an index in the core of Entity Framework.</span></span>

## <a name="conventions"></a><span data-ttu-id="c0012-106">Convenzioni</span><span class="sxs-lookup"><span data-stu-id="c0012-106">Conventions</span></span>

<span data-ttu-id="c0012-107">Per convenzione, gli indici sono denominati `IX_<type name>_<property name>`.</span><span class="sxs-lookup"><span data-stu-id="c0012-107">By convention, indexes are named `IX_<type name>_<property name>`.</span></span> <span data-ttu-id="c0012-108">Per gli indici composti `<property name>` diventa un elenco di nomi di proprietà separato da un carattere di sottolineatura.</span><span class="sxs-lookup"><span data-stu-id="c0012-108">For composite indexes `<property name>` becomes an underscore separated list of property names.</span></span>

## <a name="data-annotations"></a><span data-ttu-id="c0012-109">Annotazioni dei dati</span><span class="sxs-lookup"><span data-stu-id="c0012-109">Data Annotations</span></span>

<span data-ttu-id="c0012-110">Non è possibile configurare gli indici utilizzando le annotazioni dei dati.</span><span class="sxs-lookup"><span data-stu-id="c0012-110">Indexes can not be configured using Data Annotations.</span></span>

## <a name="fluent-api"></a><span data-ttu-id="c0012-111">API Fluent</span><span class="sxs-lookup"><span data-stu-id="c0012-111">Fluent API</span></span>

<span data-ttu-id="c0012-112">È possibile usare l'API Fluent per configurare il nome di un indice.</span><span class="sxs-lookup"><span data-stu-id="c0012-112">You can use the Fluent API to configure the name of an index.</span></span>

[!code-csharp[Main](../../../../samples/core/Modeling/FluentAPI/Relational/IndexName.cs?name=Model&highlight=9)]

<span data-ttu-id="c0012-113">È anche possibile specificare un filtro.</span><span class="sxs-lookup"><span data-stu-id="c0012-113">You can also specify a filter.</span></span>

[!code-csharp[Main](../../../../samples/core/Modeling/FluentAPI/Relational/IndexFilter.cs?name=Model&highlight=9)]

<span data-ttu-id="c0012-114">Quando si usa il provider di SQL Server EF aggiunge un filtro `'IS NOT NULL'` per tutte le colonne nullable che fanno parte di un indice univoco.</span><span class="sxs-lookup"><span data-stu-id="c0012-114">When using the SQL Server provider EF adds an `'IS NOT NULL'` filter for all nullable columns that are part of a unique index.</span></span> <span data-ttu-id="c0012-115">Per eseguire l'override di questa convenzione è possibile specificare un valore `null`.</span><span class="sxs-lookup"><span data-stu-id="c0012-115">To override this convention you can supply a `null` value.</span></span>

[!code-csharp[Main](../../../../samples/core/Modeling/FluentAPI/Relational/IndexNoFilter.cs?name=Model&highlight=10)]

### <a name="include-columns-in-sql-server-indexes"></a><span data-ttu-id="c0012-116">Includi colonne negli indici SQL Server</span><span class="sxs-lookup"><span data-stu-id="c0012-116">Include Columns in SQL Server Indexes</span></span>

<span data-ttu-id="c0012-117">È possibile configurare gli [indici con colonne incluse](https://docs.microsoft.com/sql/relational-databases/indexes/create-indexes-with-included-columns) per migliorare significativamente le prestazioni delle query quando tutte le colonne della query sono incluse nell'indice come colonne chiave o non chiave.</span><span class="sxs-lookup"><span data-stu-id="c0012-117">You can configure [indexes with included columns](https://docs.microsoft.com/sql/relational-databases/indexes/create-indexes-with-included-columns) to significantly improve query performance when all columns in the query are included in the index as key or non-key columns.</span></span>

[!code-csharp[Main](../../../../samples/core/Modeling/FluentAPI/Relational/IndexInclude.cs?name=Model)]
