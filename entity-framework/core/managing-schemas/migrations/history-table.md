---
title: Tabella di cronologia migrazioni personalizzate-EF Core
author: bricelam
ms.author: bricelam
ms.date: 11/07/2017
uid: core/managing-schemas/migrations/history-table
ms.openlocfilehash: 0007da7ce3d78fd8f17007ac79a395bb2e6efd86
ms.sourcegitcommit: 18ab4c349473d94b15b4ca977df12147db07b77f
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/06/2019
ms.locfileid: "73655713"
---
# <a name="custom-migrations-history-table"></a><span data-ttu-id="8e910-102">Tabella di cronologia migrazioni personalizzate</span><span class="sxs-lookup"><span data-stu-id="8e910-102">Custom Migrations History Table</span></span>

<span data-ttu-id="8e910-103">Per impostazione predefinita, EF Core tiene traccia di quali migrazioni sono state applicate al database mediante la registrazione in una tabella denominata `__EFMigrationsHistory`.</span><span class="sxs-lookup"><span data-stu-id="8e910-103">By default, EF Core keeps track of which migrations have been applied to the database by recording them in a table named `__EFMigrationsHistory`.</span></span> <span data-ttu-id="8e910-104">Per diversi motivi, potrebbe essere necessario personalizzare questa tabella per adattarla alle proprie esigenze.</span><span class="sxs-lookup"><span data-stu-id="8e910-104">For various reasons, you may want to customize this table to better suit your needs.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="8e910-105">Se si Personalizza la tabella di cronologia delle migrazioni *dopo aver* applicato le migrazioni, si è responsabili dell'aggiornamento della tabella esistente nel database.</span><span class="sxs-lookup"><span data-stu-id="8e910-105">If you customize the Migrations history table *after* applying migrations, you are responsible for updating the existing table in the database.</span></span>

## <a name="schema-and-table-name"></a><span data-ttu-id="8e910-106">Nome schema e tabella</span><span class="sxs-lookup"><span data-stu-id="8e910-106">Schema and table name</span></span>

<span data-ttu-id="8e910-107">È possibile modificare il nome dello schema e della tabella usando il metodo `MigrationsHistoryTable()` in `OnConfiguring()` (o `ConfigureServices()` in ASP.NET Core).</span><span class="sxs-lookup"><span data-stu-id="8e910-107">You can change the schema and table name using the `MigrationsHistoryTable()` method in `OnConfiguring()` (or `ConfigureServices()` on ASP.NET Core).</span></span> <span data-ttu-id="8e910-108">Di seguito è riportato un esempio che usa il provider di EF Core SQL Server.</span><span class="sxs-lookup"><span data-stu-id="8e910-108">Here is an example using the SQL Server EF Core provider.</span></span>

[!code-csharp[Main](../../../../samples/core/Schemas/Migrations/MigrationTableNameContext.cs#TableNameContext)]

## <a name="other-changes"></a><span data-ttu-id="8e910-109">Altre modifiche</span><span class="sxs-lookup"><span data-stu-id="8e910-109">Other changes</span></span>

<span data-ttu-id="8e910-110">Per configurare altri aspetti della tabella, eseguire l'override e sostituire il servizio `IHistoryRepository` specifico del provider.</span><span class="sxs-lookup"><span data-stu-id="8e910-110">To configure additional aspects of the table, override and replace the provider-specific `IHistoryRepository` service.</span></span> <span data-ttu-id="8e910-111">Ecco un esempio di modifica del nome della colonna MigrationId in *ID* in SQL Server.</span><span class="sxs-lookup"><span data-stu-id="8e910-111">Here is an example of changing the MigrationId column name to *Id* on SQL Server.</span></span>

[!code-csharp[Main](../../../../samples/core/Schemas/Migrations/MyHistoryRepository.cs#HistoryRepositoryContext)]

> [!WARNING]
> <span data-ttu-id="8e910-112">`SqlServerHistoryRepository` si trova all'interno di uno spazio dei nomi interno e può cambiare nelle versioni future.</span><span class="sxs-lookup"><span data-stu-id="8e910-112">`SqlServerHistoryRepository` is inside an internal namespace and may change in future releases.</span></span>

[!code-csharp[Main](../../../../samples/core/Schemas/Migrations/MyHistoryRepository.cs#HistoryRepository)]
