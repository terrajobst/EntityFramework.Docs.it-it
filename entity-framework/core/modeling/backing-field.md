---
title: Campi sottoposti a backup-EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: a628795e-64df-4f24-a5e8-76bc261e7ed8
uid: core/modeling/backing-field
ms.openlocfilehash: c3ca8bb97992c192672e8c2f2040b0de029df68d
ms.sourcegitcommit: ec196918691f50cd0b21693515b0549f06d9f39c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/23/2019
ms.locfileid: "71197486"
---
# <a name="backing-fields"></a>Campi sottoposti a backup

> [!NOTE]  
> Questa funzionalità è una novità di EF Core 1,1.

I campi di backup consentono a EF di leggere e/o scrivere in un campo anziché in una proprietà. Questa operazione può essere utile quando si utilizza l'incapsulamento nella classe per limitare l'utilizzo di e/o migliorare la semantica di accesso ai dati da parte del codice dell'applicazione, ma il valore deve essere letto e/o scritto nel database senza utilizzare tali restrizioni/ miglioramenti.

## <a name="conventions"></a>Convenzioni

Per convenzione, i campi seguenti vengono individuati come campi di backup per una determinata proprietà (elencati in ordine di precedenza). I campi vengono individuati solo per le proprietà incluse nel modello. Per ulteriori informazioni su quali proprietà sono incluse nel modello, vedere [inclusione & escluse le proprietà](included-properties.md).

* `_<camel-cased property name>`
* `_<property name>`
* `m_<camel-cased property name>`
* `m_<property name>`

[!code-csharp[Main](../../../samples/core/Modeling/Conventions/BackingField.cs#Sample)]

Quando viene configurato un campo sottostante, EF scrive direttamente in quel campo quando materializzazione le istanze dell'entità dal database, invece di usare il metodo di impostazione della proprietà. Se EF deve leggere o scrivere il valore in altri momenti, utilizzerà la proprietà, se possibile. Se, ad esempio, EF deve aggiornare il valore di una proprietà, utilizzerà il metodo di impostazione della proprietà, se ne è stato definito uno. Se la proprietà è di sola lettura, verrà scritta nel campo.

## <a name="data-annotations"></a>Annotazioni dei dati

Non è possibile configurare i campi di backup con le annotazioni dei dati.

## <a name="fluent-api"></a>API Fluent

È possibile usare l'API Fluent per configurare un campo sottostante per una proprietà.

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/BackingField.cs#Sample)]

### <a name="controlling-when-the-field-is-used"></a>Controllo quando viene utilizzato il campo

È possibile configurare quando EF usa il campo o la proprietà. Per le opzioni supportate, vedere l' [enumerazione PropertyAccessMode](https://docs.microsoft.com/dotnet/api/microsoft.entityframeworkcore.propertyaccessmode) .

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/BackingFieldAccessMode.cs#Sample)]

### <a name="fields-without-a-property"></a>Campi senza una proprietà

È anche possibile creare una proprietà concettuale nel modello che non disponga di una proprietà CLR corrispondente nella classe di entità, ma usa invece un campo per archiviare i dati nell'entità. Questo è diverso dalle [proprietà shadow](shadow-properties.md), in cui i dati vengono archiviati nello strumento di rilevamento delle modifiche. Questa operazione viene in genere utilizzata se la classe di entità utilizza metodi per ottenere o impostare i valori.

È possibile assegnare a EF il nome del campo nell' `Property(...)` API. Se non è presente alcuna proprietà con il nome specificato, Entity Framework cercherà un campo.

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/BackingFieldNoProperty.cs#Sample)]

È anche possibile scegliere di assegnare alla proprietà un nome diverso dal nome del campo. Questo nome viene quindi utilizzato quando si crea il modello, in particolare verrà utilizzato per il nome della colonna mappato a nel database.

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/BackingFieldConceptualProperty.cs#Sample)]

Quando non è presente alcuna proprietà nella classe di entità, è possibile utilizzare `EF.Property(...)` il metodo in una query LINQ per fare riferimento alla proprietà che è concettualmente parte del modello.

``` csharp
var blogs = db.blogs.OrderBy(b => EF.Property<string>(b, "Url"));
```
