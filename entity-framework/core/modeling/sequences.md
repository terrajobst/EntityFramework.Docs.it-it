---
title: Sequenze-EF Core
author: roji
ms.date: 12/18/2019
ms.assetid: 94f81a92-3c72-4e14-912a-f99310374e42
uid: core/modeling/sequences
ms.openlocfilehash: 6343e58dd79837cc69308a070c9814030505d7dd
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78417440"
---
# <a name="sequences"></a><span data-ttu-id="a0462-102">Sequenze</span><span class="sxs-lookup"><span data-stu-id="a0462-102">Sequences</span></span>

> [!NOTE]  
> <span data-ttu-id="a0462-103">Le sequenze sono una funzionalità supportata in genere solo da database relazionali.</span><span class="sxs-lookup"><span data-stu-id="a0462-103">Sequences are a feature typically supported only by relational databases.</span></span> <span data-ttu-id="a0462-104">Se si usa un database non relazionale, ad esempio Cosmos, controllare la documentazione del database per la generazione di valori univoci.</span><span class="sxs-lookup"><span data-stu-id="a0462-104">If you're using a non-relational database such as Cosmos, check your database documentation on generating unique values.</span></span>

<span data-ttu-id="a0462-105">Una sequenza genera valori numerici univoci e sequenziali nel database.</span><span class="sxs-lookup"><span data-stu-id="a0462-105">A sequence generates unique, sequential numeric values in the database.</span></span> <span data-ttu-id="a0462-106">Le sequenze non sono associate a una tabella specifica e possono essere impostate più tabelle per creare valori dalla stessa sequenza.</span><span class="sxs-lookup"><span data-stu-id="a0462-106">Sequences are not associated with a specific table, and multiple tables can be set up to draw values from the same sequence.</span></span>

## <a name="basic-usage"></a><span data-ttu-id="a0462-107">Utilizzo di base</span><span class="sxs-lookup"><span data-stu-id="a0462-107">Basic usage</span></span>

<span data-ttu-id="a0462-108">È possibile impostare una sequenza nel modello e quindi utilizzarla per generare valori per le proprietà:</span><span class="sxs-lookup"><span data-stu-id="a0462-108">You can set up a sequence in the model, and then use it to generate values for properties:</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Sequence.cs?name=Sequence&highlight=3,7)]

<span data-ttu-id="a0462-109">Si noti che l'oggetto SQL specifico usato per generare un valore da una sequenza è specifico del database. L'esempio precedente funziona in SQL Server, ma avrà esito negativo in altri database.</span><span class="sxs-lookup"><span data-stu-id="a0462-109">Note that the specific SQL used to generate a value from a sequence is database-specific; the above example works on SQL Server but will fail on other databases.</span></span> <span data-ttu-id="a0462-110">Per ulteriori informazioni, consultare la documentazione del database specifico.</span><span class="sxs-lookup"><span data-stu-id="a0462-110">Consult your specific database's documentation for more information.</span></span>

## <a name="configuring-sequence-settings"></a><span data-ttu-id="a0462-111">Configurazione delle impostazioni di sequenza</span><span class="sxs-lookup"><span data-stu-id="a0462-111">Configuring sequence settings</span></span>

<span data-ttu-id="a0462-112">È anche possibile configurare altri aspetti della sequenza, ad esempio il relativo schema, il valore iniziale, l'incremento e così via:</span><span class="sxs-lookup"><span data-stu-id="a0462-112">You can also configure additional aspects of the sequence, such as its schema, start value, increment, etc.:</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/SequenceConfiguration.cs?name=SequenceConfiguration&highlight=3-5)]
