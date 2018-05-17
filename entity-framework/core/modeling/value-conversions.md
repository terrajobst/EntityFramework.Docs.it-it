---
title: Conversioni dei valori - Core a Entity Framework
author: ajcvickers
ms.author: divega
ms.date: 02/19/2018
ms.assetid: 3154BF3C-1749-4C60-8D51-AE86773AA116
ms.technology: entity-framework-core
uid: core/modeling/value-conversions
ms.openlocfilehash: 3e97c05a87ad9b4817c03f446031ea6c74704f5b
ms.sourcegitcommit: 605e42232854ce44bae09624a6eebc35b8e2473b
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/16/2018
---
# <a name="value-conversions"></a><span data-ttu-id="3c27c-102">Conversioni di valori</span><span class="sxs-lookup"><span data-stu-id="3c27c-102">Value Conversions</span></span>

> [!NOTE]  
> <span data-ttu-id="3c27c-103">Questa funzionalità è nuova in Entity Framework Core 2.1.</span><span class="sxs-lookup"><span data-stu-id="3c27c-103">This feature is new in EF Core 2.1.</span></span>

<span data-ttu-id="3c27c-104">Convertitori di valori consentono di convertire durante la lettura o scrittura nel database i valori di proprietà.</span><span class="sxs-lookup"><span data-stu-id="3c27c-104">Value converters allow property values to be converted when reading from or writing to the database.</span></span> <span data-ttu-id="3c27c-105">Questa conversione può provenire da un valore a un altro dello stesso tipo (ad esempio, la crittografia di stringhe) o da un valore di un tipo a un valore di un altro tipo (ad esempio, la conversione dei valori enum in e da stringhe nel database.)</span><span class="sxs-lookup"><span data-stu-id="3c27c-105">This conversion can be from one value to another of the same type (for example, encrypting strings) or from a value of one type to a value of another type (for example, converting enum values to and from strings in the database.)</span></span>

## <a name="fundamentals"></a><span data-ttu-id="3c27c-106">Concetti fondamentali</span><span class="sxs-lookup"><span data-stu-id="3c27c-106">Fundamentals</span></span>

<span data-ttu-id="3c27c-107">Convertitori di valori vengono specificati in termini di un `ModelClrType` e `ProviderClrType`.</span><span class="sxs-lookup"><span data-stu-id="3c27c-107">Value converters are specified in terms of a `ModelClrType` and a `ProviderClrType`.</span></span> <span data-ttu-id="3c27c-108">Il tipo di modello è il tipo .NET della proprietà nel tipo di entità.</span><span class="sxs-lookup"><span data-stu-id="3c27c-108">The model type is the .NET type of the property in the entity type.</span></span> <span data-ttu-id="3c27c-109">Il tipo di provider è il tipo .NET riconosciuto dal provider di database.</span><span class="sxs-lookup"><span data-stu-id="3c27c-109">The provider type is the .NET type understood by the database provider.</span></span> <span data-ttu-id="3c27c-110">Ad esempio, per salvare le enumerazioni sotto forma di stringhe nel database, il tipo di modello è il tipo di enum, e il tipo di provider è `String`.</span><span class="sxs-lookup"><span data-stu-id="3c27c-110">For example, to save enums as strings in the database, the model type is the type of the enum, and the provider type is `String`.</span></span> <span data-ttu-id="3c27c-111">Questi due tipi possono essere uguale.</span><span class="sxs-lookup"><span data-stu-id="3c27c-111">These two types can be the same.</span></span>

<span data-ttu-id="3c27c-112">Le conversioni vengono definite utilizzando due `Func` alberi delle espressioni: uno dal `ModelClrType` a `ProviderClrType` e l'altro da `ProviderClrType` a `ModelClrType`.</span><span class="sxs-lookup"><span data-stu-id="3c27c-112">Conversions are defined using two `Func` expression trees: one from `ModelClrType` to `ProviderClrType` and the other from `ProviderClrType` to `ModelClrType`.</span></span> <span data-ttu-id="3c27c-113">Gli alberi delle espressioni vengono usate in modo che può essere compilati nel codice di accesso del database per le conversioni efficiente.</span><span class="sxs-lookup"><span data-stu-id="3c27c-113">Expression trees are used so that they can be compiled into the database access code for efficient conversions.</span></span> <span data-ttu-id="3c27c-114">Per le conversioni complesse, la struttura ad albero dell'espressione può essere una chiamata semplice a un metodo che esegue la conversione.</span><span class="sxs-lookup"><span data-stu-id="3c27c-114">For complex conversions, the expression tree may be a simple call to a method that performs the conversion.</span></span>

## <a name="configuring-a-value-converter"></a><span data-ttu-id="3c27c-115">Configurazione di un convertitore di valori</span><span class="sxs-lookup"><span data-stu-id="3c27c-115">Configuring a value converter</span></span>

<span data-ttu-id="3c27c-116">Le conversioni di valori sono definite nelle proprietà OnModelCreating del DbContext.</span><span class="sxs-lookup"><span data-stu-id="3c27c-116">Value conversions are defined on properties in the OnModelCreating of your DbContext.</span></span> <span data-ttu-id="3c27c-117">Si consideri, ad esempio, un tipo di entità e di enumerazione definito come:</span><span class="sxs-lookup"><span data-stu-id="3c27c-117">For example, consider an enum and entity type defined as:</span></span>
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
<span data-ttu-id="3c27c-118">Quindi le conversioni possono essere definite in OnModelCreating per archiviare i valori di enumerazione come stringhe (ad esempio "Asino", "Muli",...) nel database:</span><span class="sxs-lookup"><span data-stu-id="3c27c-118">Then conversions can be defined in OnModelCreating to store the enum values as strings (e.g. "Donkey", "Mule", ...) in the database:</span></span>
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
> <span data-ttu-id="3c27c-119">Oggetto `null` valore non verrà mai passato a un convertitore di valori.</span><span class="sxs-lookup"><span data-stu-id="3c27c-119">A `null` value will never be passed to a value converter.</span></span> <span data-ttu-id="3c27c-120">Questo semplifica l'implementazione delle conversioni e consente loro di essere condivisa fra proprietà ammette valori null e non ammette valori null.</span><span class="sxs-lookup"><span data-stu-id="3c27c-120">This makes the implementation of conversions easier and allows them to be shared amongst nullable and non-nullable properties.</span></span>

## <a name="the-valueconverter-class"></a><span data-ttu-id="3c27c-121">La classe ValueConverter</span><span class="sxs-lookup"><span data-stu-id="3c27c-121">The ValueConverter class</span></span>

<span data-ttu-id="3c27c-122">La chiamata `HasConversion` come illustrato in precedenza verrà creato un `ValueConverter` istanza e impostarla sulla proprietà.</span><span class="sxs-lookup"><span data-stu-id="3c27c-122">Calling `HasConversion` as shown above will create a `ValueConverter` instance and set it on the property.</span></span> <span data-ttu-id="3c27c-123">Il `ValueConverter` è invece possibile creare in modo esplicito.</span><span class="sxs-lookup"><span data-stu-id="3c27c-123">The `ValueConverter` can instead be created explicitly.</span></span> <span data-ttu-id="3c27c-124">Ad esempio:</span><span class="sxs-lookup"><span data-stu-id="3c27c-124">For example:</span></span>
```Csharp
var converter = new ValueConverter<EquineBeast, string>(
    v => v.ToString(),
    v => (EquineBeast)Enum.Parse(typeof(EquineBeast), v));

modelBuilder
    .Entity<Rider>()
    .Property(e => e.Mount)
    .HasConversion(converter);
```
<span data-ttu-id="3c27c-125">Può essere utile quando più proprietà utilizzano la stessa conversione.</span><span class="sxs-lookup"><span data-stu-id="3c27c-125">This can be useful when multiple properties use the same conversion.</span></span>

> [!NOTE]  
> <span data-ttu-id="3c27c-126">Attualmente non è possibile specificare in un'unica posizione che tutte le proprietà di un determinato tipo è necessario usare il convertitore stesso.</span><span class="sxs-lookup"><span data-stu-id="3c27c-126">There is currently no way to specify in one place that every property of a given type must use the same value converter.</span></span> <span data-ttu-id="3c27c-127">Questa funzionalità verrà considerata per una versione futura.</span><span class="sxs-lookup"><span data-stu-id="3c27c-127">This feature will be considered for a future release.</span></span>

## <a name="built-in-converters"></a><span data-ttu-id="3c27c-128">Convertitori di tipi incorporati</span><span class="sxs-lookup"><span data-stu-id="3c27c-128">Built-in converters</span></span>

<span data-ttu-id="3c27c-129">EF Core fornito con un set di predefiniti `ValueConverter` classi, il `Microsoft.EntityFrameworkCore.Storage.ValueConversion` dello spazio dei nomi.</span><span class="sxs-lookup"><span data-stu-id="3c27c-129">EF Core ships with a set of pre-defined `ValueConverter` classes, found in the `Microsoft.EntityFrameworkCore.Storage.ValueConversion` namespace.</span></span> <span data-ttu-id="3c27c-130">Questi sono:</span><span class="sxs-lookup"><span data-stu-id="3c27c-130">These are:</span></span>
* <span data-ttu-id="3c27c-131">`BoolToZeroOneConverter` -Bool a zero e uno</span><span class="sxs-lookup"><span data-stu-id="3c27c-131">`BoolToZeroOneConverter` - Bool to zero and one</span></span>
* <span data-ttu-id="3c27c-132">`BoolToStringConverter` -Bool a stringhe, ad esempio "Y" e "N"</span><span class="sxs-lookup"><span data-stu-id="3c27c-132">`BoolToStringConverter` - Bool to strings such as "Y" and "N"</span></span>
* <span data-ttu-id="3c27c-133">`BoolToTwoValuesConverter` -Bool a due valori</span><span class="sxs-lookup"><span data-stu-id="3c27c-133">`BoolToTwoValuesConverter` - Bool to any two values</span></span>
* <span data-ttu-id="3c27c-134">`BytesToStringConverter` -Matrice di byte stringa con codifica Base64</span><span class="sxs-lookup"><span data-stu-id="3c27c-134">`BytesToStringConverter` - Byte array to Base64-encoded string</span></span>
* <span data-ttu-id="3c27c-135">`CastingConverter` -Le conversioni che richiedono solo un cast Csharp</span><span class="sxs-lookup"><span data-stu-id="3c27c-135">`CastingConverter` - Conversions that require only a Csharp cast</span></span>
* <span data-ttu-id="3c27c-136">`CharToStringConverter` -Char unica stringa di caratteri</span><span class="sxs-lookup"><span data-stu-id="3c27c-136">`CharToStringConverter` - Char to single character string</span></span>
* <span data-ttu-id="3c27c-137">`DateTimeOffsetToBinaryConverter` -DateTimeOffset per valore a 64 bit con codifica binaria</span><span class="sxs-lookup"><span data-stu-id="3c27c-137">`DateTimeOffsetToBinaryConverter` - DateTimeOffset to binary-encoded 64-bit value</span></span>
* <span data-ttu-id="3c27c-138">`DateTimeOffsetToBytesConverter` -DateTimeOffset matrice di byte</span><span class="sxs-lookup"><span data-stu-id="3c27c-138">`DateTimeOffsetToBytesConverter` - DateTimeOffset to byte array</span></span>
* <span data-ttu-id="3c27c-139">`DateTimeOffsetToStringConverter` -DateTimeOffset stringa</span><span class="sxs-lookup"><span data-stu-id="3c27c-139">`DateTimeOffsetToStringConverter` - DateTimeOffset to string</span></span>
* <span data-ttu-id="3c27c-140">`DateTimeToBinaryConverter` -DateTime valore a 64 bit inclusi DateTimeKind</span><span class="sxs-lookup"><span data-stu-id="3c27c-140">`DateTimeToBinaryConverter` - DateTime to 64-bit value including DateTimeKind</span></span>
* <span data-ttu-id="3c27c-141">`DateTimeToStringConverter` -DateTime a string</span><span class="sxs-lookup"><span data-stu-id="3c27c-141">`DateTimeToStringConverter` - DateTime to string</span></span>
* <span data-ttu-id="3c27c-142">`DateTimeToTicksConverter` -DateTime con segni di graduazione</span><span class="sxs-lookup"><span data-stu-id="3c27c-142">`DateTimeToTicksConverter` - DateTime to ticks</span></span>
* <span data-ttu-id="3c27c-143">`EnumToNumberConverter` -Enumerazione numero sottostante</span><span class="sxs-lookup"><span data-stu-id="3c27c-143">`EnumToNumberConverter` - Enum to underlying number</span></span>
* <span data-ttu-id="3c27c-144">`EnumToStringConverter` -Enumerazione stringa</span><span class="sxs-lookup"><span data-stu-id="3c27c-144">`EnumToStringConverter` - Enum to string</span></span>
* <span data-ttu-id="3c27c-145">`GuidToBytesConverter` -Guid in matrice di byte</span><span class="sxs-lookup"><span data-stu-id="3c27c-145">`GuidToBytesConverter` - Guid to byte array</span></span>
* <span data-ttu-id="3c27c-146">`GuidToStringConverter` -Guid in stringa</span><span class="sxs-lookup"><span data-stu-id="3c27c-146">`GuidToStringConverter` - Guid to string</span></span>
* <span data-ttu-id="3c27c-147">`NumberToBytesConverter` -Qualsiasi valore numerico di una matrice di byte</span><span class="sxs-lookup"><span data-stu-id="3c27c-147">`NumberToBytesConverter` - Any numerical value to byte array</span></span>
* <span data-ttu-id="3c27c-148">`NumberToStringConverter` -Qualsiasi valore numerico in stringa</span><span class="sxs-lookup"><span data-stu-id="3c27c-148">`NumberToStringConverter` - Any numerical value to string</span></span>
* <span data-ttu-id="3c27c-149">`StringToBytesConverter` -Stringa a byte UTF8</span><span class="sxs-lookup"><span data-stu-id="3c27c-149">`StringToBytesConverter` - String to UTF8 bytes</span></span>
* <span data-ttu-id="3c27c-150">`TimeSpanToStringConverter` -TimeSpan in stringa</span><span class="sxs-lookup"><span data-stu-id="3c27c-150">`TimeSpanToStringConverter` - TimeSpan to string</span></span>
* <span data-ttu-id="3c27c-151">`TimeSpanToTicksConverter` -Intervallo di tempo da segni di graduazione</span><span class="sxs-lookup"><span data-stu-id="3c27c-151">`TimeSpanToTicksConverter` - TimeSpan to ticks</span></span>

<span data-ttu-id="3c27c-152">Si noti che `EnumToStringConverter` è incluso nell'elenco.</span><span class="sxs-lookup"><span data-stu-id="3c27c-152">Notice that `EnumToStringConverter` is included in this list.</span></span> <span data-ttu-id="3c27c-153">Ciò significa che non è necessario specificare la conversione in modo esplicito, come illustrato in precedenza.</span><span class="sxs-lookup"><span data-stu-id="3c27c-153">This means that there is no need to specify the conversion explicitly, as shown above.</span></span> <span data-ttu-id="3c27c-154">Invece, utilizzare il convertitore predefinito:</span><span class="sxs-lookup"><span data-stu-id="3c27c-154">Instead, just use the built-in converter:</span></span>
```Csharp
var converter = new EnumToStringConverter<EquineBeast>();

modelBuilder
    .Entity<Rider>()
    .Property(e => e.Mount)
    .HasConversion(converter);
```
<span data-ttu-id="3c27c-155">Si noti che tutti i convertitori incorporati sono senza stati e pertanto una singola istanza può essere condiviso in modo sicuro da più proprietà.</span><span class="sxs-lookup"><span data-stu-id="3c27c-155">Note that all the built-in converters are stateless and so a single instance can be safely shared by multiple properties.</span></span>

## <a name="pre-defined-conversions"></a><span data-ttu-id="3c27c-156">Conversioni predefinite</span><span class="sxs-lookup"><span data-stu-id="3c27c-156">Pre-defined conversions</span></span>

<span data-ttu-id="3c27c-157">Per le conversioni comuni per il quale esiste un convertitore di tipi incorporati non è necessario specificare in modo esplicito il convertitore.</span><span class="sxs-lookup"><span data-stu-id="3c27c-157">For common conversions for which a built-in converter exists there is no need to specify the converter explicitly.</span></span> <span data-ttu-id="3c27c-158">Al contrario, configurare solo il tipo di provider deve essere utilizzato ed EF verrà utilizzato automaticamente il convertitore di compilazione appropriato.</span><span class="sxs-lookup"><span data-stu-id="3c27c-158">Instead, just configure which provider type should be used and EF will automatically use the appropriate build-in converter.</span></span> <span data-ttu-id="3c27c-159">Enumerazione per le conversioni di stringa vengono utilizzate come un esempio precedente, ma EF verrà effettivamente eseguire questa operazione automaticamente se è configurato il tipo di provider:</span><span class="sxs-lookup"><span data-stu-id="3c27c-159">Enum to string conversions are used as an example above, but EF will actually do this automatically if the provider type is configured:</span></span>
```Csharp
modelBuilder
    .Entity<Rider>()
    .Property(e => e.Mount)
    .HasConversion<string>();
```
<span data-ttu-id="3c27c-160">La stessa operazione può essere ottenuta specificando in modo esplicito il tipo di colonna.</span><span class="sxs-lookup"><span data-stu-id="3c27c-160">The same thing can be achieved by explicitly specifying the column type.</span></span> <span data-ttu-id="3c27c-161">Ad esempio, se il tipo di entità è definito come in modo:</span><span class="sxs-lookup"><span data-stu-id="3c27c-161">For example, if the entity type is defined like so:</span></span>
```Csharp
public class Rider
{
    public int Id { get; set; }

    [Column(TypeName = "nvarchar(24)")]
    public EquineBeast Mount { get; set; }
}
```
<span data-ttu-id="3c27c-162">Verranno quindi salvati i valori di enumerazione come stringhe nel database senza altre operazioni di configurazione in OnModelCreating.</span><span class="sxs-lookup"><span data-stu-id="3c27c-162">Then the enum values will be saved as strings in the database without any further configuration in OnModelCreating.</span></span>

## <a name="limitations"></a><span data-ttu-id="3c27c-163">Limitazioni</span><span class="sxs-lookup"><span data-stu-id="3c27c-163">Limitations</span></span>

<span data-ttu-id="3c27c-164">Esistono alcune limitazioni correnti di sistema di conversione di valori di:</span><span class="sxs-lookup"><span data-stu-id="3c27c-164">There are a few known current limitations of the value convertion system:</span></span>
* <span data-ttu-id="3c27c-165">Come indicato in precedenza, `null` non può essere convertito.</span><span class="sxs-lookup"><span data-stu-id="3c27c-165">As noted above, `null` cannot be converted.</span></span>
* <span data-ttu-id="3c27c-166">Non è attualmente in una conversione di una proprietà in più colonne o viceversa.</span><span class="sxs-lookup"><span data-stu-id="3c27c-166">There is currently no way to spread a conversion of one property to multiple columns or vice-versa.</span></span>
* <span data-ttu-id="3c27c-167">Uso di conversioni di valori potrebbe compromettere la possibilità di EF Core per convertire le espressioni SQL.</span><span class="sxs-lookup"><span data-stu-id="3c27c-167">Use of value conversions may impact the ability of EF Core to translate expressions to SQL.</span></span> <span data-ttu-id="3c27c-168">In questo caso verrà registrato un avviso.</span><span class="sxs-lookup"><span data-stu-id="3c27c-168">A warning will be logged for such cases.</span></span>
<span data-ttu-id="3c27c-169">La rimozione di queste limitazioni viene considerata per una versione futura.</span><span class="sxs-lookup"><span data-stu-id="3c27c-169">Removal of these limitations is being considered for a future release.</span></span>
