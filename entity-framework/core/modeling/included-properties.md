---
title: Inclusione ed esclusione di proprietà - EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: e9dff604-3469-4a05-8f9e-18ac281d82a9
uid: core/modeling/included-properties
ms.openlocfilehash: 022534091bb48e491c8808791a401216a339d7b0
ms.sourcegitcommit: 87fcaba46535aa351db4bdb1231bd14b40e459b9
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/22/2019
ms.locfileid: "59929824"
---
# <a name="including--excluding-properties"></a><span data-ttu-id="06c8a-102">Inclusione ed esclusione di proprietà</span><span class="sxs-lookup"><span data-stu-id="06c8a-102">Including & Excluding Properties</span></span>

<span data-ttu-id="06c8a-103">Tra cui una proprietà nel modello significa che Entity Framework include metadati sulla proprietà e tenterà di leggere e scrivere i valori da e verso il database.</span><span class="sxs-lookup"><span data-stu-id="06c8a-103">Including a property in the model means that EF has metadata about that property and will attempt to read and write values from/to the database.</span></span>

## <a name="conventions"></a><span data-ttu-id="06c8a-104">Convenzioni</span><span class="sxs-lookup"><span data-stu-id="06c8a-104">Conventions</span></span>

<span data-ttu-id="06c8a-105">Per convenzione, le proprietà pubbliche con metodi get e un setter verranno inclusi nel modello.</span><span class="sxs-lookup"><span data-stu-id="06c8a-105">By convention, public properties with a getter and a setter will be included in the model.</span></span>

## <a name="data-annotations"></a><span data-ttu-id="06c8a-106">Annotazioni dei dati</span><span class="sxs-lookup"><span data-stu-id="06c8a-106">Data Annotations</span></span>

<span data-ttu-id="06c8a-107">È possibile utilizzare le annotazioni dei dati per escludere una proprietà dal modello.</span><span class="sxs-lookup"><span data-stu-id="06c8a-107">You can use Data Annotations to exclude a property from the model.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/Samples/IgnoreProperty.cs?highlight=17)]

## <a name="fluent-api"></a><span data-ttu-id="06c8a-108">API Fluent</span><span class="sxs-lookup"><span data-stu-id="06c8a-108">Fluent API</span></span>

<span data-ttu-id="06c8a-109">È possibile usare l'API Fluent per escludere una proprietà dal modello.</span><span class="sxs-lookup"><span data-stu-id="06c8a-109">You can use the Fluent API to exclude a property from the model.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Samples/IgnoreProperty.cs?highlight=12,13)]
