---
title: 'Salvataggio di base: EF Core'
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: 850d842e-3fad-4ef2-be17-053768e97b9e
ms.technology: entity-framework-core
uid: core/saving/basic
ms.openlocfilehash: deead323301dc4a0ee0748b4536ddff4596b99e6
ms.sourcegitcommit: 4997314356118d0d97b04ad82e433e49bb9420a2
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/16/2018
---
# <a name="basic-save"></a>Salvataggio di base

Informazioni su come aggiungere, modificare e rimuovere i dati utilizzando il contesto e classi di entità.

> [!TIP]  
> È possibile visualizzare l'[esempio](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Saving/Saving/Basics/) di questo articolo in GitHub.

## <a name="adding-data"></a>Aggiunta di dati

Utilizzare il *DbSet.Add* metodo per aggiungere nuove istanze di classi di entità. I dati verranno inseriti nel database quando si chiama *SaveChanges*.

[!code-csharp[Main](../../../samples/core/Saving/Saving/Basics/Sample.cs#Add)]

> [!TIP]  
> I metodi Add, Attach e aggiornamento tutte le attività nel grafico completo di entità passato a esso relativo, come descritto nel [i dati correlati](related-data.md) sezione. In alternativa, la proprietà EntityEntry.State può essere utilizzata per impostare lo stato di solo una singola entità. Ad esempio `context.Entry(blog).State = EntityState.Modified`.

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
