---
title: Inclusione & escluse le proprietà-EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: e9dff604-3469-4a05-8f9e-18ac281d82a9
uid: core/modeling/included-properties
ms.openlocfilehash: cd111af891ef0bbaccf515eed0c1991f105bd362
ms.sourcegitcommit: ec196918691f50cd0b21693515b0549f06d9f39c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/23/2019
ms.locfileid: "71197417"
---
# <a name="including--excluding-properties"></a><span data-ttu-id="8a8a5-102">Inclusione & escluse le proprietà</span><span class="sxs-lookup"><span data-stu-id="8a8a5-102">Including & Excluding Properties</span></span>

<span data-ttu-id="8a8a5-103">L'inclusione di una proprietà nel modello significa che EF dispone di metadati relativi a tale proprietà e tenterà di leggere e scrivere valori da/nel database.</span><span class="sxs-lookup"><span data-stu-id="8a8a5-103">Including a property in the model means that EF has metadata about that property and will attempt to read and write values from/to the database.</span></span>

## <a name="conventions"></a><span data-ttu-id="8a8a5-104">Convenzioni</span><span class="sxs-lookup"><span data-stu-id="8a8a5-104">Conventions</span></span>

<span data-ttu-id="8a8a5-105">Per convenzione, le proprietà pubbliche con un getter e un setter verranno incluse nel modello.</span><span class="sxs-lookup"><span data-stu-id="8a8a5-105">By convention, public properties with a getter and a setter will be included in the model.</span></span>

## <a name="data-annotations"></a><span data-ttu-id="8a8a5-106">Annotazioni dei dati</span><span class="sxs-lookup"><span data-stu-id="8a8a5-106">Data Annotations</span></span>

<span data-ttu-id="8a8a5-107">È possibile utilizzare le annotazioni dei dati per escludere una proprietà dal modello.</span><span class="sxs-lookup"><span data-stu-id="8a8a5-107">You can use Data Annotations to exclude a property from the model.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/IgnoreProperty.cs?highlight=17)]

## <a name="fluent-api"></a><span data-ttu-id="8a8a5-108">API Fluent</span><span class="sxs-lookup"><span data-stu-id="8a8a5-108">Fluent API</span></span>

<span data-ttu-id="8a8a5-109">È possibile utilizzare l'API Fluent per escludere una proprietà dal modello.</span><span class="sxs-lookup"><span data-stu-id="8a8a5-109">You can use the Fluent API to exclude a property from the model.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/IgnoreProperty.cs?highlight=12,13)]
