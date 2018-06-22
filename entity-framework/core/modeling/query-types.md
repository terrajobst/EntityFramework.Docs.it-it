---
title: Tipi di query - Core a Entity Framework
author: anpete
ms.author: anpete
ms.date: 2/26/2018
ms.assetid: 9F4450C5-1A3F-4BB6-AC19-9FAC64292AAD
ms.technology: entity-framework-core
uid: core/modeling/query-types
ms.openlocfilehash: f16e3a130f3a4f92b2bf6014f2df0ca4eec56a25
ms.sourcegitcommit: 038acd91ce2f5a28d76dcd2eab72eeba225e366d
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/14/2018
ms.locfileid: "34163174"
---
# <a name="query-types"></a>Tipi di query
> [!NOTE]
> Questa funzionalità è nuova in Entity Framework Core 2.1

Oltre ai tipi di entità, può contenere un modello di Entity Framework Core _tipi di query_, che può essere usato per eseguire query sui dati che non sono stato eseguito il mapping ai tipi di entità.

Tipi di query presentano molte analogie con tipi di entità:

- Possono inoltre essere aggiunti al modello oppure in `OnModelCreating`, o tramite una proprietà "set" su un oggetto derivato _DbContext_.
- Supportano molte delle stesse funzionalità di mapping, come l'ereditarietà di mapping, le proprietà di navigazione (vedere le limitazioni seguenti) e, negli archivi relazionali, la possibilità di configurare le oggetti di database di destinazione e le colonne tramite metodi dell'API fluent o le annotazioni dei dati.

Tuttavia sono diversi dall'entità tipi che sono:

- Non richiedono una chiave da definire.
- Non viene mai tenuta traccia delle modifiche la _DbContext_ e pertanto sono mai inseriti, aggiornati o eliminati nel database.
- Non vengono individuati per convenzione.
- Supportano solo un subset di funzionalità di mapping di spostamento, in particolare:
  - Mai agiscono come l'entità finale principale di una relazione.
  - Possono contenere solo le proprietà di navigazione di riferimento che punta all'entità.
  - Entità non possono contenere le proprietà di navigazione a tipi di query.
- Vengono indirizzate nel _ModelBuilder_ utilizzando il `Query` metodo invece che al `Entity` (metodo).
- Viene eseguito il mapping sul _DbContext_ tramite le proprietà di tipo `DbQuery<T>` anziché `DbSet<T>`
- Viene eseguito il mapping agli oggetti di database utilizzando la `ToView` metodo, piuttosto che `ToTable`.
- Può essere mappato a un _definizione di query_ - una definizione di query è una query secondaria dichiarata nel modello che funge da un'origine dati per un tipo di query.

Alcuni degli scenari di utilizzo principali per i tipi di query sono:

- Serve come tipo restituito per hoc `FromSql()` query.
- Mapping a viste di database.
- Mapping a tabelle che non dispone di una chiave primaria definita.
- Mapping per le query definite nel modello.

> [!TIP]
> Mapping di un tipo di query a un oggetto di database viene eseguito mediante il `ToView` API fluent. Dal punto di vista di EF nell'elemento principale, l'oggetto di database specificato in questo metodo è un _vista_, vale a dire che viene considerato come un'origine di query di sola lettura e non può essere la destinazione dell'aggiornamento, inserimento o le operazioni di eliminazione. Tuttavia, ciò non significa che l'oggetto di database risulta effettivamente necessario per essere una vista di database, in alternativa può essere una tabella di database che verrà trattata come di sola lettura. Al contrario, per i tipi di entità, Core EF si presuppone che un oggetto di database specificato nella `ToTable` metodo può essere considerato come un _tabella_, vale a dire che può essere utilizzato come origine di query ma anche assegnato dall'aggiornamento, eliminazione e inserimento operazioni. In effetti, è possibile specificare il nome di una vista di database in `ToTable` e tutto dovrebbe funzionare, purché la vista è configurata per essere aggiornabile nel database.

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

Successivamente, è stato possibile configurare il tipo di query in _OnModelCreating_ utilizzando il `modelBuilder.Query<T>` API.
Si utilizza configurazione standard di Microsoft Office fluent API per configurare il mapping per il tipo di Query:

[!code-csharp[Main](../../../efcore-dev/samples/QueryTypes/Program.cs#Configuration)]

Infine, possiamo eseguire una query la visualizzazione del database nella modalità standard:

[!code-csharp[Main](../../../efcore-dev/samples/QueryTypes/Program.cs#Query)]

> [!TIP]
> Si noti che vengono definite anche una proprietà di query di livello (DbQuery) di agire come radice per le query su questo tipo di contesto.
