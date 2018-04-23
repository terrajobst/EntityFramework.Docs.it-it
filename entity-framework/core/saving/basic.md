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
# <a name="basic-save"></a><span data-ttu-id="6878a-102">Salvataggio di base</span><span class="sxs-lookup"><span data-stu-id="6878a-102">Basic Save</span></span>

<span data-ttu-id="6878a-103">Informazioni su come aggiungere, modificare e rimuovere i dati utilizzando il contesto e classi di entità.</span><span class="sxs-lookup"><span data-stu-id="6878a-103">Learn how to add, modify, and remove data using your context and entity classes.</span></span>

> [!TIP]  
> <span data-ttu-id="6878a-104">È possibile visualizzare l'[esempio](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Saving/Saving/Basics/) di questo articolo in GitHub.</span><span class="sxs-lookup"><span data-stu-id="6878a-104">You can view this article's [sample](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Saving/Saving/Basics/) on GitHub.</span></span>

## <a name="adding-data"></a><span data-ttu-id="6878a-105">Aggiunta di dati</span><span class="sxs-lookup"><span data-stu-id="6878a-105">Adding Data</span></span>

<span data-ttu-id="6878a-106">Utilizzare il *DbSet.Add* metodo per aggiungere nuove istanze di classi di entità.</span><span class="sxs-lookup"><span data-stu-id="6878a-106">Use the *DbSet.Add* method to add new instances of your entity classes.</span></span> <span data-ttu-id="6878a-107">I dati verranno inseriti nel database quando si chiama *SaveChanges*.</span><span class="sxs-lookup"><span data-stu-id="6878a-107">The data will be inserted in the database when you call *SaveChanges*.</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Saving/Basics/Sample.cs#Add)]

> [!TIP]  
> <span data-ttu-id="6878a-108">I metodi Add, Attach e aggiornamento tutte le attività nel grafico completo di entità passato a esso relativo, come descritto nel [i dati correlati](related-data.md) sezione.</span><span class="sxs-lookup"><span data-stu-id="6878a-108">The Add, Attach, and Update methods all work on the full graph of entities passed to them, as described in the [Related Data](related-data.md) section.</span></span> <span data-ttu-id="6878a-109">In alternativa, la proprietà EntityEntry.State può essere utilizzata per impostare lo stato di solo una singola entità.</span><span class="sxs-lookup"><span data-stu-id="6878a-109">Alternately, the EntityEntry.State property can be used to set the state of just a single entity.</span></span> <span data-ttu-id="6878a-110">Ad esempio `context.Entry(blog).State = EntityState.Modified`.</span><span class="sxs-lookup"><span data-stu-id="6878a-110">For example, `context.Entry(blog).State = EntityState.Modified`.</span></span>

## <a name="updating-data"></a><span data-ttu-id="6878a-111">Aggiornamento di dati</span><span class="sxs-lookup"><span data-stu-id="6878a-111">Updating Data</span></span>

<span data-ttu-id="6878a-112">Entity Framework rileva automaticamente le modifiche apportate a un'entità esistente viene rilevata dal contesto.</span><span class="sxs-lookup"><span data-stu-id="6878a-112">EF will automatically detect changes made to an existing entity that is tracked by the context.</span></span> <span data-ttu-id="6878a-113">Ciò include le entità che carico query o dal database e le entità aggiunte in precedenza e salvate nel database.</span><span class="sxs-lookup"><span data-stu-id="6878a-113">This includes entities that you load/query from the database, and entities that were previously added and saved to the database.</span></span>

<span data-ttu-id="6878a-114">È sufficiente modificare i valori assegnati alle proprietà e quindi chiamare *SaveChanges*.</span><span class="sxs-lookup"><span data-stu-id="6878a-114">Simply modify the values assigned to properties and then call *SaveChanges*.</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Saving/Basics/Sample.cs#Update)]

## <a name="deleting-data"></a><span data-ttu-id="6878a-115">Eliminazione di dati</span><span class="sxs-lookup"><span data-stu-id="6878a-115">Deleting Data</span></span>

<span data-ttu-id="6878a-116">Utilizzare il *DbSet.Remove* metodo per eliminare le istanze delle classi di entità.</span><span class="sxs-lookup"><span data-stu-id="6878a-116">Use the *DbSet.Remove* method to delete instances of you entity classes.</span></span>

<span data-ttu-id="6878a-117">Se l'entità esiste già nel database, verranno eliminato durante *SaveChanges*.</span><span class="sxs-lookup"><span data-stu-id="6878a-117">If the entity already exists in the database, it will be deleted during *SaveChanges*.</span></span> <span data-ttu-id="6878a-118">Se l'entità non è ancora stato salvato nel database (ad esempio viene registrato con l'aggiunta) quindi verrà rimossa dal contesto e non sarà più possibile inserito quando *SaveChanges* viene chiamato.</span><span class="sxs-lookup"><span data-stu-id="6878a-118">If the entity has not yet been saved to the database (i.e. it is tracked as added) then it will be removed from the context and will no longer be inserted when *SaveChanges* is called.</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Saving/Basics/Sample.cs#Remove)]

## <a name="multiple-operations-in-a-single-savechanges"></a><span data-ttu-id="6878a-119">Più operazioni in un singolo SaveChanges</span><span class="sxs-lookup"><span data-stu-id="6878a-119">Multiple Operations in a single SaveChanges</span></span>

<span data-ttu-id="6878a-120">È possibile combinare più operazioni di aggiornamento/Aggiungi/Rimuovi in una singola chiamata a *SaveChanges*.</span><span class="sxs-lookup"><span data-stu-id="6878a-120">You can combine multiple Add/Update/Remove operations into a single call to *SaveChanges*.</span></span>

> [!NOTE]  
> <span data-ttu-id="6878a-121">Per la maggior parte dei provider di database, *SaveChanges* è transazionale.</span><span class="sxs-lookup"><span data-stu-id="6878a-121">For most database providers, *SaveChanges* is transactional.</span></span> <span data-ttu-id="6878a-122">In questo modo tutte le operazioni avrà esito positivo o negativo e le operazioni non verranno mai sinistra parzialmente applicate.</span><span class="sxs-lookup"><span data-stu-id="6878a-122">This means  all the operations will either succeed or fail and the operations will never be left partially applied.</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Saving/Basics/Sample.cs#MultipleOperations)]
