---
title: Lunghezza massima-EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: c39c5d43-018d-48b8-94f2-b8bc7c686c69
uid: core/modeling/max-length
ms.openlocfilehash: b6f0594fed0c491b4f79dcda5273cdebe9ecf35f
ms.sourcegitcommit: ec196918691f50cd0b21693515b0549f06d9f39c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/23/2019
ms.locfileid: "71197219"
---
# <a name="maximum-length"></a><span data-ttu-id="a6a5f-102">Lunghezza massima</span><span class="sxs-lookup"><span data-stu-id="a6a5f-102">Maximum Length</span></span>

<span data-ttu-id="a6a5f-103">La configurazione di una lunghezza massima fornisce un hint all'archivio dati sul tipo di dati appropriato da usare per una determinata proprietà.</span><span class="sxs-lookup"><span data-stu-id="a6a5f-103">Configuring a maximum length provides a hint to the data store about the appropriate data type to use for a given property.</span></span> <span data-ttu-id="a6a5f-104">La `string` lunghezza massima si applica solo ai tipi di dati della matrice `byte[]`, ad esempio e.</span><span class="sxs-lookup"><span data-stu-id="a6a5f-104">Maximum length only applies to array data types, such as `string` and `byte[]`.</span></span>

> [!NOTE]  
> <span data-ttu-id="a6a5f-105">Entity Framework non esegue alcuna convalida della lunghezza massima prima di passare i dati al provider.</span><span class="sxs-lookup"><span data-stu-id="a6a5f-105">Entity Framework does not do any validation of maximum length before passing data to the provider.</span></span> <span data-ttu-id="a6a5f-106">È necessario che il provider o l'archivio dati venga convalidato se appropriato.</span><span class="sxs-lookup"><span data-stu-id="a6a5f-106">It is up to the provider or data store to validate if appropriate.</span></span> <span data-ttu-id="a6a5f-107">Se, ad esempio, la destinazione è SQL Server, il superamento della lunghezza massima provocherà un'eccezione poiché il tipo di dati della colonna sottostante non consentirà l'archiviazione di dati in eccesso.</span><span class="sxs-lookup"><span data-stu-id="a6a5f-107">For example, when targeting SQL Server, exceeding the maximum length will result in an exception as the data type of the underlying column will not allow excess data to be stored.</span></span>

## <a name="conventions"></a><span data-ttu-id="a6a5f-108">Convenzioni</span><span class="sxs-lookup"><span data-stu-id="a6a5f-108">Conventions</span></span>

<span data-ttu-id="a6a5f-109">Per convenzione, viene lasciata al provider di database per scegliere un tipo di dati appropriato per le proprietà.</span><span class="sxs-lookup"><span data-stu-id="a6a5f-109">By convention, it is left up to the database provider to choose an appropriate data type for properties.</span></span> <span data-ttu-id="a6a5f-110">Per le proprietà con lunghezza, il provider di database sceglierà in genere un tipo di dati che consente la lunghezza dei dati più lunga.</span><span class="sxs-lookup"><span data-stu-id="a6a5f-110">For properties that have a length, the database provider will generally choose a data type that allows for the longest length of data.</span></span> <span data-ttu-id="a6a5f-111">Ad esempio, Microsoft SQL Server `nvarchar(max)` utilizzerà per `string` le proprietà (o `nvarchar(450)` se la colonna viene utilizzata come chiave).</span><span class="sxs-lookup"><span data-stu-id="a6a5f-111">For example, Microsoft SQL Server will use `nvarchar(max)` for `string` properties (or `nvarchar(450)` if the column is used as a key).</span></span>

## <a name="data-annotations"></a><span data-ttu-id="a6a5f-112">Annotazioni dei dati</span><span class="sxs-lookup"><span data-stu-id="a6a5f-112">Data Annotations</span></span>

<span data-ttu-id="a6a5f-113">È possibile utilizzare le annotazioni dei dati per configurare una lunghezza massima per una proprietà.</span><span class="sxs-lookup"><span data-stu-id="a6a5f-113">You can use the Data Annotations to configure a maximum length for a property.</span></span> <span data-ttu-id="a6a5f-114">In questo esempio, la destinazione SQL Server questa operazione comporterebbe `nvarchar(500)` l'utilizzo del tipo di dati.</span><span class="sxs-lookup"><span data-stu-id="a6a5f-114">In this example, targeting SQL Server this would result in the `nvarchar(500)` data type being used.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/MaxLength.cs?highlight=14)]

## <a name="fluent-api"></a><span data-ttu-id="a6a5f-115">API Fluent</span><span class="sxs-lookup"><span data-stu-id="a6a5f-115">Fluent API</span></span>

<span data-ttu-id="a6a5f-116">È possibile usare l'API Fluent per configurare una lunghezza massima per una proprietà.</span><span class="sxs-lookup"><span data-stu-id="a6a5f-116">You can use the Fluent API to configure a maximum length for a property.</span></span> <span data-ttu-id="a6a5f-117">In questo esempio, la destinazione SQL Server questa operazione comporterebbe `nvarchar(500)` l'utilizzo del tipo di dati.</span><span class="sxs-lookup"><span data-stu-id="a6a5f-117">In this example, targeting SQL Server this would result in the `nvarchar(500)` data type being used.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/MaxLength.cs?highlight=11-13)]
