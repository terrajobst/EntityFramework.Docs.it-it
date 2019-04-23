---
title: Proprietà obbligatorie o facoltative - EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: ddaa0a54-9f43-4c34-aae3-f95c96c69842
uid: core/modeling/required-optional
ms.openlocfilehash: 564d9e62e2ed4f1a52b569630ed4994529e31dc1
ms.sourcegitcommit: 87fcaba46535aa351db4bdb1231bd14b40e459b9
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/22/2019
ms.locfileid: "59929810"
---
# <a name="required-and-optional-properties"></a><span data-ttu-id="d097d-102">Proprietà obbligatorie e facoltative</span><span class="sxs-lookup"><span data-stu-id="d097d-102">Required and Optional Properties</span></span>

<span data-ttu-id="d097d-103">Una proprietà è considerata facoltativa se è valido per contenere `null`.</span><span class="sxs-lookup"><span data-stu-id="d097d-103">A property is considered optional if it is valid for it to contain `null`.</span></span> <span data-ttu-id="d097d-104">Se `null` non è un valore valido da assegnare a una proprietà, quindi viene considerato come una proprietà obbligatoria.</span><span class="sxs-lookup"><span data-stu-id="d097d-104">If `null` is not a valid value to be assigned to a property then it is considered to be a required property.</span></span>

## <a name="conventions"></a><span data-ttu-id="d097d-105">Convenzioni</span><span class="sxs-lookup"><span data-stu-id="d097d-105">Conventions</span></span>

<span data-ttu-id="d097d-106">Per convenzione, una proprietà il cui tipo CLR può contenere null verrà configurata come facoltativi (`string`, `int?`, `byte[]`e così via.).</span><span class="sxs-lookup"><span data-stu-id="d097d-106">By convention, a property whose CLR type can contain null will be configured as optional (`string`, `int?`, `byte[]`, etc.).</span></span> <span data-ttu-id="d097d-107">Le proprietà il cui tipo CLR non può contenere null verranno configurate come richiesto (`int`, `decimal`, `bool`e così via.).</span><span class="sxs-lookup"><span data-stu-id="d097d-107">Properties whose CLR type cannot contain null will be configured as required (`int`, `decimal`, `bool`, etc.).</span></span>

> [!NOTE]  
> <span data-ttu-id="d097d-108">Una proprietà il cui tipo CLR non può contenere null non possa essere configurata come facoltativi.</span><span class="sxs-lookup"><span data-stu-id="d097d-108">A property whose CLR type cannot contain null cannot be configured as optional.</span></span> <span data-ttu-id="d097d-109">La proprietà verrà considerata sempre richiesti da Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="d097d-109">The property will always be considered required by Entity Framework.</span></span>

## <a name="data-annotations"></a><span data-ttu-id="d097d-110">Annotazioni dei dati</span><span class="sxs-lookup"><span data-stu-id="d097d-110">Data Annotations</span></span>

<span data-ttu-id="d097d-111">È possibile usare le annotazioni dei dati per indicare che la proprietà è obbligatoria.</span><span class="sxs-lookup"><span data-stu-id="d097d-111">You can use Data Annotations to indicate that a property is required.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/Samples/Required.cs?highlight=14)]

## <a name="fluent-api"></a><span data-ttu-id="d097d-112">API Fluent</span><span class="sxs-lookup"><span data-stu-id="d097d-112">Fluent API</span></span>

<span data-ttu-id="d097d-113">È possibile usare l'API Fluent per indicare che la proprietà è obbligatoria.</span><span class="sxs-lookup"><span data-stu-id="d097d-113">You can use the Fluent API to indicate that a property is required.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Samples/Required.cs?highlight=11-13)]

