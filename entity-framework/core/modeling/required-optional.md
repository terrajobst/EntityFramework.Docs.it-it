---
title: Proprietà obbligatorie/facoltative-EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: ddaa0a54-9f43-4c34-aae3-f95c96c69842
uid: core/modeling/required-optional
ms.openlocfilehash: 7200cd2eeeba2f22365ef09b1f50edd077240130
ms.sourcegitcommit: cbaa6cc89bd71d5e0bcc891e55743f0e8ea3393b
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/20/2019
ms.locfileid: "71149151"
---
# <a name="required-and-optional-properties"></a><span data-ttu-id="1312b-102">Proprietà obbligatorie e facoltative</span><span class="sxs-lookup"><span data-stu-id="1312b-102">Required and Optional Properties</span></span>

<span data-ttu-id="1312b-103">Una proprietà è considerata facoltativa se è valida per `null`contenerla.</span><span class="sxs-lookup"><span data-stu-id="1312b-103">A property is considered optional if it is valid for it to contain `null`.</span></span> <span data-ttu-id="1312b-104">Se `null` non è un valore valido da assegnare a una proprietà, viene considerata una proprietà obbligatoria.</span><span class="sxs-lookup"><span data-stu-id="1312b-104">If `null` is not a valid value to be assigned to a property then it is considered to be a required property.</span></span>

## <a name="conventions"></a><span data-ttu-id="1312b-105">Convenzioni</span><span class="sxs-lookup"><span data-stu-id="1312b-105">Conventions</span></span>

<span data-ttu-id="1312b-106">Per convenzione, una proprietà il cui tipo .NET può contenere null verrà configurata come`string`facoltativa `byte[]`(, `int?`, e così via).</span><span class="sxs-lookup"><span data-stu-id="1312b-106">By convention, a property whose .NET type can contain null will be configured as optional (`string`, `int?`, `byte[]`, etc.).</span></span> <span data-ttu-id="1312b-107">Le proprietà il cui tipo CLR non può contenere valori null verranno configurate `bool`come richieste (`int`, `decimal`, e così via).</span><span class="sxs-lookup"><span data-stu-id="1312b-107">Properties whose CLR type cannot contain null will be configured as required (`int`, `decimal`, `bool`, etc.).</span></span>

> [!NOTE]  
> <span data-ttu-id="1312b-108">Una proprietà il cui tipo .NET non può contenere null non può essere configurata come facoltativa.</span><span class="sxs-lookup"><span data-stu-id="1312b-108">A property whose .NET type cannot contain null cannot be configured as optional.</span></span> <span data-ttu-id="1312b-109">La proprietà sarà sempre considerata obbligatoria dal Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="1312b-109">The property will always be considered required by Entity Framework.</span></span>

## <a name="data-annotations"></a><span data-ttu-id="1312b-110">Annotazioni dei dati</span><span class="sxs-lookup"><span data-stu-id="1312b-110">Data Annotations</span></span>

<span data-ttu-id="1312b-111">È possibile utilizzare le annotazioni dei dati per indicare che una proprietà è obbligatoria.</span><span class="sxs-lookup"><span data-stu-id="1312b-111">You can use Data Annotations to indicate that a property is required.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/Samples/Required.cs?highlight=14)]

## <a name="fluent-api"></a><span data-ttu-id="1312b-112">API Fluent</span><span class="sxs-lookup"><span data-stu-id="1312b-112">Fluent API</span></span>

<span data-ttu-id="1312b-113">È possibile usare l'API Fluent per indicare che una proprietà è obbligatoria.</span><span class="sxs-lookup"><span data-stu-id="1312b-113">You can use the Fluent API to indicate that a property is required.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Samples/Required.cs?highlight=11-13)]

