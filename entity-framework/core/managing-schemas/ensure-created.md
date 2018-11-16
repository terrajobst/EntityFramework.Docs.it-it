---
title: Creare ed eliminare le API - EF Core
author: bricelam
ms.author: bricelam
ms.date: 11/7/2018
ms.openlocfilehash: 40d9e3aa0aba1bf2bc341f01dd815ed7cb7b48fa
ms.sourcegitcommit: b3c2b34d5f006ee3b41d6668f16fe7dcad1b4317
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/15/2018
ms.locfileid: "51688629"
---
# <a name="create-and-drop-apis"></a>Creare ed eliminare le API

I metodi EnsureCreated ed EnsureDeleted forniscono una semplice alternativa [migrazioni](migrations/index.md) per la gestione dello schema del database. Questi metodi sono utili negli scenari quando i dati sono temporanei e possono essere eliminati quando viene modificato lo schema. Ad esempio durante la creazione di prototipi, nei test o per le cache locale.

Alcuni provider (in particolare quelli non relazionali) non supporta le migrazioni. Per questi provider, EnsureCreated è spesso il modo più semplice per inizializzare lo schema del database.

> [!WARNING]
> Le migrazioni ed EnsureCreated non funzionano bene insieme. Se si usano le migrazioni, non usare EnsureCreated per inizializzare lo schema.

Transizione da EnsureCreated alle migrazioni non è un'esperienza senza problemi. Il modo più semplice per farlo è eliminare il database e ricrearlo con le migrazioni. Se si prevede di usare le migrazioni in futuro, è consigliabile iniziare le migrazioni anziché EnsureCreated.

## <a name="ensuredeleted"></a>EnsureDeleted

Se esiste, il metodo EnsureDeleted eliminerà il database. Se non si dispone delle autorizzazioni appropriate, viene generata un'eccezione.

``` csharp
// Drop the database if it exists
dbContext.Database.EnsureDeleted();
```

## <a name="ensurecreated"></a>EnsureCreated

EnsureCreated creerà il database se non esiste e inizializzare lo schema del database. Se sono presenti eventuali tabelle (incluse le tabelle per un'altra classe DbContext), lo schema non inizializzato.

``` csharp
// Create the database if it doesn't exist
dbContext.Database.EnsureCreated();
```

> [!TIP]
> Sono disponibili anche versioni asincrone di questi metodi.

## <a name="sql-script"></a>Script SQL

Per ottenere il codice SQL utilizzato da EnsureCreated, è possibile usare il metodo GenerateCreateScript.

``` csharp
var sql = dbContext.Database.GenerateCreateScript();
```

## <a name="multiple-dbcontext-classes"></a>Più classi di DbContext

EnsureCreated funziona solo quando alcun tabelle non sono presenti nel database. Se necessario, è possibile scrivere il proprio controllo per verificare se lo schema deve essere inizializzata e usare il servizio IRelationalDatabaseCreator sottostante per inizializzare lo schema.

``` csharp
// TODO: Check whether the schema needs to be initialized

// Initialize the schema for this DbContext
var databaseCreator = dbContext.GetService<IRelationalDatabaseCreator>();
databaseCreator.CreateTables();
```
