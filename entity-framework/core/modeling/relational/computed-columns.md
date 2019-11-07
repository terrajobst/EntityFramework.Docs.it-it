---
title: Colonne calcolate-EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: e9d81f06-805d-45c9-97c2-3546df654829
uid: core/modeling/relational/computed-columns
ms.openlocfilehash: 4e92ed6d785f3b7bdf54015101bdcddb9e4e0e1c
ms.sourcegitcommit: 18ab4c349473d94b15b4ca977df12147db07b77f
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/06/2019
ms.locfileid: "73655924"
---
# <a name="computed-columns"></a><span data-ttu-id="d179b-102">Colonne calcolate</span><span class="sxs-lookup"><span data-stu-id="d179b-102">Computed Columns</span></span>

> [!NOTE]  
> <span data-ttu-id="d179b-103">La configurazione di questa sezione è applicabile in generale ai database relazionali.</span><span class="sxs-lookup"><span data-stu-id="d179b-103">The configuration in this section is applicable to relational databases in general.</span></span> <span data-ttu-id="d179b-104">I metodi di estensione descritti diventano disponibili quando si installa un provider di database relazionali (a causa del pacchetto *Microsoft.EntityFrameworkCore.Relational* condiviso).</span><span class="sxs-lookup"><span data-stu-id="d179b-104">The extension methods shown here will become available when you install a relational database provider (due to the shared *Microsoft.EntityFrameworkCore.Relational* package).</span></span>

<span data-ttu-id="d179b-105">Una colonna calcolata è una colonna il cui valore viene calcolato nel database.</span><span class="sxs-lookup"><span data-stu-id="d179b-105">A computed column is a column whose value is calculated in the database.</span></span> <span data-ttu-id="d179b-106">Per calcolare il valore di una colonna calcolata, è possibile utilizzare altre colonne della tabella.</span><span class="sxs-lookup"><span data-stu-id="d179b-106">A computed column can use other columns in the table to calculate its value.</span></span>

## <a name="conventions"></a><span data-ttu-id="d179b-107">Convenzioni</span><span class="sxs-lookup"><span data-stu-id="d179b-107">Conventions</span></span>

<span data-ttu-id="d179b-108">Per convenzione, le colonne calcolate non vengono create nel modello.</span><span class="sxs-lookup"><span data-stu-id="d179b-108">By convention, computed columns are not created in the model.</span></span>

## <a name="data-annotations"></a><span data-ttu-id="d179b-109">Annotazioni dei dati</span><span class="sxs-lookup"><span data-stu-id="d179b-109">Data Annotations</span></span>

<span data-ttu-id="d179b-110">Le colonne calcolate non possono essere configurate con le annotazioni dei dati.</span><span class="sxs-lookup"><span data-stu-id="d179b-110">Computed columns can not be configured with Data Annotations.</span></span>

## <a name="fluent-api"></a><span data-ttu-id="d179b-111">API Fluent</span><span class="sxs-lookup"><span data-stu-id="d179b-111">Fluent API</span></span>

<span data-ttu-id="d179b-112">È possibile utilizzare l'API Fluent per specificare che una proprietà deve essere mappata a una colonna calcolata.</span><span class="sxs-lookup"><span data-stu-id="d179b-112">You can use the Fluent API to specify that a property should map to a computed column.</span></span>

[!code-csharp[Main](../../../../samples/core/Modeling/FluentAPI/Relational/ComputedColumn.cs?name=ComputedColumn&highlight=9)]
