---
title: Indici - EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 4581e7ba-5e7f-452c-9937-0aaf790ba10a
uid: core/modeling/relational/indexes
ms.openlocfilehash: 605b30ce710d9034deab97f695496ec66a576565
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 08/27/2018
ms.locfileid: "42993217"
---
# <a name="indexes"></a><span data-ttu-id="ca7ed-102">Indici</span><span class="sxs-lookup"><span data-stu-id="ca7ed-102">Indexes</span></span>

> [!NOTE]  
> <span data-ttu-id="ca7ed-103">La configurazione di questa sezione è applicabile in generale ai database relazionali.</span><span class="sxs-lookup"><span data-stu-id="ca7ed-103">The configuration in this section is applicable to relational databases in general.</span></span> <span data-ttu-id="ca7ed-104">I metodi di estensione descritti diventano disponibili quando si installa un provider di database relazionali (a causa del pacchetto *Microsoft.EntityFrameworkCore.Relational* condiviso).</span><span class="sxs-lookup"><span data-stu-id="ca7ed-104">The extension methods shown here will become available when you install a relational database provider (due to the shared *Microsoft.EntityFrameworkCore.Relational* package).</span></span>

<span data-ttu-id="ca7ed-105">Un indice in un database relazionale viene eseguito il mapping allo stesso concetto come indice nel nucleo di Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="ca7ed-105">An index in a relational database maps to the same concept as an index in the core of Entity Framework.</span></span>

## <a name="conventions"></a><span data-ttu-id="ca7ed-106">Convenzioni</span><span class="sxs-lookup"><span data-stu-id="ca7ed-106">Conventions</span></span>

<span data-ttu-id="ca7ed-107">Per convenzione, gli indici sono denominati `IX_<type name>_<property name>`.</span><span class="sxs-lookup"><span data-stu-id="ca7ed-107">By convention, indexes are named `IX_<type name>_<property name>`.</span></span> <span data-ttu-id="ca7ed-108">Per gli indici composti `<property name>` diventa un elenco separato da un carattere di sottolineatura di nomi di proprietà.</span><span class="sxs-lookup"><span data-stu-id="ca7ed-108">For composite indexes `<property name>` becomes an underscore separated list of property names.</span></span>

## <a name="data-annotations"></a><span data-ttu-id="ca7ed-109">Annotazioni dei dati</span><span class="sxs-lookup"><span data-stu-id="ca7ed-109">Data Annotations</span></span>

<span data-ttu-id="ca7ed-110">Gli indici non possono essere configurati utilizzando le annotazioni dei dati.</span><span class="sxs-lookup"><span data-stu-id="ca7ed-110">Indexes can not be configured using Data Annotations.</span></span>

## <a name="fluent-api"></a><span data-ttu-id="ca7ed-111">API Fluent</span><span class="sxs-lookup"><span data-stu-id="ca7ed-111">Fluent API</span></span>

<span data-ttu-id="ca7ed-112">È possibile usare l'API Fluent per configurare il nome di un indice.</span><span class="sxs-lookup"><span data-stu-id="ca7ed-112">You can use the Fluent API to configure the name of an index.</span></span>

[!code-csharp[Main](../../../../samples/core/Modeling/FluentAPI/Samples/Relational/IndexName.cs?name=Model&highlight=9)]

<span data-ttu-id="ca7ed-113">È anche possibile specificare un filtro.</span><span class="sxs-lookup"><span data-stu-id="ca7ed-113">You can also specify a filter.</span></span>

[!code-csharp[Main](../../../../samples/core/Modeling/FluentAPI/Samples/Relational/IndexFilter.cs?name=Model&highlight=9)]

<span data-ttu-id="ca7ed-114">Quando si utilizza il provider di SQL Server EF aggiunge filtrare un 'IS NOT NULL' per tutte le colonne nullable che fanno parte di un indice univoco.</span><span class="sxs-lookup"><span data-stu-id="ca7ed-114">When using the SQL Server provider EF adds a 'IS NOT NULL' filter for all nullable columns that are part of a unique index.</span></span> <span data-ttu-id="ca7ed-115">Eseguire l'override di questa convenzione è possibile fornire un `null` valore.</span><span class="sxs-lookup"><span data-stu-id="ca7ed-115">To override this convention you can supply a `null` value.</span></span>

[!code-csharp[Main](../../../../samples/core/Modeling/FluentAPI/Samples/Relational/IndexNoFilter.cs?name=Model&highlight=10)]
