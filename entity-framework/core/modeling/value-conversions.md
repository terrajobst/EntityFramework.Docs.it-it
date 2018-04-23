---
title: Conversioni dei valori - Core a Entity Framework
author: ajcvickers
ms.author: divega
ms.date: 02/19/2018
ms.assetid: 3154BF3C-1749-4C60-8D51-AE86773AA116
ms.technology: entity-framework-core
uid: core/modeling/value-conversions
ms.openlocfilehash: 329d2757059462468ca30772d37789343c03ba7b
ms.sourcegitcommit: 4997314356118d0d97b04ad82e433e49bb9420a2
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/16/2018
---
# <a name="value-conversions"></a>Conversioni di valori

> [!NOTE]  
> Questa funzionalità è nuova in Entity Framework Core 2.1.

Convertitori di valori consentono di convertire durante la lettura o scrittura nel database i valori di proprietà. Questa conversione può provenire da un valore a un altro dello stesso tipo (ad esempio, la crittografia di stringhe) o da un valore di un tipo a un valore di un altro tipo (ad esempio, la conversione dei valori enum in e da stringhe nel database.)

## <a name="fundamentals"></a>Concetti fondamentali

Convertitori di valori vengono specificati in termini di un `ModelClrType` e `ProviderClrType`. Il tipo di modello è il tipo .NET della proprietà nel tipo di entità. Il tipo di provider è il tipo .NET riconosciuto dal provider di database. Ad esempio, per salvare le enumerazioni sotto forma di stringhe nel database, il tipo di modello è il tipo di enum, e il tipo di provider è `String`. Questi due tipi possono essere uguale.

Le conversioni vengono definite utilizzando due `Func` alberi delle espressioni: uno dal `ModelClrType` a `ProviderClrType` e l'altro da `ProviderClrType` a `ModelClrType`. Gli alberi delle espressioni vengono usate in modo che può essere compilati nel codice di accesso del database per le conversioni efficiente. Per le conversioni complesse, la struttura ad albero dell'espressione può essere una chiamata semplice a un metodo che esegue la conversione.

## <a name="configuring-a-value-converter"></a>Configurazione di un convertitore di valori

Le conversioni di valori sono definite nelle proprietà OnModelCreating del DbContext. Si consideri, ad esempio, un tipo di entità e di enumerazione definito come:
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
Quindi le conversioni possono essere definite in OnModelCreating per archiviare i valori di enumerazione come stringhe (ad esempio "Asino", "Muli",...) nel database:
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
> Oggetto `null` valore non verrà mai passato a un convertitore di valori. Questo semplifica l'implementazione delle conversioni e consente loro di essere condivisa fra proprietà ammette valori null e non ammette valori null.

## <a name="the-valueconverter-class"></a>La classe ValueConverter

La chiamata `HasConversion` come illustrato in precedenza verrà creato un `ValueConverter` istanza e impostarla sulla proprietà. Il `ValueConverter` è invece possibile creare in modo esplicito. Ad esempio:
```Csharp
var converter = new ValueConverter<EquineBeast, string>(
    v => v.ToString(),
    v => (EquineBeast)Enum.Parse(typeof(EquineBeast), v));

modelBuilder
    .Entity<Rider>()
    .Property(e => e.Mount)
    .HasConversion(converter);
```
Può essere utile quando più proprietà utilizzano la stessa conversione.

> [!NOTE]  
> Attualmente non è possibile specificare in un'unica posizione che tutte le proprietà di un determinato tipo è necessario usare il convertitore stesso. Questa funzionalità verrà considerata per una versione futura.

## <a name="built-in-converters"></a>Convertitori di tipi incorporati

EF Core fornito con un set di predefiniti `ValueConverter` classi, il `Microsoft.EntityFrameworkCore.Storage.Converters` dello spazio dei nomi. Questi sono:
* `BoolToZeroOneConverter` -Bool a zero e uno
* `BoolToStringConverter` -Bool a stringhe, ad esempio "Y" e "N"
* `BoolToTwoValuesConverter` -Bool a due valori
* `BytesToStringConverter` -Matrice di byte stringa con codifica Base64
* `CastingConverter` -Le conversioni che richiedono solo un cast Csharp
* `CharToStringConverter` -Char unica stringa di caratteri
* `DateTimeOffsetToBinaryConverter` -DateTimeOffset per valore a 64 bit con codifica binaria
* `DateTimeOffsetToBytesConverter` -DateTimeOffset matrice di byte
* `DateTimeOffsetToStringConverter` -DateTimeOffset stringa
* `DateTimeToBinaryConverter` -DateTime valore a 64 bit inclusi DateTimeKind
* `DateTimeToStringConverter` -DateTime a string
* `DateTimeToTicksConverter` -DateTime con segni di graduazione
* `EnumToNumberConverter` -Enumerazione numero sottostante
* `EnumToStringConverter` -Enumerazione stringa
* `GuidToBytesConverter` -Guid in matrice di byte
* `GuidToStringConverter` -Guid in stringa
* `NumberToBytesConverter` -Qualsiasi valore numerico di una matrice di byte
* `NumberToStringConverter` -Qualsiasi valore numerico in stringa
* `StringToBytesConverter` -Stringa a byte UTF8
* `TimeSpanToStringConverter` -TimeSpan in stringa
* `TimeSpanToTicksConverter` -Intervallo di tempo da segni di graduazione

Si noti che `EnumToStringConverter` è incluso nell'elenco. Ciò significa che non è necessario specificare la conversione in modo esplicito, come illustrato in precedenza. Invece, utilizzare il convertitore predefinito:
```Csharp
var converter = new EnumToStringConverter<EquineBeast>();

modelBuilder
    .Entity<Rider>()
    .Property(e => e.Mount)
    .HasConversion(converter);
```
Si noti che tutti i convertitori incorporati sono senza stati e pertanto una singola istanza può essere condiviso in modo sicuro da più proprietà.

## <a name="pre-defined-conversions"></a>Conversioni predefinite

Per le conversioni comuni per il quale esiste un convertitore di tipi incorporati non è necessario specificare in modo esplicito il convertitore. Al contrario, configurare solo il tipo di provider deve essere utilizzato ed EF verrà utilizzato automaticamente il convertitore di compilazione appropriato. Enumerazione per le conversioni di stringa vengono utilizzate come un esempio precedente, ma EF verrà effettivamente eseguire questa operazione automaticamente se è configurato il tipo di provider:
```Csharp
modelBuilder
    .Entity<Rider>()
    .Property(e => e.Mount)
    .HasConversion<string>();
```
La stessa operazione può essere ottenuta specificando in modo esplicito il tipo di colonna. Ad esempio, se il tipo di entità è definito come in modo:
```Csharp
public class Rider
{
    public int Id { get; set; }

    [Column(TypeName = "nvarchar(24)")]
    public EquineBeast Mount { get; set; }
}
```
Verranno quindi salvati i valori di enumerazione come stringhe nel database senza altre operazioni di configurazione in OnModelCreating.

## <a name="limitations"></a>Limitazioni

Esistono alcune limitazioni correnti di sistema di conversione di valori di:
* Come indicato in precedenza, `null` non può essere convertito.
* Non è attualmente in una conversione di una proprietà in più colonne o viceversa.
* Uso di conversioni di valori potrebbe compromettere la possibilità di EF Core per convertire le espressioni SQL. In questo caso verrà registrato un avviso.
La rimozione di queste limitazioni viene considerata per una versione futura.
