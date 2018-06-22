---
title: Salvataggio dei dati - EF Core
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: ef044629-feca-4fd1-a48f-d208daedaf92
ms.technology: entity-framework-core
uid: core/saving/index
ms.openlocfilehash: 9280b9c34b41c0319f918488cd7d28eeceef12e7
ms.sourcegitcommit: 01a75cd483c1943ddd6f82af971f07abde20912e
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 10/27/2017
ms.locfileid: "26048873"
---
# <a name="saving-data"></a><span data-ttu-id="75203-102">Salvataggio dei dati</span><span class="sxs-lookup"><span data-stu-id="75203-102">Saving Data</span></span>

<span data-ttu-id="75203-103">Ogni istanza del contesto include un elemento `ChangeTracker` che è responsabile del rilevamento delle modifiche da scrivere nel database.</span><span class="sxs-lookup"><span data-stu-id="75203-103">Each context instance has a `ChangeTracker` that is responsible for keeping track of changes that need to be written to the database.</span></span> <span data-ttu-id="75203-104">Quando si apportano modifiche alle istanze delle classi di entità, queste modifiche vengono registrate in `ChangeTracker` e quindi scritte nel database quando si chiama `SaveChanges`.</span><span class="sxs-lookup"><span data-stu-id="75203-104">As you make changes to instances of your entity classes, these changes are recorded in the `ChangeTracker` and then written to the database when you call `SaveChanges`.</span></span> <span data-ttu-id="75203-105">Il provider di database è responsabile della conversione delle modifiche in operazioni specifiche del database (ad esempio, i comandi `INSERT`, `UPDATE` e `DELETE` per un database relazionale).</span><span class="sxs-lookup"><span data-stu-id="75203-105">The database provider is responsible for translating the changes into database-specific operations (e.g. `INSERT`, `UPDATE`, and `DELETE` commands for a relational database).</span></span>
