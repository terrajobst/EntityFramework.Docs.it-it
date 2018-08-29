---
title: Tipi di dati - EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 9d2e647f-29e4-483b-af00-74269eb06e8f
uid: core/modeling/relational/data-types
ms.openlocfilehash: 9060f66c752be01090ce40be6bf3a32f348ce571
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 08/27/2018
ms.locfileid: "42993521"
---
# <a name="data-types"></a><span data-ttu-id="be2e8-102">Tipi di dati</span><span class="sxs-lookup"><span data-stu-id="be2e8-102">Data Types</span></span>

> [!NOTE]  
> <span data-ttu-id="be2e8-103">La configurazione di questa sezione è applicabile in generale ai database relazionali.</span><span class="sxs-lookup"><span data-stu-id="be2e8-103">The configuration in this section is applicable to relational databases in general.</span></span> <span data-ttu-id="be2e8-104">I metodi di estensione descritti diventano disponibili quando si installa un provider di database relazionali (a causa del pacchetto *Microsoft.EntityFrameworkCore.Relational* condiviso).</span><span class="sxs-lookup"><span data-stu-id="be2e8-104">The extension methods shown here will become available when you install a relational database provider (due to the shared *Microsoft.EntityFrameworkCore.Relational* package).</span></span>

<span data-ttu-id="be2e8-105">Tipo di dati fa riferimento al tipo specifico di database della colonna a cui viene mappata una proprietà.</span><span class="sxs-lookup"><span data-stu-id="be2e8-105">Data type refers to the database specific type of the column to which a property is mapped.</span></span>

## <a name="conventions"></a><span data-ttu-id="be2e8-106">Convenzioni</span><span class="sxs-lookup"><span data-stu-id="be2e8-106">Conventions</span></span>

<span data-ttu-id="be2e8-107">Per convenzione, il provider di database consente di selezionare un tipo di dati in base al tipo CLR della proprietà.</span><span class="sxs-lookup"><span data-stu-id="be2e8-107">By convention, the database provider selects a data type based on the CLR type of the property.</span></span> <span data-ttu-id="be2e8-108">Tiene in considerazione anche altri metadati, ad esempio l'applicazione configurata [lunghezza massima](../max-length.md), se la proprietà fa parte di una chiave primaria e così via.</span><span class="sxs-lookup"><span data-stu-id="be2e8-108">It also takes into account other metadata, such as the configured [Maximum Length](../max-length.md), whether the property is part of a primary key, etc.</span></span>

<span data-ttu-id="be2e8-109">Ad esempio, SQL Server Usa `datetime2(7)` per `DateTime` delle proprietà, e `nvarchar(max)` per `string` delle proprietà (o `nvarchar(450)` per `string` proprietà che vengono usate come chiave).</span><span class="sxs-lookup"><span data-stu-id="be2e8-109">For example, SQL Server uses `datetime2(7)` for `DateTime` properties, and `nvarchar(max)` for `string` properties (or `nvarchar(450)` for `string` properties that are used as a key).</span></span>

## <a name="data-annotations"></a><span data-ttu-id="be2e8-110">Annotazioni dei dati</span><span class="sxs-lookup"><span data-stu-id="be2e8-110">Data Annotations</span></span>

<span data-ttu-id="be2e8-111">È possibile usare le annotazioni dei dati per specificare un tipo di dati esatto per una colonna.</span><span class="sxs-lookup"><span data-stu-id="be2e8-111">You can use Data Annotations to specify an exact data type for a column.</span></span>

<span data-ttu-id="be2e8-112">Ad esempio il codice seguente consente di configurare `Url` sotto forma di stringa non unicode con lunghezza massima pari a `200` e `Rating` come decimale con precisione di `5` e la scalabilità di `2`.</span><span class="sxs-lookup"><span data-stu-id="be2e8-112">For example the following code configures `Url` as a non-unicode string with maximum length of `200` and `Rating` as decimal with precision of `5` and scale of `2`.</span></span>

[!code-csharp[Main](../../../../samples/core/Modeling/DataAnnotations/Samples/Relational/DataType.cs?name=Entities&highlight=4,6)]

## <a name="fluent-api"></a><span data-ttu-id="be2e8-113">API Fluent</span><span class="sxs-lookup"><span data-stu-id="be2e8-113">Fluent API</span></span>

<span data-ttu-id="be2e8-114">È anche possibile usare l'API Fluent per specificare gli stessi tipi di dati per le colonne.</span><span class="sxs-lookup"><span data-stu-id="be2e8-114">You can also use the Fluent API to specify the same data types for the columns.</span></span>

[!code-csharp[Main](../../../../samples/core/Modeling/FluentAPI/Samples/Relational/DataType.cs?name=Model&highlight=9-10)]
