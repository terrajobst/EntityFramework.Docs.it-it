---
title: Conversioni dei valori - EF Core
author: ajcvickers
ms.author: divega
ms.date: 02/19/2018
ms.assetid: 3154BF3C-1749-4C60-8D51-AE86773AA116
ms.technology: entity-framework-core
uid: core/modeling/value-conversions
ms.openlocfilehash: 5bfb6111ac450db91f3f1a7074a924a1c8400ce7
ms.sourcegitcommit: bdd06c9a591ba5e6d6a3ec046c80de98f598f3f3
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/10/2018
ms.locfileid: "37949092"
---
# <a name="value-conversions"></a>Conversioni dei valori

> [!NOTE]  
> Questa funzionalità è stata introdotta in EF Core 2.1.

Convertitori di valori i valori delle proprietà da convertire durante la lettura o la scrittura nel database. Questa conversione può provenire da un valore a un altro dello stesso tipo (ad esempio, la crittografia di stringhe) o da un valore di un tipo in un valore di un altro tipo (ad esempio, la conversione dei valori enum in e da stringhe nel database.)

## <a name="fundamentals"></a>Concetti fondamentali

Convertitori di valori vengono specificati in termini di un `ModelClrType` e un `ProviderClrType`. Il tipo di modello è il tipo di .NET della proprietà nel tipo di entità. Il tipo di provider è il tipo .NET riconosciuto dal provider del database. Ad esempio, per salvare le enumerazioni come stringhe nel database, il tipo di modello è il tipo di enumerazione e il tipo di provider `String`. Questi due tipi possono essere lo stesso.

Le conversioni vengono definite utilizzando due `Func` alberi delle espressioni: uno dal `ModelClrType` a `ProviderClrType` e l'altro `ProviderClrType` a `ModelClrType`. Gli alberi delle espressioni vengono usate in modo che può essere compilati nel codice di accesso del database per le conversioni efficiente. Per le conversioni complesse, l'albero delle espressioni può essere una semplice chiamata a un metodo che esegue la conversione.

## <a name="configuring-a-value-converter"></a>Configurazione di un convertitore di valori

Le conversioni dei valori definite nella proprietà in OnModelCreating di DbContext. Si consideri, ad esempio, un tipo di entità e di enumerazione definito come:
```Csharp
public class Rider
{
    public int Id { get; set; }
    public EquineBeast Mount { get; set; }
}

public enum EquineBeast
{
    Donkey,
    Mule,
    Horse,
    Unicorn
}
```
Le conversioni possono quindi essere definite in OnModelCreating per archiviare i valori di enumerazione sotto forma di stringhe (ad esempio, "Asino", "Muli",...) nel database:
```Csharp
protected override void OnModelCreating(ModelBuilder modelBuilder)
{
    modelBuilder
        .Entity<Rider>()
        .Property(e => e.Mount)
        .HasConversion(
            v => v.ToString(),
            v => (EquineBeast)Enum.Parse(typeof(EquineBeast), v));
}
```
> [!NOTE]  
> Oggetto `null` valore non verrà mai passato a un convertitore di valori. Questo semplifica l'implementazione delle conversioni e consente loro di essere condivisa fra le proprietà che ammette valori null e non nullable.

## <a name="the-valueconverter-class"></a>La classe di ValueConverter

La chiamata `HasConversion` come illustrato in precedenza verrà creato un `ValueConverter` dell'istanza e impostarla sulla proprietà. Il `ValueConverter` invece può essere creato in modo esplicito. Ad esempio:
```Csharp
var converter = new ValueConverter<EquineBeast, string>(
    v => v.ToString(),
    v => (EquineBeast)Enum.Parse(typeof(EquineBeast), v));

modelBuilder
    .Entity<Rider>()
    .Property(e => e.Mount)
    .HasConversion(converter);
```
Ciò può essere utile quando più proprietà utilizzano la stessa conversione.

> [!NOTE]  
> Non è attualmente possibile specificare in un'unica posizione che tutte le proprietà di un determinato tipo devono usare il convertitore di valore stesso. Questa funzionalità verrà considerata una versione futura.

## <a name="built-in-converters"></a>Convertitori di tipi incorporati

EF Core include un set di pre-definite `ValueConverter` classi, presenti nel `Microsoft.EntityFrameworkCore.Storage.ValueConversion` dello spazio dei nomi. Questi sono:
* `BoolToZeroOneConverter` -Bool a zero e uno
* `BoolToStringConverter` -Bool in stringhe, ad esempio "Y" e "N"
* `BoolToTwoValuesConverter` -Bool a due valori
* `BytesToStringConverter` -Matrice di byte alla stringa con codifica Base64
* `CastingConverter` -Le conversioni che richiedono solo un cast Csharp
* `CharToStringConverter` -Char stringa singolo carattere
* `DateTimeOffsetToBinaryConverter` -DateTimeOffset valore a 64 bit con codifica binaria
* `DateTimeOffsetToBytesConverter` -DateTimeOffset matrice di byte
* `DateTimeOffsetToStringConverter` -DateTimeOffset alla stringa
* `DateTimeToBinaryConverter` -DateTime valore a 64 bit compresi DateTimeKind
* `DateTimeToStringConverter` -DateTime a string
* `DateTimeToTicksConverter` -DateTime con segni di graduazione
* `EnumToNumberConverter` -Enum numero sottostante
* `EnumToStringConverter` -Enum in stringa
* `GuidToBytesConverter` -Guid in matrice di byte
* `GuidToStringConverter` -Guid in stringa
* `NumberToBytesConverter` -Qualsiasi valore numerico in matrice di byte
* `NumberToStringConverter` -Qualsiasi valore numerico in stringa
* `StringToBytesConverter` -Stringa a byte UTF8
* `TimeSpanToStringConverter` -TimeSpan in stringa
* `TimeSpanToTicksConverter` -Intervallo di tempo da segni di graduazione

Si noti che `EnumToStringConverter` è incluso in questo elenco. Ciò significa che non è necessario specificare la conversione in modo esplicito, come illustrato in precedenza. Usare invece solo il convertitore incorporato:
```Csharp
var converter = new EnumToStringConverter<EquineBeast>();

modelBuilder
    .Entity<Rider>()
    .Property(e => e.Mount)
    .HasConversion(converter);
```
Si noti che tutti i convertitori predefiniti sono senza stati e pertanto una singola istanza può essere condiviso in modo sicuro da più proprietà.

## <a name="pre-defined-conversions"></a>Conversioni predefinite

Per le conversioni comune per il quale esiste un convertitore di tipi incorporati non è necessario specificare in modo esplicito il convertitore. Al contrario, configurare solo il tipo di provider debba essere usate ed Entity Framework la userà automaticamente il convertitore di compilazione appropriato. Enumerazione per le conversioni di stringa vengono usati come un esempio precedente, ma EF verranno effettivamente eseguire questa operazione automaticamente se è configurato il tipo di provider:
```Csharp
modelBuilder
    .Entity<Rider>()
    .Property(e => e.Mount)
    .HasConversion<string>();
```
La stessa operazione, è possibile specificare in modo esplicito il tipo di colonna. Ad esempio, se il tipo di entità è definito come in modo che:
```Csharp
public class Rider
{
    public int Id { get; set; }

    [Column(TypeName = "nvarchar(24)")]
    public EquineBeast Mount { get; set; }
}
```
Quindi verranno salvati come stringhe nel database senza altre operazioni di configurazione in OnModelCreating i valori di enumerazione.

## <a name="limitations"></a>Limitazioni

Esistono alcune limitazioni correnti del sistema di conversione del valore:
* Come indicato in precedenza, `null` non può essere convertito.
* Attualmente non è possibile distribuire una conversione di una proprietà in più colonne o viceversa.
* Uso di conversioni dei valori può influire sulla capacità di EF Core per tradurre le espressioni SQL. Per questi casi verrà registrato un avviso.
Per una versione futura verrà preso in considerazione la rimozione di queste limitazioni.
