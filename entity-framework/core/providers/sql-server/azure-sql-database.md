---
title: Provider di database Microsoft SQL Server-opzioni del database SQL di Azure-EF Core
description: Come specificare il livello di servizio e il livello di prestazioni per il database SQL di Azure con il provider di database Entity Framework Core di SQL Server
author: AndriySvyryd
ms.author: ansvyryd
ms.date: 11/05/2019
uid: core/providers/sql-server/azure-sql-database
ms.openlocfilehash: c4f7b91110a0e700ed06130661e611cf45bee05f
ms.sourcegitcommit: 7a709ce4f77134782393aa802df5ab2718714479
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 12/04/2019
ms.locfileid: "74824791"
---
# <a name="specifying-azure-sql-database-options"></a><span data-ttu-id="13276-103">Specifica delle opzioni del database SQL di Azure</span><span class="sxs-lookup"><span data-stu-id="13276-103">Specifying Azure SQL Database Options</span></span>

>[!NOTE]
> <span data-ttu-id="13276-104">Questa API è una novità di EF Core 3,1.</span><span class="sxs-lookup"><span data-stu-id="13276-104">This API is new in EF Core 3.1.</span></span>

<span data-ttu-id="13276-105">Il database SQL di Azure offre [un'ampia gamma di opzioni di prezzo](https://azure.microsoft.com/pricing/details/sql-database/single/) che vengono in genere configurate tramite il portale di Azure.</span><span class="sxs-lookup"><span data-stu-id="13276-105">Azure SQL Database provides [a variety of pricing options](https://azure.microsoft.com/pricing/details/sql-database/single/) that are usually configured through the Azure Portal.</span></span> <span data-ttu-id="13276-106">Tuttavia, se si sta gestendo lo schema utilizzando [EF Core migrazioni](xref:core/managing-schemas/migrations/index) , è possibile specificare le opzioni desiderate nel modello stesso.</span><span class="sxs-lookup"><span data-stu-id="13276-106">However if you are managing the schema using [EF Core migrations](xref:core/managing-schemas/migrations/index) you can specify the desired options in the model itself.</span></span>

<span data-ttu-id="13276-107">È possibile specificare il livello di servizio del database (edizione) utilizzando [HasServiceTier](/dotnet/api/Microsoft.EntityFrameworkCore.SqlServerModelBuilderExtensions.HasServiceTier):</span><span class="sxs-lookup"><span data-stu-id="13276-107">You can specify the service tier of the database (EDITION) using [HasServiceTier](/dotnet/api/Microsoft.EntityFrameworkCore.SqlServerModelBuilderExtensions.HasServiceTier):</span></span>

[!code-csharp[HasServiceTier](../../../../samples/core/SqlServer/AzureDatabase/AzureSqlContext.cs?name=HasServiceTier)]

<span data-ttu-id="13276-108">È possibile specificare le dimensioni massime del database usando [HasDatabaseMaxSize](/dotnet/api/Microsoft.EntityFrameworkCore.SqlServerModelBuilderExtensions.HasDatabaseMaxSize):</span><span class="sxs-lookup"><span data-stu-id="13276-108">You can specify the maximum size of the database using [HasDatabaseMaxSize](/dotnet/api/Microsoft.EntityFrameworkCore.SqlServerModelBuilderExtensions.HasDatabaseMaxSize):</span></span>

[!code-csharp[HasDatabaseMaxSize](../../../../samples/core/SqlServer/AzureDatabase/AzureSqlContext.cs?name=HasDatabaseMaxSize)]

<span data-ttu-id="13276-109">È possibile specificare il livello di prestazioni del database (SERVICE_OBJECTIVE) utilizzando [HasPerformanceLevel](/dotnet/api/Microsoft.EntityFrameworkCore.SqlServerModelBuilderExtensions.HasPerformanceLevel):</span><span class="sxs-lookup"><span data-stu-id="13276-109">You can specify the performance level of the database (SERVICE_OBJECTIVE) using [HasPerformanceLevel](/dotnet/api/Microsoft.EntityFrameworkCore.SqlServerModelBuilderExtensions.HasPerformanceLevel):</span></span>

[!code-csharp[HasPerformanceLevel](../../../../samples/core/SqlServer/AzureDatabase/AzureSqlContext.cs?name=HasPerformanceLevel)]

<span data-ttu-id="13276-110">Usare [HasPerformanceLevelSql](/dotnet/api/Microsoft.EntityFrameworkCore.SqlServerModelBuilderExtensions.HasPerformanceLevelSql) per configurare il pool elastico, perché il valore non è un valore letterale stringa:</span><span class="sxs-lookup"><span data-stu-id="13276-110">Use [HasPerformanceLevelSql](/dotnet/api/Microsoft.EntityFrameworkCore.SqlServerModelBuilderExtensions.HasPerformanceLevelSql) to configure the elastic pool, since the value is not a string literal:</span></span>

[!code-csharp[HasPerformanceLevel](../../../../samples/core/SqlServer/AzureDatabase/AzureSqlContext.cs?name=HasPerformanceLevelSql)]


>[!TIP]
> <span data-ttu-id="13276-111">È possibile trovare tutti i valori supportati nella [documentazione di alter database](/sql/t-sql/statements/alter-database-transact-sql?view=azuresqldb-current).</span><span class="sxs-lookup"><span data-stu-id="13276-111">You can find all the supported values in the [ALTER DATABASE documentation](/sql/t-sql/statements/alter-database-transact-sql?view=azuresqldb-current).</span></span>