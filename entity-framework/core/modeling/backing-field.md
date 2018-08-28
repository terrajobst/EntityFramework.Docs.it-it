---
title: Il backup di campi - EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: a628795e-64df-4f24-a5e8-76bc261e7ed8
uid: core/modeling/backing-field
ms.openlocfilehash: 79221b6f7968675ff10f80d5df181b674b6a20c9
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 08/27/2018
ms.locfileid: "42994093"
---
# <a name="backing-fields"></a>Campi sottostanti

> [!NOTE]  
> Questa funzionalità è una novità di EF Core 1.1.

Campi sottostanti consentono a Entity Framework di lettura e/o scrittura a un campo anziché a una proprietà. Ciò risulta utile quando l'incapsulamento della classe viene utilizzata per limitare l'uso di/o migliorare la semantica per l'accesso ai dati dal codice dell'applicazione, ma il valore deve essere letto da e/o scritto nel database senza l'utilizzo di tali restrizioni / miglioramenti.

## <a name="conventions"></a>Convenzioni

Per convenzione, i campi seguenti verranno individuati come campi per una determinata proprietà (elencati in ordine di precedenza) sottostanti. I campi vengono individuati solo per le proprietà che vengono incluse nel modello. Per altre informazioni su cui le proprietà vengono incluse nel modello, vedere [inclusione ed esclusione di proprietà](included-properties.md).

* `_<camel-cased property name>`
* `_<property name>`
* `m_<camel-cased property name>`
* `m_<property name>`

[!code-csharp[Main](../../../samples/core/Modeling/Conventions/Samples/BackingField.cs#Sample)]

Quando un campo sottostante è configurato, quando la materializzazione delle istanze di entità dal database, anziché usando i setter delle proprietà, EF scriverà direttamente a tale campo. Se Entity Framework deve leggere o scrivere il valore in altri momenti, utilizzerà la proprietà se possibile. Ad esempio, se è necessario aggiornare il valore di una proprietà di EF, utilizzerà i setter delle proprietà se ne è stata definita. Se la proprietà è di sola lettura, quindi lo scriverà al campo.

## <a name="data-annotations"></a>Annotazioni dei dati

Campi sottostanti non può essere configurato con le annotazioni dei dati.

## <a name="fluent-api"></a>API Fluent

È possibile usare l'API Fluent per configurare un campo sottostante per una proprietà.

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Samples/BackingField.cs#Sample)]

### <a name="controlling-when-the-field-is-used"></a>Il controllo quando viene usato il campo

È possibile configurare quando Entity Framework Usa il campo o proprietà. Vedere le [enum PropertyAccessMode](https://docs.microsoft.com/dotnet/api/microsoft.entityframeworkcore.propertyaccessmode) per le opzioni supportate.

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Samples/BackingFieldAccessMode.cs#Sample)]

### <a name="fields-without-a-property"></a>Campi senza una proprietà

È anche possibile creare una proprietà concettuale nel modello che non dispone di una proprietà CLR corrispondente nella classe di entità, ma Usa invece un campo per archiviare i dati nell'entità. Questo comportamento è diverso da [proprietà Shadow](shadow-properties.md), in cui i dati vengono archiviati nel rilevamento delle modifiche. Ciò in genere vengono usato se la classe di entità Usa i metodi per ottenere o impostare i valori.

È possibile assegnare il nome del campo di Entity Framework il `Property(...)` API. Se è presente nessuna proprietà con il nome specificato, Entity Framework cercherà un campo.

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Samples/BackingFieldNoProperty.cs#Sample)]

È anche possibile scegliere di assegnare un nome diverso da quello del campo della proprietà. Questo nome viene quindi utilizzato durante la creazione del modello, in particolare verrà usato per il nome della colonna che viene eseguito il mapping nel database.

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Samples/BackingFieldConceptualProperty.cs#Sample)]

Quando è presente alcuna proprietà nella classe di entità, è possibile usare il `EF.Property(...)` metodo in una query LINQ per fare riferimento alla proprietà che è concettualmente parte del modello.

``` csharp
var blogs = db.blogs.OrderBy(b => EF.Property<string>(b, "Url"));
```
