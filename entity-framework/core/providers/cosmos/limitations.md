---
title: Provider di Azure Cosmos DB-limitazioni-EF Core
author: AndriySvyryd
ms.author: ansvyryd
ms.date: 09/12/2019
ms.assetid: 9d02a2cd-484e-4687-b8a8-3748ba46dbc9
uid: core/providers/cosmos/limitations
ms.openlocfilehash: 8dcc82a68c89e21ad1902a0bbbce8ebbc3535801
ms.sourcegitcommit: cbaa6cc89bd71d5e0bcc891e55743f0e8ea3393b
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/20/2019
ms.locfileid: "71150809"
---
# <a name="ef-core-azure-cosmos-db-provider-limitations"></a>Limitazioni del provider di Azure Cosmos DB EF Core

Il provider Cosmos presenta alcune limitazioni. Molte di queste limitazioni sono dovute a limitazioni nel motore di database di Cosmos sottostante e non sono specifiche di EF. Ma la cosa più semplice [non è stata ancora implementata](https://github.com/aspnet/EntityFrameworkCore/issues?page=1&q=is%3Aissue+is%3Aopen+Cosmos+in%3Atitle+label%3Atype-enhancement+sort%3Areactions-%2B1-desc).

## <a name="temporary-limitations"></a>Limitazioni temporanee

- Anche se è presente un solo tipo di entità senza ereditarietà mappata a un contenitore, è ancora presente una proprietà Discriminator.
- I tipi di entità con chiavi di partizione non funzionano correttamente in alcuni scenari
- `Include`le chiamate non sono supportate
- `Join`le chiamate non sono supportate

## <a name="azure-cosmos-db-sdk-limitations"></a>Limitazioni di Azure Cosmos DB SDK

- Sono disponibili solo metodi asincroni

> [!WARNING]
> Poiché non sono presenti versioni di sincronizzazione dei metodi di basso livello EF core si basa su, la funzionalità corrispondente è attualmente implementata `.Wait()` chiamando sull'oggetto `Task`restituito. Ciò significa che l'uso di `SaveChanges`metodi come `ToList` o anziché le relative controparti asincrone potrebbe causare un deadlock nell'applicazione

## <a name="azure-cosmos-db-limitations"></a>Limitazioni Azure Cosmos DB

È possibile visualizzare la panoramica completa delle [funzionalità supportate da Azure Cosmos DB](https://docs.microsoft.com/en-us/azure/cosmos-db/modeling-data), che rappresentano le differenze più importanti rispetto a un database relazionale:

- Le transazioni avviate dal client non sono supportate
- Alcune query tra partizioni non sono supportate o molto più lente a seconda degli operatori interessati