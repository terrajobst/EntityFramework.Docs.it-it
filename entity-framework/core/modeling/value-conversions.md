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
# <a name="value-conversions"></a><span data-ttu-id="fc164-102">Conversioni dei valori</span><span class="sxs-lookup"><span data-stu-id="fc164-102">Value Conversions</span></span>

> [!NOTE]  
> <span data-ttu-id="fc164-103">Questa funzionalità è stata introdotta in EF Core 2.1.</span><span class="sxs-lookup"><span data-stu-id="fc164-103">This feature is new in EF Core 2.1.</span></span>

<span data-ttu-id="fc164-104">Convertitori di valori i valori delle proprietà da convertire durante la lettura o la scrittura nel database.</span><span class="sxs-lookup"><span data-stu-id="fc164-104">Value converters allow property values to be converted when reading from or writing to the database.</span></span> <span data-ttu-id="fc164-105">Questa conversione può provenire da un valore a un altro dello stesso tipo (ad esempio, la crittografia di stringhe) o da un valore di un tipo in un valore di un altro tipo (ad esempio, la conversione dei valori enum in e da stringhe nel database.)</span><span class="sxs-lookup"><span data-stu-id="fc164-105">This conversion can be from one value to another of the same type (for example, encrypting strings) or from a value of one type to a value of another type (for example, converting enum values to and from strings in the database.)</span></span>

## <a name="fundamentals"></a><span data-ttu-id="fc164-106">Concetti fondamentali</span><span class="sxs-lookup"><span data-stu-id="fc164-106">Fundamentals</span></span>

<span data-ttu-id="fc164-107">Convertitori di valori vengono specificati in termini di un `ModelClrType` e un `ProviderClrType`.</span><span class="sxs-lookup"><span data-stu-id="fc164-107">Value converters are specified in terms of a `ModelClrType` and a `ProviderClrType`.</span></span> <span data-ttu-id="fc164-108">Il tipo di modello è il tipo di .NET della proprietà nel tipo di entità.</span><span class="sxs-lookup"><span data-stu-id="fc164-108">The model type is the .NET type of the property in the entity type.</span></span> <span data-ttu-id="fc164-109">Il tipo di provider è il tipo .NET riconosciuto dal provider del database.</span><span class="sxs-lookup"><span data-stu-id="fc164-109">The provider type is the .NET type understood by the database provider.</span></span> <span data-ttu-id="fc164-110">Ad esempio, per salvare le enumerazioni come stringhe nel database, il tipo di modello è il tipo di enumerazione e il tipo di provider `String`.</span><span class="sxs-lookup"><span data-stu-id="fc164-110">For example, to save enums as strings in the database, the model type is the type of the enum, and the provider type is `String`.</span></span> <span data-ttu-id="fc164-111">Questi due tipi possono essere lo stesso.</span><span class="sxs-lookup"><span data-stu-id="fc164-111">These two types can be the same.</span></span>

<span data-ttu-id="fc164-112">Le conversioni vengono definite utilizzando due `Func` alberi delle espressioni: uno dal `ModelClrType` a `ProviderClrType` e l'altro `ProviderClrType` a `ModelClrType`.</span><span class="sxs-lookup"><span data-stu-id="fc164-112">Conversions are defined using two `Func` expression trees: one from `ModelClrType` to `ProviderClrType` and the other from `ProviderClrType` to `ModelClrType`.</span></span> <span data-ttu-id="fc164-113">Gli alberi delle espressioni vengono usate in modo che può essere compilati nel codice di accesso del database per le conversioni efficiente.</span><span class="sxs-lookup"><span data-stu-id="fc164-113">Expression trees are used so that they can be compiled into the database access code for efficient conversions.</span></span> <span data-ttu-id="fc164-114">Per le conversioni complesse, l'albero delle espressioni può essere una semplice chiamata a un metodo che esegue la conversione.</span><span class="sxs-lookup"><span data-stu-id="fc164-114">For complex conversions, the expression tree may be a simple call to a method that performs the conversion.</span></span>

## <a name="configuring-a-value-converter"></a><span data-ttu-id="fc164-115">Configurazione di un convertitore di valori</span><span class="sxs-lookup"><span data-stu-id="fc164-115">Configuring a value converter</span></span>

<span data-ttu-id="fc164-116">Le conversioni dei valori definite nella proprietà in OnModelCreating di DbContext.</span><span class="sxs-lookup"><span data-stu-id="fc164-116">Value conversions are defined on properties in the OnModelCreating of your DbContext.</span></span> <span data-ttu-id="fc164-117">Si consideri, ad esempio, un tipo di entità e di enumerazione definito come:</span><span class="sxs-lookup"><span data-stu-id="fc164-117">For example, consider an enum and entity type defined as:</span></span>
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
<span data-ttu-id="fc164-118">Le conversioni possono quindi essere definite in OnModelCreating per archiviare i valori di enumerazione sotto forma di stringhe (ad esempio, "Asino", "Muli",...) nel database:</span><span class="sxs-lookup"><span data-stu-id="fc164-118">Then conversions can be defined in OnModelCreating to store the enum values as strings (for example, "Donkey", "Mule", ...) in the database:</span></span>
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
> <span data-ttu-id="fc164-119">Oggetto `null` valore non verrà mai passato a un convertitore di valori.</span><span class="sxs-lookup"><span data-stu-id="fc164-119">A `null` value will never be passed to a value converter.</span></span> <span data-ttu-id="fc164-120">Questo semplifica l'implementazione delle conversioni e consente loro di essere condivisa fra le proprietà che ammette valori null e non nullable.</span><span class="sxs-lookup"><span data-stu-id="fc164-120">This makes the implementation of conversions easier and allows them to be shared amongst nullable and non-nullable properties.</span></span>

## <a name="the-valueconverter-class"></a><span data-ttu-id="fc164-121">La classe di ValueConverter</span><span class="sxs-lookup"><span data-stu-id="fc164-121">The ValueConverter class</span></span>

<span data-ttu-id="fc164-122">La chiamata `HasConversion` come illustrato in precedenza verrà creato un `ValueConverter` dell'istanza e impostarla sulla proprietà.</span><span class="sxs-lookup"><span data-stu-id="fc164-122">Calling `HasConversion` as shown above will create a `ValueConverter` instance and set it on the property.</span></span> <span data-ttu-id="fc164-123">Il `ValueConverter` invece può essere creato in modo esplicito.</span><span class="sxs-lookup"><span data-stu-id="fc164-123">The `ValueConverter` can instead be created explicitly.</span></span> <span data-ttu-id="fc164-124">Ad esempio:</span><span class="sxs-lookup"><span data-stu-id="fc164-124">For example:</span></span>
```Csharp
var converter = new ValueConverter<EquineBeast, string>(
    v => v.ToString(),
    v => (EquineBeast)Enum.Parse(typeof(EquineBeast), v));

modelBuilder
    .Entity<Rider>()
    .Property(e => e.Mount)
    .HasConversion(converter);
```
<span data-ttu-id="fc164-125">Ciò può essere utile quando più proprietà utilizzano la stessa conversione.</span><span class="sxs-lookup"><span data-stu-id="fc164-125">This can be useful when multiple properties use the same conversion.</span></span>

> [!NOTE]  
> <span data-ttu-id="fc164-126">Non è attualmente possibile specificare in un'unica posizione che tutte le proprietà di un determinato tipo devono usare il convertitore di valore stesso.</span><span class="sxs-lookup"><span data-stu-id="fc164-126">There is currently no way to specify in one place that every property of a given type must use the same value converter.</span></span> <span data-ttu-id="fc164-127">Questa funzionalità verrà considerata una versione futura.</span><span class="sxs-lookup"><span data-stu-id="fc164-127">This feature will be considered for a future release.</span></span>

## <a name="built-in-converters"></a><span data-ttu-id="fc164-128">Convertitori di tipi incorporati</span><span class="sxs-lookup"><span data-stu-id="fc164-128">Built-in converters</span></span>

<span data-ttu-id="fc164-129">EF Core include un set di pre-definite `ValueConverter` classi, presenti nel `Microsoft.EntityFrameworkCore.Storage.ValueConversion` dello spazio dei nomi.</span><span class="sxs-lookup"><span data-stu-id="fc164-129">EF Core ships with a set of pre-defined `ValueConverter` classes, found in the `Microsoft.EntityFrameworkCore.Storage.ValueConversion` namespace.</span></span> <span data-ttu-id="fc164-130">Questi sono:</span><span class="sxs-lookup"><span data-stu-id="fc164-130">These are:</span></span>
* <span data-ttu-id="fc164-131">`BoolToZeroOneConverter` -Bool a zero e uno</span><span class="sxs-lookup"><span data-stu-id="fc164-131">`BoolToZeroOneConverter` - Bool to zero and one</span></span>
* <span data-ttu-id="fc164-132">`BoolToStringConverter` -Bool in stringhe, ad esempio "Y" e "N"</span><span class="sxs-lookup"><span data-stu-id="fc164-132">`BoolToStringConverter` - Bool to strings such as "Y" and "N"</span></span>
* <span data-ttu-id="fc164-133">`BoolToTwoValuesConverter` -Bool a due valori</span><span class="sxs-lookup"><span data-stu-id="fc164-133">`BoolToTwoValuesConverter` - Bool to any two values</span></span>
* <span data-ttu-id="fc164-134">`BytesToStringConverter` -Matrice di byte alla stringa con codifica Base64</span><span class="sxs-lookup"><span data-stu-id="fc164-134">`BytesToStringConverter` - Byte array to Base64-encoded string</span></span>
* <span data-ttu-id="fc164-135">`CastingConverter` -Le conversioni che richiedono solo un cast Csharp</span><span class="sxs-lookup"><span data-stu-id="fc164-135">`CastingConverter` - Conversions that require only a Csharp cast</span></span>
* <span data-ttu-id="fc164-136">`CharToStringConverter` -Char stringa singolo carattere</span><span class="sxs-lookup"><span data-stu-id="fc164-136">`CharToStringConverter` - Char to single character string</span></span>
* <span data-ttu-id="fc164-137">`DateTimeOffsetToBinaryConverter` -DateTimeOffset valore a 64 bit con codifica binaria</span><span class="sxs-lookup"><span data-stu-id="fc164-137">`DateTimeOffsetToBinaryConverter` - DateTimeOffset to binary-encoded 64-bit value</span></span>
* <span data-ttu-id="fc164-138">`DateTimeOffsetToBytesConverter` -DateTimeOffset matrice di byte</span><span class="sxs-lookup"><span data-stu-id="fc164-138">`DateTimeOffsetToBytesConverter` - DateTimeOffset to byte array</span></span>
* <span data-ttu-id="fc164-139">`DateTimeOffsetToStringConverter` -DateTimeOffset alla stringa</span><span class="sxs-lookup"><span data-stu-id="fc164-139">`DateTimeOffsetToStringConverter` - DateTimeOffset to string</span></span>
* <span data-ttu-id="fc164-140">`DateTimeToBinaryConverter` -DateTime valore a 64 bit compresi DateTimeKind</span><span class="sxs-lookup"><span data-stu-id="fc164-140">`DateTimeToBinaryConverter` - DateTime to 64-bit value including DateTimeKind</span></span>
* <span data-ttu-id="fc164-141">`DateTimeToStringConverter` -DateTime a string</span><span class="sxs-lookup"><span data-stu-id="fc164-141">`DateTimeToStringConverter` - DateTime to string</span></span>
* <span data-ttu-id="fc164-142">`DateTimeToTicksConverter` -DateTime con segni di graduazione</span><span class="sxs-lookup"><span data-stu-id="fc164-142">`DateTimeToTicksConverter` - DateTime to ticks</span></span>
* <span data-ttu-id="fc164-143">`EnumToNumberConverter` -Enum numero sottostante</span><span class="sxs-lookup"><span data-stu-id="fc164-143">`EnumToNumberConverter` - Enum to underlying number</span></span>
* <span data-ttu-id="fc164-144">`EnumToStringConverter` -Enum in stringa</span><span class="sxs-lookup"><span data-stu-id="fc164-144">`EnumToStringConverter` - Enum to string</span></span>
* <span data-ttu-id="fc164-145">`GuidToBytesConverter` -Guid in matrice di byte</span><span class="sxs-lookup"><span data-stu-id="fc164-145">`GuidToBytesConverter` - Guid to byte array</span></span>
* <span data-ttu-id="fc164-146">`GuidToStringConverter` -Guid in stringa</span><span class="sxs-lookup"><span data-stu-id="fc164-146">`GuidToStringConverter` - Guid to string</span></span>
* <span data-ttu-id="fc164-147">`NumberToBytesConverter` -Qualsiasi valore numerico in matrice di byte</span><span class="sxs-lookup"><span data-stu-id="fc164-147">`NumberToBytesConverter` - Any numerical value to byte array</span></span>
* <span data-ttu-id="fc164-148">`NumberToStringConverter` -Qualsiasi valore numerico in stringa</span><span class="sxs-lookup"><span data-stu-id="fc164-148">`NumberToStringConverter` - Any numerical value to string</span></span>
* <span data-ttu-id="fc164-149">`StringToBytesConverter` -Stringa a byte UTF8</span><span class="sxs-lookup"><span data-stu-id="fc164-149">`StringToBytesConverter` - String to UTF8 bytes</span></span>
* <span data-ttu-id="fc164-150">`TimeSpanToStringConverter` -TimeSpan in stringa</span><span class="sxs-lookup"><span data-stu-id="fc164-150">`TimeSpanToStringConverter` - TimeSpan to string</span></span>
* <span data-ttu-id="fc164-151">`TimeSpanToTicksConverter` -Intervallo di tempo da segni di graduazione</span><span class="sxs-lookup"><span data-stu-id="fc164-151">`TimeSpanToTicksConverter` - TimeSpan to ticks</span></span>

<span data-ttu-id="fc164-152">Si noti che `EnumToStringConverter` è incluso in questo elenco.</span><span class="sxs-lookup"><span data-stu-id="fc164-152">Notice that `EnumToStringConverter` is included in this list.</span></span> <span data-ttu-id="fc164-153">Ciò significa che non è necessario specificare la conversione in modo esplicito, come illustrato in precedenza.</span><span class="sxs-lookup"><span data-stu-id="fc164-153">This means that there is no need to specify the conversion explicitly, as shown above.</span></span> <span data-ttu-id="fc164-154">Usare invece solo il convertitore incorporato:</span><span class="sxs-lookup"><span data-stu-id="fc164-154">Instead, just use the built-in converter:</span></span>
```Csharp
var converter = new EnumToStringConverter<EquineBeast>();

modelBuilder
    .Entity<Rider>()
    .Property(e => e.Mount)
    .HasConversion(converter);
```
<span data-ttu-id="fc164-155">Si noti che tutti i convertitori predefiniti sono senza stati e pertanto una singola istanza può essere condiviso in modo sicuro da più proprietà.</span><span class="sxs-lookup"><span data-stu-id="fc164-155">Note that all the built-in converters are stateless and so a single instance can be safely shared by multiple properties.</span></span>

## <a name="pre-defined-conversions"></a><span data-ttu-id="fc164-156">Conversioni predefinite</span><span class="sxs-lookup"><span data-stu-id="fc164-156">Pre-defined conversions</span></span>

<span data-ttu-id="fc164-157">Per le conversioni comune per il quale esiste un convertitore di tipi incorporati non è necessario specificare in modo esplicito il convertitore.</span><span class="sxs-lookup"><span data-stu-id="fc164-157">For common conversions for which a built-in converter exists there is no need to specify the converter explicitly.</span></span> <span data-ttu-id="fc164-158">Al contrario, configurare solo il tipo di provider debba essere usate ed Entity Framework la userà automaticamente il convertitore di compilazione appropriato.</span><span class="sxs-lookup"><span data-stu-id="fc164-158">Instead, just configure which provider type should be used and EF will automatically use the appropriate build-in converter.</span></span> <span data-ttu-id="fc164-159">Enumerazione per le conversioni di stringa vengono usati come un esempio precedente, ma EF verranno effettivamente eseguire questa operazione automaticamente se è configurato il tipo di provider:</span><span class="sxs-lookup"><span data-stu-id="fc164-159">Enum to string conversions are used as an example above, but EF will actually do this automatically if the provider type is configured:</span></span>
```Csharp
modelBuilder
    .Entity<Rider>()
    .Property(e => e.Mount)
    .HasConversion<string>();
```
<span data-ttu-id="fc164-160">La stessa operazione, è possibile specificare in modo esplicito il tipo di colonna.</span><span class="sxs-lookup"><span data-stu-id="fc164-160">The same thing can be achieved by explicitly specifying the column type.</span></span> <span data-ttu-id="fc164-161">Ad esempio, se il tipo di entità è definito come in modo che:</span><span class="sxs-lookup"><span data-stu-id="fc164-161">For example, if the entity type is defined like so:</span></span>
```Csharp
public class Rider
{
    public int Id { get; set; }

    [Column(TypeName = "nvarchar(24)")]
    public EquineBeast Mount { get; set; }
}
```
<span data-ttu-id="fc164-162">Quindi verranno salvati come stringhe nel database senza altre operazioni di configurazione in OnModelCreating i valori di enumerazione.</span><span class="sxs-lookup"><span data-stu-id="fc164-162">Then the enum values will be saved as strings in the database without any further configuration in OnModelCreating.</span></span>

## <a name="limitations"></a><span data-ttu-id="fc164-163">Limitazioni</span><span class="sxs-lookup"><span data-stu-id="fc164-163">Limitations</span></span>

<span data-ttu-id="fc164-164">Esistono alcune limitazioni correnti del sistema di conversione del valore:</span><span class="sxs-lookup"><span data-stu-id="fc164-164">There are a few known current limitations of the value convertion system:</span></span>
* <span data-ttu-id="fc164-165">Come indicato in precedenza, `null` non può essere convertito.</span><span class="sxs-lookup"><span data-stu-id="fc164-165">As noted above, `null` cannot be converted.</span></span>
* <span data-ttu-id="fc164-166">Attualmente non è possibile distribuire una conversione di una proprietà in più colonne o viceversa.</span><span class="sxs-lookup"><span data-stu-id="fc164-166">There is currently no way to spread a conversion of one property to multiple columns or vice-versa.</span></span>
* <span data-ttu-id="fc164-167">Uso di conversioni dei valori può influire sulla capacità di EF Core per tradurre le espressioni SQL.</span><span class="sxs-lookup"><span data-stu-id="fc164-167">Use of value conversions may impact the ability of EF Core to translate expressions to SQL.</span></span> <span data-ttu-id="fc164-168">Per questi casi verrà registrato un avviso.</span><span class="sxs-lookup"><span data-stu-id="fc164-168">A warning will be logged for such cases.</span></span>
<span data-ttu-id="fc164-169">Per una versione futura verrà preso in considerazione la rimozione di queste limitazioni.</span><span class="sxs-lookup"><span data-stu-id="fc164-169">Removal of these limitations is being considered for a future release.</span></span>
