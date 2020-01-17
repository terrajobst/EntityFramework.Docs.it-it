---
title: Utilizzo dei tipi di riferimento Nullable-EF Core
author: roji
ms.date: 09/09/2019
ms.assetid: bde4e0ee-fba3-4813-a849-27049323d301
uid: core/miscellaneous/nullable-reference-types
ms.openlocfilehash: c16a475c363320cd18804a4efe78ccae1ae22f0d
ms.sourcegitcommit: f2a38c086291699422d8b28a72d9611d1b24ad0d
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/16/2020
ms.locfileid: "76124353"
---
# <a name="working-with-nullable-reference-types"></a>Utilizzo dei tipi di riferimento Nullable

C#8 è stata introdotta una nuova funzionalità denominata [tipi di riferimento Nullable](/dotnet/csharp/tutorials/nullable-reference-types), che consente di aggiungere annotazioni ai tipi di riferimento, per indicare se è valido o meno. Se non si ha familiarità con questa funzionalità, è consigliabile prenderne nota leggendo i C# documenti.

Questa pagina introduce il supporto di EF Core per i tipi di riferimento nullable e descrive le procedure consigliate per l'utilizzo.

## <a name="required-and-optional-properties"></a>Proprietà obbligatorie e facoltative

La documentazione principale sulle proprietà obbligatorie e facoltative e la relativa interazione con i tipi di riferimento Nullable è la pagina delle [proprietà obbligatoria e facoltativa](xref:core/modeling/entity-properties#required-and-optional-properties) . È consigliabile iniziare leggendo prima di tutto la pagina.

> [!NOTE]
> Prestare attenzione quando si abilitano i tipi di riferimento nullable in un progetto esistente: le proprietà del tipo di riferimento che in precedenza erano configurate come facoltative ora verranno configurate come richieste, a meno che non vengano annotate in modo esplicito come Nullable. Quando si gestisce uno schema di database relazionale, è possibile che vengano generate migrazioni che modificano il supporto di valori null per la colonna di database.

## <a name="dbcontext-and-dbset"></a>DbContext e DbSet

Quando sono abilitati i tipi di riferimento C# Nullable, il compilatore genera avvisi per qualsiasi proprietà non nullable non inizializzata, in quanto questi contengono valori null. Di conseguenza, la prassi comune di definire un `DbSet` non nullable in un contesto genererà ora un avviso. Tuttavia, EF Core Inizializza sempre tutte le proprietà `DbSet` sui tipi derivati da DbContext, in modo che non siano mai null, anche se il compilatore non è a conoscenza di questo. Pertanto, è consigliabile lasciare le proprietà `DbSet` non nullable, in modo da consentire l'accesso senza controlli null e per silenziare gli avvisi del compilatore impostando in modo esplicito su null con l'ausilio dell'operatore che perdona i valori null (!):

[!code-csharp[Main](../../../samples/core/Miscellaneous/NullableReferenceTypes/NullableReferenceTypesContext.cs?name=Context&highlight=3-4)]

## <a name="non-nullable-properties-and-initialization"></a>Proprietà e inizializzazione non nullable

Gli avvisi del compilatore per i tipi di riferimento non nullable non inizializzati rappresentano anche un problema per le proprietà regolari nei tipi di entità. Nell'esempio precedente, questi avvisi sono stati evitati usando il [binding del costruttore](xref:core/modeling/constructors), una funzionalità che funziona perfettamente con proprietà non nullable, garantendo che siano sempre inizializzate. Tuttavia, in alcuni scenari il binding del costruttore non è un'opzione: le proprietà di navigazione, ad esempio, non possono essere inizializzate in questo modo.

Le proprietà di navigazione obbligatorie presentano un ulteriore problema: Sebbene un dipendente esista sempre per un'entità specifica, può essere caricato o meno da una determinata query, a seconda delle esigenze in quel punto del programma ([vedere i diversi modelli per il caricamento dei dati](xref:core/querying/related-data)). Allo stesso tempo, è preferibile rendere tali proprietà Nullable, in quanto ciò impone l'accesso a tutti gli accessi per verificare la presenza di valori null, anche se sono necessari.

Un modo per gestire questi scenari consiste nel disporre di una proprietà che non ammette i valori null con un [campo](xref:core/modeling/backing-field)sottostante Nullable:

[!code-csharp[Main](../../../samples/core/Miscellaneous/NullableReferenceTypes/Order.cs?range=10-17)]

Poiché la proprietà di navigazione è non nullable, viene configurata una navigazione obbligatoria. fino a quando lo spostamento viene caricato correttamente, il dipendente sarà accessibile tramite la proprietà. Se, tuttavia, viene eseguito l'accesso alla proprietà senza prima caricare correttamente l'entità correlata, viene generata un'eccezione InvalidOperationException, perché il contratto API è stato usato in modo errato. Si noti che EF deve essere configurato per accedere sempre al campo sottostante e non alla proprietà, perché si basa sulla possibilità di leggere il valore anche quando viene annullato. per informazioni su come eseguire questa operazione, vedere la documentazione relativa ai [campi di supporto](xref:core/modeling/backing-field) e prendere in considerazione la possibilità di specificare `PropertyAccessMode.Field` per assicurarsi che la configurazione sia corretta.

Come alternativa Terser, è possibile semplicemente inizializzare la proprietà su null con l'ausilio dell'operatore con indulgenza null (!):

[!code-csharp[Main](../../../samples/core/Miscellaneous/NullableReferenceTypes/Order.cs?range=19)]

Un valore null effettivo non verrà mai osservato ad eccezione di come risultato di un bug di programmazione, ad esempio l'accesso alla proprietà di navigazione senza caricare correttamente l'entità correlata in precedenza.

> [!NOTE]
> Le esplorazioni della raccolta, che contengono riferimenti a più entità correlate, devono essere sempre non nullable. Una raccolta vuota significa che non esiste alcuna entità correlata, ma l'elenco stesso non deve mai essere null.

## <a name="navigating-and-including-nullable-relationships"></a>Esplorazione e inclusione di relazioni Nullable

Quando si gestiscono relazioni facoltative, è possibile che si verifichino avvisi del compilatore in cui un'eccezione di riferimento null effettiva potrebbe non essere possibile. Quando si traducono ed eseguono le query LINQ, EF Core garantisce che se non esiste un'entità correlata facoltativa, eventuali spostamenti verranno semplicemente ignorati, anziché generare. Tuttavia, il compilatore non è a conoscenza di questo EF Core garanzia e genera avvisi come se la query LINQ venisse eseguita in memoria, con LINQ to Objects. Di conseguenza, è necessario usare l'operatore di perdonazione null (!) per informare il compilatore che non è possibile un valore null effettivo:

[!code-csharp[Main](../../../samples/core/Miscellaneous/NullableReferenceTypes/Program.cs?range=46)]

Si verifica un problema simile quando si includono più livelli di relazioni tra gli spostamenti facoltativi:

[!code-csharp[Main](../../../samples/core/Miscellaneous/NullableReferenceTypes/Program.cs?range=36-39&highlight=2)]

Se questa operazione viene eseguita molto e i tipi di entità in questione sono prevalentemente (o esclusivamente) usati nelle query di EF Core, è consigliabile rendere le proprietà di navigazione non nullable e configurarle come facoltative tramite l'API Fluent o le annotazioni dei dati. Tutti gli avvisi del compilatore vengono rimossi mantenendo la relazione facoltativa. Tuttavia, se le entità vengono attraversate all'esterno di EF Core, è possibile osservare valori null anche se le proprietà sono annotate come non nullable.

## <a name="limitations"></a>Limitazioni

* Il reverse engineering non supporta [ C# attualmente 8 tipi di riferimento Nullable (NRTs)](/dotnet/csharp/tutorials/nullable-reference-types): EF Core C# genera sempre codice che presuppone che la funzionalità sia disattivata. Ad esempio, le colonne di testo Nullable verranno sottoposto a impalcatura come proprietà con tipo `string`, non `string?`, con l'API Fluent o le annotazioni dei dati utilizzate per configurare se una proprietà è obbligatoria o meno. È possibile modificare il codice con impalcature e sostituirle C# con annotazioni di valori null. Il supporto dell'impalcatura per i tipi di riferimento nullable viene rilevato da Issue [#15520](https://github.com/aspnet/EntityFrameworkCore/issues/15520).
* La superficie dell'API pubblica di EF Core non è ancora stata annotata per il supporto di valori null (l'API pubblica è "ignaro null"), rendendo talvolta scomodo da usare quando la funzionalità NRT è attivata. In particolare, include gli operatori LINQ asincroni esposti da EF Core, ad esempio [FirstOrDefaultAsync](/dotnet/api/microsoft.entityframeworkcore.entityframeworkqueryableextensions.firstordefaultasync#Microsoft_EntityFrameworkCore_EntityFrameworkQueryableExtensions_FirstOrDefaultAsync__1_System_Linq_IQueryable___0__System_Linq_Expressions_Expression_System_Func___0_System_Boolean___System_Threading_CancellationToken_). Si prevede di risolvere questo problema per la versione 5,0.
