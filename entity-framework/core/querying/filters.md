---
title: Filtri di query globali - EF Core
author: anpete
ms.author: anpete
ms.date: 11/03/2017
ms.technology: entity-framework-core
uid: core/querying/filters
ms.openlocfilehash: 6b7a4069917c93015a218c131ff0d0a3920fb69d
ms.sourcegitcommit: 4467032fd6ca223e5965b59912d74cf88a1dd77f
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 08/01/2018
ms.locfileid: "42447640"
---
# <a name="global-query-filters"></a>Filtri di query globali

I filtri di query globali sono predicati di query LINQ (un'espressione booleana in genere passata all'operatore di query *Where* di LINQ) applicati ai tipi di entità nel modello di metadati (solitamente in *OnModelCreating*). Questi filtri vengono automaticamente applicati a qualsiasi query LINQ che interessa questi tipi di entità, inclusi quelli a cui si fa riferimento in modo indiretto, ad esempio tramite l'uso di riferimenti alle proprietà Include o di navigazione diretta. Alcune applicazioni comuni di questa funzionalità sono:

* **Eliminazione temporanea**, un tipo di entità definisce una proprietà *IsDeleted*.
* **Multi-tenancy**, un tipo di entità definisce una proprietà *TenantId*.

## <a name="example"></a>Esempio

L'esempio seguente mostra come usare i filtri di query globali per implementare i comportamenti di query per eliminazione temporanea e multi-tenancy in un modello di blog semplice.

> [!TIP]
> È possibile visualizzare l'[esempio](https://github.com/aspnet/EntityFrameworkCore/tree/master/samples/QueryFilters) di questo articolo in GitHub.

Prima di tutto, definire le entità:

[!code-csharp[Main](../../../efcore-repo/samples/QueryFilters/Program.cs#Entities)]

Si noti la dichiarazione di un campo __tenantId_ per l'entità _Blog_. Questo verrà usato per associare ogni istanza di Blog a un tenant specifico. Viene anche definita una proprietà _IsDeleted_ per il tipo di entità _Post_. Questa proprietà viene usata per tenere traccia dell'eliminazione temporanea di un'istanza di _Post_. L'istanza è contrassegnata come eliminata senza rimuovere fisicamente i dati sottostanti.

A questo punto, configurare i filtri di query in _OnModelCreating_ usando l'API ```HasQueryFilter```.

[!code-csharp[Main](../../../efcore-repo/samples/QueryFilters/Program.cs#Configuration)]

Le espressioni del predicato passate alle chiamate di _HasQueryFilter_ verranno ora applicate automaticamente a tutte le query LINQ per tali tipi.

> [!TIP]
> Si noti l'uso del campo a livello di istanza di DbContext ```_tenantId```, usato per impostare il tenant corrente. I filtri a livello di modello useranno il valore dell'istanza del contesto corretta, ovvero l'istanza che esegue la query.

## <a name="disabling-filters"></a>Disabilitazione dei filtri

È possibile disabilitare i filtri per singole query LINQ usando l'operatore ```IgnoreQueryFilters()```.

[!code-csharp[Main](../../../efcore-repo/samples/QueryFilters/Program.cs#IgnoreFilters)]

## <a name="limitations"></a>Limitazioni

I filtri di query globali presentano le limitazioni seguenti:

* I filtri non possono contenere riferimenti a proprietà di navigazione.
* I filtri possono essere definiti solo per il tipo di entità radice di una gerarchia di ereditarietà.
