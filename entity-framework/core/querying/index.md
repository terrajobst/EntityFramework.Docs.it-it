---
title: Esecuzione di query su dati - EF Core
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: 7c65ec3e-46c8-48f8-8232-9e31f96c277b
ms.technology: entity-framework-core
uid: core/querying/index
ms.openlocfilehash: a2dd830b25c64b007a881c105a87b5c631b00266
ms.sourcegitcommit: 01a75cd483c1943ddd6f82af971f07abde20912e
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 10/27/2017
ms.locfileid: "26048883"
---
# <a name="querying-data"></a>Esecuzione di query su dati

Entity Framework Core usa query LINQ (Language Integrated Query) per eseguire query sui dati dal database. Le query LINQ consentono di usare C#, o il linguaggio .NET che si preferisce, per generare query fortemente tipizzate in base alle classi di contesto ed entità derivate. Una rappresentazione della query LINQ viene passata al provider di database, per essere convertita nel linguaggio di query specifico del database, come ad esempio SQL per un database relazionale. Per altre informazioni sulla modalità di elaborazione di una query, vedere [How Queries Work](overview.md) (Funzionamento delle query).
