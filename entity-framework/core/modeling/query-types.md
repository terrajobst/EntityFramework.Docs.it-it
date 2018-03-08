---
title: Tipi di query - Core a Entity Framework
author: anpete
ms.author: anpete
ms.date: 2/26/2018
ms.assetid: 9F4450C5-1A3F-4BB6-AC19-9FAC64292AAD
ms.technology: entity-framework-core
uid: core/modeling/query-types
ms.openlocfilehash: 19a371c65da33e8209cc1ab3423a67c34ddae61e
ms.sourcegitcommit: fc68321c211aca38f7b9dc3a75677c6ca1b2524b
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/08/2018
---
# <a name="query-types"></a>Tipi di query
> [!NOTE]
> Questa funzionalità è nuova in Entity Framework Core 2.1

Tipi di query sono tipi di risultati di query di sola lettura che possono essere aggiunto al modello di base di Entity Framework. Tipi di query abilitare query ad hoc (ad esempio, tipi anonimi), ma sono più flessibili perché possono avere configurazione del mapping specificata.

Sono concettualmente simili ai tipi di entità in cui:

- Sono tipi POCO c# che vengono aggiunti al modello, in ```OnModelCreating``` utilizzando il ```ModelBuilder.Query``` metodo, o tramite una proprietà DbContext "set" (per i tipi di query tale proprietà è digitata come ```DbQuery<T>``` piuttosto che ```DbSet<T>```).
- Supportano la maggior parte delle stesse funzionalità di mapping come tipi di entità normale. Ad esempio, il mapping di ereditarietà, spostamenti (vedere limitiations riportato di seguito) e, in archivi relazionali, la possibilità di configurare gli oggetti dello schema di database di destinazione tramite ```ToTable```, ```HasColumn``` metodi api fluent (o le annotazioni dei dati).

Tipi di query sono diversi dall'entità tipi in cui:

- Non richiedono una chiave da definire.
- Non sono mai monitorati dal rilevamento delle modifiche.
- Non vengono individuati per convenzione.
- Supportano solo un sottoinsieme di funzionalità di mapping di spostamento, in particolare, non possono agire come l'entità finale principale di una relazione.
- Può essere mappato a un _definizione query_ -una definizione di Query è secondario che funge da un'origine dati per un tipo di Query.

Alcuni degli scenari di utilizzo principali per i tipi di query sono:

- Mapping a viste di database.
- Mapping a tabelle che non dispone di una chiave primaria definita.
- Serve come tipo restituito per hoc ```FromSql()``` query.
- Mapping per le query definite nel modello.

> [!TIP]
> Mapping di un tipo di query a una vista di database viene eseguito mediante il ```ToTable``` API fluent.

## <a name="example"></a>Esempio

Nell'esempio seguente viene illustrato come utilizzare il tipo di Query eseguire query su una vista di database.

> [!TIP]
> È possibile visualizzare l'[esempio](https://github.com/aspnet/EntityFrameworkCore/tree/dev/samples/QueryTypes) di questo articolo in GitHub.

In primo luogo, è necessario definire un modello semplice di Blog e Post:

[!code-csharp[Main](../../../efcore-dev/samples/QueryTypes/Program.cs#Entities)]

Successivamente, è possibile definire una vista di database semplice che consente di eseguire una query il numero di inserimenti associata a ogni blog:

[!code-csharp[Main](../../../efcore-dev/samples/QueryTypes/Program.cs#View)]

Successivamente, è possibile definire una classe per contenere il risultato della vista di database:

[!code-csharp[Main](../../../efcore-dev/samples/QueryTypes/Program.cs#QueryType)]

Successivamente, è stato possibile configurare il tipo di query in _OnModelCreating_ utilizzando il ```modelBuilder.Query<T>``` API.
Si utilizza configurazione standard di Microsoft Office fluent API per configurare il mapping per il tipo di Query:

[!code-csharp[Main](../../../efcore-dev/samples/QueryTypes/Program.cs#Configuration)]

Infine, possiamo eseguire una query la visualizzazione del database nella modalità standard:

[!code-csharp[Main](../../../efcore-dev/samples/QueryTypes/Program.cs#Query)]

> [!TIP]
> Si noti che vengono definite anche una proprietà di query di livello (DbQuery) di agire come radice per le query su questo tipo di contesto.
