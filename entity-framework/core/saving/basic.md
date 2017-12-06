---
title: 'Salvataggio di base: EF Core'
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: 850d842e-3fad-4ef2-be17-053768e97b9e
ms.technology: entity-framework-core
uid: core/saving/basic
ms.openlocfilehash: e99d755b8f7c42b15a73cfdb6a2f8999a405a739
ms.sourcegitcommit: 01a75cd483c1943ddd6f82af971f07abde20912e
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 10/27/2017
---
# <a name="basic-save"></a>Salvataggio di base

Informazioni su come aggiungere, modificare e rimuovere i dati utilizzando il contesto e classi di entità.

> [!TIP]  
> È possibile visualizzare in questo articolo [esempio](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Saving/Saving/Basics/) su GitHub.

## <a name="adding-data"></a>Aggiunta di dati

Utilizzare il *DbSet.Add* metodo per aggiungere nuove istanze di classi di entità. I dati verranno inseriti nel database quando si chiama *SaveChanges*.

[!code-csharp[Main](../../../samples/core/Saving/Saving/Basics/Sample.cs#Add)]

## <a name="updating-data"></a>Aggiornamento di dati

Entity Framework rileva automaticamente le modifiche apportate a un'entità esistente viene rilevata dal contesto. Ciò include le entità che carico query o dal database e le entità aggiunte in precedenza e salvate nel database.

È sufficiente modificare i valori assegnati alle proprietà e quindi chiamare *SaveChanges*.

[!code-csharp[Main](../../../samples/core/Saving/Saving/Basics/Sample.cs#Update)]

## <a name="deleting-data"></a>Eliminazione di dati

Utilizzare il *DbSet.Remove* metodo per eliminare le istanze delle classi di entità.

Se l'entità esiste già nel database, verranno eliminato durante *SaveChanges*. Se l'entità non è ancora stato salvato nel database (ad esempio viene registrato con l'aggiunta) quindi verrà rimossa dal contesto e non sarà più possibile inserito quando *SaveChanges* viene chiamato.

[!code-csharp[Main](../../../samples/core/Saving/Saving/Basics/Sample.cs#Remove)]

## <a name="multiple-operations-in-a-single-savechanges"></a>Più operazioni in un singolo SaveChanges

È possibile combinare più operazioni di aggiornamento/Aggiungi/Rimuovi in una singola chiamata a *SaveChanges*.

> [!NOTE]  
> Per la maggior parte dei provider di database, *SaveChanges* è transazionale. In questo modo tutte le operazioni avrà esito positivo o negativo e le operazioni non verranno mai sinistra parzialmente applicate.

[!code-csharp[Main](../../../samples/core/Saving/Saving/Basics/Sample.cs#MultipleOperations)]
