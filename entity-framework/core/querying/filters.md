---
title: Filtri di Query globale - Core EF
author: anpete
ms.author: anpete
ms.date: 11/03/2017
ms.technology: entity-framework-core
uid: core/querying/filters
ms.openlocfilehash: 4e3c3c99d155f69e00fed99c415f519808ea1a68
ms.sourcegitcommit: 6e379265e4f087fb7cf180c824722c81750554dc
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/15/2017
---
# <a name="global-query-filters"></a>Filtri di Query globale

I filtri di query globale sono predicati di query LINQ (un'espressione booleana in genere passato a LINQ *in* operatore query) applicato ai tipi di entità nel modello di metadati (in genere in *OnModelCreating*). Tali filtri vengono applicati automaticamente a tutte le query LINQ che interessa i tipi di entità, inclusi riferimenti a proprietà di navigazione a cui viene fatto riferimento indirettamente, ad esempio tramite l'utilizzo di inclusione o diretto di tipi di entità. Vengono elencate alcune applicazioni di questa funzionalità:

* **Eliminare temporaneamente** -definisce un tipo di entità un *IsDeleted* proprietà.
* **Multi-tenancy** -definisce un tipo di entità un *TenantId* proprietà.

## <a name="example"></a>Esempio

Nell'esempio seguente viene illustrato come utilizzare i filtri di Query globale per implementare i comportamenti di soft-delete e multi-tenancy query in un modello semplice blog.

> [!TIP]
> È possibile visualizzare in questo articolo [esempio](https://github.com/aspnet/EntityFrameworkCore/tree/dev/samples/QueryFilters) su GitHub.

Prima di definire le entità:

[!code-csharp[Main](../../../efcore-dev/samples/QueryFilters/Program.cs#Entities)]

Si noti la dichiarazione di un __tenantId_ nel campo di _Blog_ entità. Questo verrà utilizzato per associare ogni istanza di blog relativo a un tenant specifico. Viene inoltre definito un _IsDeleted_ proprietà il _Post_ tipo di entità. Questa opzione viene utilizzata questa opzione per consentire di controllare se un _Post_ istanza è stato "eliminato". Ad esempio L'istanza è contrassegnata come eliminata withouth rimozione fisicamente i dati sottostanti.

A questo punto, configurare i filtri di query in _OnModelCreating_ utilizzando il ```HasQueryFilter``` API.

[!code-csharp[Main](../../../efcore-dev/samples/QueryFilters/Program.cs#Configuration)]

Le espressioni del predicato passato per il _HasQueryFilter_ chiamate verranno ora applicate automaticamente a tutte le query LINQ per tali tipi.

> [!TIP]
> Si noti l'utilizzo di un campo di livello di istanza di DbContext: ```_tenantId``` utilizzato per impostare il tenant corrente. I filtri a livello di modello verranno utilizzato il valore dall'istanza del contesto corretto. Ad esempio L'istanza che esegue la query.

## <a name="disabling-filters"></a>La disabilitazione di filtri

I filtri possono essere disabilitati per le singole query LINQ usando il ```IgnoreQueryFilters()``` operatore.

[!code-csharp[Main](../../../efcore-dev/samples/QueryFilters/Program.cs#IgnoreFilters)]

## <a name="limitations"></a>Limitazioni

I filtri di query globale presentano le limitazioni seguenti:

* I filtri non possono contenere riferimenti a proprietà di navigazione.
* I filtri possono essere definiti solo per la radice tipo di entità di una gerarchia di ereditarietà.
