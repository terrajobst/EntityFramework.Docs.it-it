---
title: Sequenze-EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 94f81a92-3c72-4e14-912a-f99310374e42
uid: core/modeling/relational/sequences
ms.openlocfilehash: b810caaffa329bb5ad6f3486145d0ade9287eada
ms.sourcegitcommit: 18ab4c349473d94b15b4ca977df12147db07b77f
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/06/2019
ms.locfileid: "73656119"
---
# <a name="sequences"></a><span data-ttu-id="602c7-102">Sequenze</span><span class="sxs-lookup"><span data-stu-id="602c7-102">Sequences</span></span>

> [!NOTE]  
> <span data-ttu-id="602c7-103">La configurazione di questa sezione è applicabile in generale ai database relazionali.</span><span class="sxs-lookup"><span data-stu-id="602c7-103">The configuration in this section is applicable to relational databases in general.</span></span> <span data-ttu-id="602c7-104">I metodi di estensione descritti diventano disponibili quando si installa un provider di database relazionali (a causa del pacchetto *Microsoft.EntityFrameworkCore.Relational* condiviso).</span><span class="sxs-lookup"><span data-stu-id="602c7-104">The extension methods shown here will become available when you install a relational database provider (due to the shared *Microsoft.EntityFrameworkCore.Relational* package).</span></span>

<span data-ttu-id="602c7-105">Una sequenza genera valori numerici sequenziali nel database.</span><span class="sxs-lookup"><span data-stu-id="602c7-105">A sequence generates a sequential numeric values in the database.</span></span> <span data-ttu-id="602c7-106">Le sequenze non sono associate a una tabella specifica.</span><span class="sxs-lookup"><span data-stu-id="602c7-106">Sequences are not associated with a specific table.</span></span>

## <a name="conventions"></a><span data-ttu-id="602c7-107">Convenzioni</span><span class="sxs-lookup"><span data-stu-id="602c7-107">Conventions</span></span>

<span data-ttu-id="602c7-108">Per convenzione, le sequenze non vengono introdotte nel modello.</span><span class="sxs-lookup"><span data-stu-id="602c7-108">By convention, sequences are not introduced in to the model.</span></span>

## <a name="data-annotations"></a><span data-ttu-id="602c7-109">Annotazioni dei dati</span><span class="sxs-lookup"><span data-stu-id="602c7-109">Data Annotations</span></span>

<span data-ttu-id="602c7-110">Non è possibile configurare una sequenza usando le annotazioni dei dati.</span><span class="sxs-lookup"><span data-stu-id="602c7-110">You can not configure a sequence using Data Annotations.</span></span>

## <a name="fluent-api"></a><span data-ttu-id="602c7-111">API Fluent</span><span class="sxs-lookup"><span data-stu-id="602c7-111">Fluent API</span></span>

<span data-ttu-id="602c7-112">È possibile utilizzare l'API Fluent per creare una sequenza nel modello.</span><span class="sxs-lookup"><span data-stu-id="602c7-112">You can use the Fluent API to create a sequence in the model.</span></span>

[!code-csharp[Main](../../../../samples/core/Modeling/FluentAPI/Relational/Sequence.cs?name=Model&highlight=7)]

<span data-ttu-id="602c7-113">È inoltre possibile configurare un aspetto aggiuntivo della sequenza, ad esempio il relativo schema, il valore iniziale e l'incremento.</span><span class="sxs-lookup"><span data-stu-id="602c7-113">You can also configure additional aspect of the sequence, such as its schema, start value, and increment.</span></span>

[!code-csharp[Main](../../../../samples/core/Modeling/FluentAPI/Relational/SequenceConfigured.cs?name=Sequence&highlight=7,8,9)]

<span data-ttu-id="602c7-114">Una volta introdotta una sequenza, è possibile utilizzarla per generare valori per le proprietà nel modello.</span><span class="sxs-lookup"><span data-stu-id="602c7-114">Once a sequence is introduced, you can use it to generate values for properties in your model.</span></span> <span data-ttu-id="602c7-115">Ad esempio, è possibile usare i [valori predefiniti](default-values.md) per inserire il valore successivo dalla sequenza.</span><span class="sxs-lookup"><span data-stu-id="602c7-115">For example, you can use [Default Values](default-values.md) to insert the next value from the sequence.</span></span>

[!code-csharp[Main](../../../../samples/core/Modeling/FluentAPI/Relational/SequenceUsed.cs?name=Default&highlight=13)]
