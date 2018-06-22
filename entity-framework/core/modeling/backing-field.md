---
title: Il backup di base EF campi-
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: a628795e-64df-4f24-a5e8-76bc261e7ed8
ms.technology: entity-framework-core
uid: core/modeling/backing-field
ms.openlocfilehash: e95417b3368d09a64f9ec02ae40c38dc770c2e6b
ms.sourcegitcommit: 860ec5d047342fbc4063a0de881c9861cc1f8813
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/05/2017
ms.locfileid: "26053461"
---
# <a name="backing-fields"></a>Campi sottostanti

> [!NOTE]  
> Questa funzionalità è nuova in Entity Framework Core 1.1.

Campi sottostanti consentono EF leggere e/o scrittura a un campo anziché a una proprietà. Ciò può rivelarsi utile incapsulamento nella classe viene utilizzata per limitare l'utilizzo di e/o migliorare la semantica dell'accesso ai dati dal codice dell'applicazione, ma il valore deve essere letto da e/o scritto nel database senza l'utilizzo di tali restrizioni / miglioramenti.

## <a name="conventions"></a>Convenzioni

Per convenzione, i campi seguenti verranno individuati come il backup di campi per una determinata proprietà (elencati in ordine di precedenza). I campi vengono individuati solo per le proprietà che vengono incluse nel modello. Per ulteriori informazioni, in cui sono incluse le proprietà nel modello, vedere [inclusi & esclusione proprietà](included-properties.md).

* `_<camel-cased property name>`
* `_<property name>`
* `m_<camel-cased property name>`
* `m_<property name>`

[!code-csharp[Main](../../../samples/core/Modeling/Conventions/Samples/BackingField.cs#Sample)]

Quando un campo sottostante è configurato, EF scriverà direttamente a tale campo durante la materializzazione di istanze di entità dal database (anziché il setter di proprietà). Se Entity Framework deve leggere o scrivere il valore in altri momenti, utilizzerà la proprietà se possibile. Ad esempio, se è necessario aggiornare il valore per una proprietà EF, utilizzerà il setter della proprietà se definita. Se la proprietà è di sola lettura, scriverà nel campo.

## <a name="data-annotations"></a>Annotazioni dei dati

Il backup di campi non può essere configurato con le annotazioni dei dati.

## <a name="fluent-api"></a>Microsoft Office Fluent API

Per configurare un campo sottostante per una proprietà, è possibile utilizzare l'API Fluent.

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Samples/BackingField.cs#Sample)]

### <a name="controlling-when-the-field-is-used"></a>Quando viene usato il campo di controllo

È possibile configurare quando Entity Framework Usa il campo o proprietà. Vedere il [PropertyAccessMode enum](https://docs.microsoft.com/dotnet/api/microsoft.entityframeworkcore.propertyaccessmode) per le opzioni supportate.

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Samples/BackingFieldAccessMode.cs#Sample)]

### <a name="fields-without-a-property"></a>Campi senza una proprietà

È anche possibile creare una proprietà concettuale del modello che non dispone di una proprietà CLR corrispondente nella classe di entità, ma utilizza invece un campo per archiviare i dati nell'entità. Questo comportamento è diverso da [proprietà Shadow](shadow-properties.md), in cui i dati vengono archiviati nel rilevamento delle modifiche. Questo viene in genere utilizzato se la classe di entità vengono utilizzati metodi per ottenere o impostare i valori.

È possibile assegnare il nome del campo di EF il `Property(...)` API. Se è presente alcuna proprietà con il nome specificato, Entity Framework cercherà un campo.

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Samples/BackingFieldNoProperty.cs#Sample)]

È inoltre possibile assegnare la proprietà di un nome diverso dal nome del campo. Questo nome viene quindi utilizzato durante la creazione del modello, in particolare verrà utilizzato per il nome della colonna che viene eseguito il mapping per il database.

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Samples/BackingFieldConceptualProperty.cs#Sample)]

Quando è presente alcuna proprietà nella classe di entità, è possibile utilizzare il `EF.Property(...)` metodo in una query LINQ per fare riferimento alla proprietà a livello concettuale che fa parte del modello.

``` csharp
var blogs = db.blogs.OrderBy(b => EF.Property<string>(b, "Url"));
```
