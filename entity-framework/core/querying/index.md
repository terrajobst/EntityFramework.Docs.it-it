---
title: Esecuzione di query su dati - EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 7c65ec3e-46c8-48f8-8232-9e31f96c277b
uid: core/querying/index
ms.openlocfilehash: 51aaa5de11d3fe38b4fba82db8dcb5658088cc27
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 08/27/2018
ms.locfileid: "42993534"
---
# <a name="querying-data"></a>Esecuzione di query su dati

Entity Framework Core usa query LINQ (Language Integrated Query) per eseguire query sui dati dal database. Le query LINQ consentono di usare C#, o il linguaggio .NET che si preferisce, per generare query fortemente tipizzate in base alle classi di contesto ed entità derivate. Una rappresentazione della query LINQ viene passata al provider di database, per essere convertita nel linguaggio di query specifico del database, come ad esempio SQL per un database relazionale. Per altre informazioni sulla modalità di elaborazione di una query, vedere [How Queries Work](overview.md) (Funzionamento delle query).
