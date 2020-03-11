---
title: Novità di EF Core 1.1 - EF Core
author: divega
ms.date: 10/27/2016
ms.assetid: C7FE8C85-445A-4F0C-97EC-CC3F7F1D6F5E
uid: core/what-is-new/ef-core-1.1
ms.openlocfilehash: d582712ed62443318f4b9e209511fb2a557d667e
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78417517"
---
# <a name="new-features-in-ef-core-11"></a><span data-ttu-id="deb45-102">Nuove funzionalità di EF Core 1.1</span><span class="sxs-lookup"><span data-stu-id="deb45-102">New features in EF Core 1.1</span></span>

## <a name="modeling"></a><span data-ttu-id="deb45-103">Modellazione</span><span class="sxs-lookup"><span data-stu-id="deb45-103">Modeling</span></span>

### <a name="field-mapping"></a><span data-ttu-id="deb45-104">Mapping campi</span><span class="sxs-lookup"><span data-stu-id="deb45-104">Field mapping</span></span>

<span data-ttu-id="deb45-105">Consente di configurare un campo sottostante per una proprietà.</span><span class="sxs-lookup"><span data-stu-id="deb45-105">Allows you to configure a backing field for a property.</span></span> <span data-ttu-id="deb45-106">Può essere utile per le proprietà di sola lettura o per i dati che hanno metodi Get/Set invece di una proprietà.</span><span class="sxs-lookup"><span data-stu-id="deb45-106">This can be useful for read-only properties, or data that has Get/Set methods rather than a property.</span></span>

### <a name="mapping-to-memory-optimized-tables-in-sql-server"></a><span data-ttu-id="deb45-107">Mapping a tabelle ottimizzate per la memoria in SQL Server</span><span class="sxs-lookup"><span data-stu-id="deb45-107">Mapping to Memory-Optimized Tables in SQL Server</span></span>

<span data-ttu-id="deb45-108">È possibile specificare che la tabella a cui viene eseguito il mapping di un'entità è ottimizzata per la memoria.</span><span class="sxs-lookup"><span data-stu-id="deb45-108">You can specify that the table an entity is mapped to is memory-optimized.</span></span> <span data-ttu-id="deb45-109">Quando si usa EF Core per creare e gestire un database in base al modello (con le migrazioni o `Database.EnsureCreated()`), per queste entità verrà creata una tabella ottimizzata per la memoria.</span><span class="sxs-lookup"><span data-stu-id="deb45-109">When using EF Core to create and maintain a database based on your model (either with migrations or `Database.EnsureCreated()`), a memory-optimized table will be created for these entities.</span></span>

## <a name="change-tracking"></a><span data-ttu-id="deb45-110">Rilevamento modifiche</span><span class="sxs-lookup"><span data-stu-id="deb45-110">Change tracking</span></span>

### <a name="additional-change-tracking-apis-from-ef6"></a><span data-ttu-id="deb45-111">Altre API di rilevamento modifiche da EF6</span><span class="sxs-lookup"><span data-stu-id="deb45-111">Additional change tracking APIs from EF6</span></span>

<span data-ttu-id="deb45-112">Ad esempio, `Reload`, `GetModifiedProperties`, `GetDatabaseValues` e così via.</span><span class="sxs-lookup"><span data-stu-id="deb45-112">Such as `Reload`, `GetModifiedProperties`, `GetDatabaseValues` etc.</span></span>

## <a name="query"></a><span data-ttu-id="deb45-113">Query</span><span class="sxs-lookup"><span data-stu-id="deb45-113">Query</span></span>

### <a name="explicit-loading"></a><span data-ttu-id="deb45-114">Caricamento esplicito</span><span class="sxs-lookup"><span data-stu-id="deb45-114">Explicit Loading</span></span>

<span data-ttu-id="deb45-115">Consente di attivare il popolamento di una proprietà di navigazione in un'entità caricata in precedenza dal database.</span><span class="sxs-lookup"><span data-stu-id="deb45-115">Allows you to trigger population of a navigation property on an entity that was previously loaded from the database.</span></span>

### <a name="dbsetfind"></a><span data-ttu-id="deb45-116">DbSet.Find</span><span class="sxs-lookup"><span data-stu-id="deb45-116">DbSet.Find</span></span>

<span data-ttu-id="deb45-117">Consente di recuperare facilmente un'entità in base al valore della chiave primaria.</span><span class="sxs-lookup"><span data-stu-id="deb45-117">Provides an easy way to fetch an entity based on its primary key value.</span></span>

## <a name="other"></a><span data-ttu-id="deb45-118">Altri</span><span class="sxs-lookup"><span data-stu-id="deb45-118">Other</span></span>

### <a name="connection-resiliency"></a><span data-ttu-id="deb45-119">Resilienza delle connessioni</span><span class="sxs-lookup"><span data-stu-id="deb45-119">Connection resiliency</span></span>

<span data-ttu-id="deb45-120">Ritenta automaticamente i comandi del database non riusciti.</span><span class="sxs-lookup"><span data-stu-id="deb45-120">Automatically retries failed database commands.</span></span> <span data-ttu-id="deb45-121">È particolarmente utile durante la connessione a SQL Azure, in cui sono comuni gli errori temporanei.</span><span class="sxs-lookup"><span data-stu-id="deb45-121">This is especially useful when connection to SQL Azure, where transient failures are common.</span></span>

### <a name="simplified-service-replacement"></a><span data-ttu-id="deb45-122">Sostituzione dei servizi semplificata</span><span class="sxs-lookup"><span data-stu-id="deb45-122">Simplified service replacement</span></span>

<span data-ttu-id="deb45-123">Facilita la sostituzione dei servizi interni usati da EF.</span><span class="sxs-lookup"><span data-stu-id="deb45-123">Makes it easier to replace internal services that EF uses.</span></span>
