---
title: Mapping colonne-EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 05a47de9-1078-488e-a823-b516a4208f33
uid: core/modeling/relational/columns
ms.openlocfilehash: eaffc0cc1642f64edabeeef974f5f6de7a23b656
ms.sourcegitcommit: ec196918691f50cd0b21693515b0549f06d9f39c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/23/2019
ms.locfileid: "71197207"
---
# <a name="column-mapping"></a><span data-ttu-id="b52c6-102">Mapping colonne</span><span class="sxs-lookup"><span data-stu-id="b52c6-102">Column Mapping</span></span>

> [!NOTE]  
> <span data-ttu-id="b52c6-103">La configurazione di questa sezione è applicabile in generale ai database relazionali.</span><span class="sxs-lookup"><span data-stu-id="b52c6-103">The configuration in this section is applicable to relational databases in general.</span></span> <span data-ttu-id="b52c6-104">I metodi di estensione descritti diventano disponibili quando si installa un provider di database relazionali (a causa del pacchetto *Microsoft.EntityFrameworkCore.Relational* condiviso).</span><span class="sxs-lookup"><span data-stu-id="b52c6-104">The extension methods shown here will become available when you install a relational database provider (due to the shared *Microsoft.EntityFrameworkCore.Relational* package).</span></span>

<span data-ttu-id="b52c6-105">Il mapping delle colonne consente di identificare i dati della colonna da cui eseguire le query e salvarli nel database.</span><span class="sxs-lookup"><span data-stu-id="b52c6-105">Column mapping identifies which column data should be queried from and saved to in the database.</span></span>

## <a name="conventions"></a><span data-ttu-id="b52c6-106">Convenzioni</span><span class="sxs-lookup"><span data-stu-id="b52c6-106">Conventions</span></span>

<span data-ttu-id="b52c6-107">Per convenzione, ogni proprietà viene configurata per eseguire il mapping a una colonna con lo stesso nome della proprietà.</span><span class="sxs-lookup"><span data-stu-id="b52c6-107">By convention, each property will be set up to map to a column with the same name as the property.</span></span>

## <a name="data-annotations"></a><span data-ttu-id="b52c6-108">Annotazioni dei dati</span><span class="sxs-lookup"><span data-stu-id="b52c6-108">Data Annotations</span></span>

<span data-ttu-id="b52c6-109">È possibile utilizzare le annotazioni dei dati per configurare la colonna a cui viene eseguito il mapping di una proprietà.</span><span class="sxs-lookup"><span data-stu-id="b52c6-109">You can use Data Annotations to configure the column to which a property is mapped.</span></span>

[!code-csharp[Main](../../../../samples/core/Modeling/DataAnnotations/Relational/Column.cs?highlight=13)]

## <a name="fluent-api"></a><span data-ttu-id="b52c6-110">API Fluent</span><span class="sxs-lookup"><span data-stu-id="b52c6-110">Fluent API</span></span>

<span data-ttu-id="b52c6-111">È possibile usare l'API Fluent per configurare la colonna a cui viene eseguito il mapping di una proprietà.</span><span class="sxs-lookup"><span data-stu-id="b52c6-111">You can use the Fluent API to configure the column to which a property is mapped.</span></span>

[!code-csharp[Main](../../../../samples/core/Modeling/FluentAPI/Relational/Column.cs?highlight=11-13)]
