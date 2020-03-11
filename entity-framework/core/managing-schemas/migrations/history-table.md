---
title: Tabella di cronologia migrazioni personalizzate-EF Core
author: bricelam
ms.author: bricelam
ms.date: 11/07/2017
uid: core/managing-schemas/migrations/history-table
ms.openlocfilehash: 0007da7ce3d78fd8f17007ac79a395bb2e6efd86
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78416843"
---
# <a name="custom-migrations-history-table"></a>Tabella di cronologia migrazioni personalizzate

Per impostazione predefinita, EF Core tiene traccia di quali migrazioni sono state applicate al database mediante la registrazione in una tabella denominata `__EFMigrationsHistory`. Per diversi motivi, potrebbe essere necessario personalizzare questa tabella per adattarla alle proprie esigenze.

> [!IMPORTANT]
> Se si Personalizza la tabella di cronologia delle migrazioni *dopo aver* applicato le migrazioni, si è responsabili dell'aggiornamento della tabella esistente nel database.

## <a name="schema-and-table-name"></a>Nome schema e tabella

È possibile modificare il nome dello schema e della tabella usando il metodo `MigrationsHistoryTable()` in `OnConfiguring()` (o `ConfigureServices()` in ASP.NET Core). Di seguito è riportato un esempio che usa il provider di EF Core SQL Server.

[!code-csharp[Main](../../../../samples/core/Schemas/Migrations/MigrationTableNameContext.cs#TableNameContext)]

## <a name="other-changes"></a>Altre modifiche

Per configurare altri aspetti della tabella, eseguire l'override e sostituire il servizio `IHistoryRepository` specifico del provider. Ecco un esempio di modifica del nome della colonna MigrationId in *ID* in SQL Server.

[!code-csharp[Main](../../../../samples/core/Schemas/Migrations/MyHistoryRepository.cs#HistoryRepositoryContext)]

> [!WARNING]
> `SqlServerHistoryRepository` si trova all'interno di uno spazio dei nomi interno e può cambiare nelle versioni future.

[!code-csharp[Main](../../../../samples/core/Schemas/Migrations/MyHistoryRepository.cs#HistoryRepository)]
