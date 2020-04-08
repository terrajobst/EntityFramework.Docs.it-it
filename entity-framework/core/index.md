---
title: Panoramica di Entity Framework Core - EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: bc2a2676-bc46-493f-bf49-e3cc97994d57
uid: core/index
ms.openlocfilehash: e6127f775d6bbbdf81debf5519388fe252fe079d
ms.sourcegitcommit: 9b562663679854c37c05fca13d93e180213fb4aa
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 04/07/2020
ms.locfileid: "78412836"
---
# <a name="entity-framework-core"></a>Entity Framework Core

Entity Framework (EF) Core è una versione semplice, estendibile, [open source](https://github.com/aspnet/EntityFrameworkCore) e multipiattaforma della tecnologia di accesso ai dati di grande diffusione Entity Framework.

Entity Framework Core può essere usato come mapper relazionale a oggetti (O/RM), consentendo agli sviluppatori .NET di utilizzare un database con oggetti .NET ed eliminando la necessità della maggior parte del codice di accesso ai dati che è in genere necessario scrivere.

EF Core supporta molti motori di database. Per informazioni dettagliate, vedere [Provider di Database](providers/index.md).

## <a name="the-model"></a>Il modello

Con Entity Framework Core, l'accesso ai dati viene eseguito tramite un modello. Un modello è costituito da classi di entità e da un contesto dell'oggetto che rappresenta una sessione con il database che consente di eseguire una query e salvare i dati. Per altre informazioni, vedere [Creazione di un modello](modeling/index.md).

È possibile generare un modello da un database esistente, scrivere manualmente il codice di un modello in modo che corrisponda al database o usare [migrazioni di Entity Framework](managing-schemas/migrations/index.md) per creare un database a partire dal modello e quindi svilupparlo man mano che il modello cambia nel tempo.

[!code-csharp[Main](../../samples/core/Intro/Model.cs)]

## <a name="querying"></a>Query

Le istanze di classi di entità vengono recuperate dal database tramite LINQ (Language Integrated Query). Per altre informazioni, vedere [Esecuzione di query su dati](querying/index.md).

[!code-csharp[Main](../../samples/core/Intro/Program.cs#Querying)]

## <a name="saving-data"></a>Salvataggio dei dati

I dati vengano creati, eliminati e modificati nel database tramite le istanze di classi di entità. Per altre informazioni, vedere [Salvataggio di dati](saving/index.md).

[!code-csharp[Main](../../samples/core/Intro/Program.cs#SavingData)]

## <a name="next-steps"></a>Passaggi successivi

Per le esercitazioni introduttive, vedere [Introduzione a Entity Framework Core](get-started/index.md).
