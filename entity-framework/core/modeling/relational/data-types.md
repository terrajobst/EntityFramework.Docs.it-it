---
title: Tipi di dati-EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 9d2e647f-29e4-483b-af00-74269eb06e8f
uid: core/modeling/relational/data-types
ms.openlocfilehash: d667cbcb821e321faed36d097b531c7c55b81248
ms.sourcegitcommit: cbaa6cc89bd71d5e0bcc891e55743f0e8ea3393b
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/20/2019
ms.locfileid: "71149161"
---
# <a name="data-types"></a><span data-ttu-id="f8130-102">Tipi di dati</span><span class="sxs-lookup"><span data-stu-id="f8130-102">Data Types</span></span>

> [!NOTE]  
> <span data-ttu-id="f8130-103">La configurazione di questa sezione è applicabile in generale ai database relazionali.</span><span class="sxs-lookup"><span data-stu-id="f8130-103">The configuration in this section is applicable to relational databases in general.</span></span> <span data-ttu-id="f8130-104">I metodi di estensione descritti diventano disponibili quando si installa un provider di database relazionali (a causa del pacchetto *Microsoft.EntityFrameworkCore.Relational* condiviso).</span><span class="sxs-lookup"><span data-stu-id="f8130-104">The extension methods shown here will become available when you install a relational database provider (due to the shared *Microsoft.EntityFrameworkCore.Relational* package).</span></span>

<span data-ttu-id="f8130-105">Il tipo di dati fa riferimento al tipo specifico del database della colonna a cui viene mappata una proprietà.</span><span class="sxs-lookup"><span data-stu-id="f8130-105">Data type refers to the database specific type of the column to which a property is mapped.</span></span>

## <a name="conventions"></a><span data-ttu-id="f8130-106">Convenzioni</span><span class="sxs-lookup"><span data-stu-id="f8130-106">Conventions</span></span>

<span data-ttu-id="f8130-107">Per convenzione, il provider di database seleziona un tipo di dati basato sul tipo .NET della proprietà.</span><span class="sxs-lookup"><span data-stu-id="f8130-107">By convention, the database provider selects a data type based on the .NET type of the property.</span></span> <span data-ttu-id="f8130-108">Prende in considerazione anche altri metadati, ad esempio la [lunghezza massima](../max-length.md)configurata, se la proprietà fa parte di una chiave primaria e così via.</span><span class="sxs-lookup"><span data-stu-id="f8130-108">It also takes into account other metadata, such as the configured [Maximum Length](../max-length.md), whether the property is part of a primary key, etc.</span></span>

<span data-ttu-id="f8130-109">Ad esempio, SQL Server usa `datetime2(7)` per `DateTime` le proprietà e `nvarchar(max)` per `string` le proprietà ( `nvarchar(450)` o `string` per le proprietà usate come chiave).</span><span class="sxs-lookup"><span data-stu-id="f8130-109">For example, SQL Server uses `datetime2(7)` for `DateTime` properties, and `nvarchar(max)` for `string` properties (or `nvarchar(450)` for `string` properties that are used as a key).</span></span>

## <a name="data-annotations"></a><span data-ttu-id="f8130-110">Annotazioni dei dati</span><span class="sxs-lookup"><span data-stu-id="f8130-110">Data Annotations</span></span>

<span data-ttu-id="f8130-111">È possibile utilizzare le annotazioni dei dati per specificare un tipo di dati esatto per una colonna.</span><span class="sxs-lookup"><span data-stu-id="f8130-111">You can use Data Annotations to specify an exact data type for a column.</span></span>

<span data-ttu-id="f8130-112">`Url` Il codice seguente, ad esempio, consente di configurare come stringa non Unicode con `Rating` `200` lunghezza massima e come decimale con `5` precisione e scala `2`di.</span><span class="sxs-lookup"><span data-stu-id="f8130-112">For example the following code configures `Url` as a non-unicode string with maximum length of `200` and `Rating` as decimal with precision of `5` and scale of `2`.</span></span>

[!code-csharp[Main](../../../../samples/core/Modeling/DataAnnotations/Samples/Relational/DataType.cs?name=Entities&highlight=4,6)]

## <a name="fluent-api"></a><span data-ttu-id="f8130-113">API Fluent</span><span class="sxs-lookup"><span data-stu-id="f8130-113">Fluent API</span></span>

<span data-ttu-id="f8130-114">È anche possibile usare l'API Fluent per specificare gli stessi tipi di dati per le colonne.</span><span class="sxs-lookup"><span data-stu-id="f8130-114">You can also use the Fluent API to specify the same data types for the columns.</span></span>

[!code-csharp[Main](../../../../samples/core/Modeling/FluentAPI/Samples/Relational/DataType.cs?name=Model&highlight=9-10)]
