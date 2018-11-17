---
title: Dati spaziali - EF Core
author: bricelam
ms.author: bricelam
ms.date: 11/01/2018
ms.assetid: 2BDE29FC-4161-41A0-841E-69F51CCD9341
uid: core/modeling/spatial
ms.openlocfilehash: cf488c6b7d94ca19018efe1c23ff410fe7eb594b
ms.sourcegitcommit: 81c53ac43d8f15b900f117294ec71dc49fe028fa
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/16/2018
ms.locfileid: "51817910"
---
# <a name="spatial-data"></a><span data-ttu-id="7996b-102">Dati spaziali</span><span class="sxs-lookup"><span data-stu-id="7996b-102">Spatial Data</span></span>

> [!NOTE]
> <span data-ttu-id="7996b-103">Questa funzionalità è una novità di EF Core 2.2.</span><span class="sxs-lookup"><span data-stu-id="7996b-103">This feature is new in EF Core 2.2.</span></span>

<span data-ttu-id="7996b-104">I dati spaziali rappresentano la posizione fisica e la forma degli oggetti.</span><span class="sxs-lookup"><span data-stu-id="7996b-104">Spatial data represents the physical location and the shape of objects.</span></span> <span data-ttu-id="7996b-105">Molti database forniscono supporto per questo tipo di dati in modo che può essere indicizzato e sottoposti a query insieme agli altri dati.</span><span class="sxs-lookup"><span data-stu-id="7996b-105">Many databases provide support for this type of data so it can be indexed and queried alongside other data.</span></span> <span data-ttu-id="7996b-106">Gli scenari comuni includono l'esecuzione di query per gli oggetti all'interno di una determinata distanza da un percorso o selezionare l'oggetto il cui bordo contiene una posizione specificata.</span><span class="sxs-lookup"><span data-stu-id="7996b-106">Common scenarios include querying for objects within a given distance from a location, or selecting the object whose border contains a given location.</span></span> <span data-ttu-id="7996b-107">EF Core supporta il mapping ai tipi di dati spaziali utilizzando il [NetTopologySuite](https://github.com/NetTopologySuite/NetTopologySuite) libreria spaziale.</span><span class="sxs-lookup"><span data-stu-id="7996b-107">EF Core supports mapping to spatial data types using the [NetTopologySuite](https://github.com/NetTopologySuite/NetTopologySuite) spatial library.</span></span>

## <a name="installing"></a><span data-ttu-id="7996b-108">Installazione di</span><span class="sxs-lookup"><span data-stu-id="7996b-108">Installing</span></span>

<span data-ttu-id="7996b-109">Per usare i dati spaziali con EF Core, è necessario installare il pacchetto NuGet di supporto appropriato.</span><span class="sxs-lookup"><span data-stu-id="7996b-109">In order to use spatial data with EF Core, you need to install the appropriate supporting NuGet package.</span></span> <span data-ttu-id="7996b-110">Il pacchetto che è necessario installare dipende dal provider in uso.</span><span class="sxs-lookup"><span data-stu-id="7996b-110">Which package you need to install depends on the provider you're using.</span></span>

<span data-ttu-id="7996b-111">Provider di EF Core</span><span class="sxs-lookup"><span data-stu-id="7996b-111">EF Core Provider</span></span>                        | <span data-ttu-id="7996b-112">Pacchetto NuGet spaziali</span><span class="sxs-lookup"><span data-stu-id="7996b-112">Spatial NuGet Package</span></span>
--------------------------------------- | ---------------------
<span data-ttu-id="7996b-113">Entityframeworkcore</span><span class="sxs-lookup"><span data-stu-id="7996b-113">Microsoft.EntityFrameworkCore.SqlServer</span></span> | [<span data-ttu-id="7996b-114">Microsoft.EntityFrameworkCore.SqlServer.NetTopologySuite</span><span class="sxs-lookup"><span data-stu-id="7996b-114">Microsoft.EntityFrameworkCore.SqlServer.NetTopologySuite</span></span>](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.SqlServer.NetTopologySuite)
<span data-ttu-id="7996b-115">Microsoft</span><span class="sxs-lookup"><span data-stu-id="7996b-115">Microsoft.EntityFrameworkCore.Sqlite</span></span>    | [<span data-ttu-id="7996b-116">Microsoft.EntityFrameworkCore.Sqlite.NetTopologySuite</span><span class="sxs-lookup"><span data-stu-id="7996b-116">Microsoft.EntityFrameworkCore.Sqlite.NetTopologySuite</span></span>](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Sqlite.NetTopologySuite)
<span data-ttu-id="7996b-117">Microsoft.EntityFrameworkCore.InMemory</span><span class="sxs-lookup"><span data-stu-id="7996b-117">Microsoft.EntityFrameworkCore.InMemory</span></span>  | [<span data-ttu-id="7996b-118">NetTopologySuite</span><span class="sxs-lookup"><span data-stu-id="7996b-118">NetTopologySuite</span></span>](https://www.nuget.org/packages/NetTopologySuite)
<span data-ttu-id="7996b-119">Npgsql</span><span class="sxs-lookup"><span data-stu-id="7996b-119">Npgsql.EntityFrameworkCore.PostgreSQL</span></span>   | [<span data-ttu-id="7996b-120">Npgsql.EntityFrameworkCore.PostgreSQL.NetTopologySuite</span><span class="sxs-lookup"><span data-stu-id="7996b-120">Npgsql.EntityFrameworkCore.PostgreSQL.NetTopologySuite</span></span>](https://www.nuget.org/packages/Npgsql.EntityFrameworkCore.PostgreSQL.NetTopologySuite)

## <a name="reverse-engineering"></a><span data-ttu-id="7996b-121">Decompilazione</span><span class="sxs-lookup"><span data-stu-id="7996b-121">Reverse engineering</span></span>

<span data-ttu-id="7996b-122">NuGet spaziali pacchetti anche abilitare [decompilazione](../managing-schemas/scaffolding.md) modelli con le proprietà spaziali, ma è necessario installare il pacchetto ***prima*** esecuzione `Scaffold-DbContext` o `dotnet ef dbcontext scaffold`.</span><span class="sxs-lookup"><span data-stu-id="7996b-122">The spatial NuGet packages also enable [reverse engineering](../managing-schemas/scaffolding.md) models with spatial properties, but you need to install the package ***before*** running `Scaffold-DbContext` or `dotnet ef dbcontext scaffold`.</span></span> <span data-ttu-id="7996b-123">In caso contrario, si riceveranno gli avvisi relativi non trova i mapping dei tipi per le colonne e le colonne verranno ignorate.</span><span class="sxs-lookup"><span data-stu-id="7996b-123">If you don't, you'll receive warnings about not finding type mappings for the columns and the columns will be skipped.</span></span>

## <a name="nettopologysuite-nts"></a><span data-ttu-id="7996b-124">NetTopologySuite (NTS)</span><span class="sxs-lookup"><span data-stu-id="7996b-124">NetTopologySuite (NTS)</span></span>

<span data-ttu-id="7996b-125">NetTopologySuite è una libreria spaziale per .NET.</span><span class="sxs-lookup"><span data-stu-id="7996b-125">NetTopologySuite is a spatial library for .NET.</span></span> <span data-ttu-id="7996b-126">EF Core Abilita mapping ai dati spaziali dei tipi nel database utilizzando tipi NTS nel modello.</span><span class="sxs-lookup"><span data-stu-id="7996b-126">EF Core enables mapping to spatial data types in the database by using NTS types in your model.</span></span>

<span data-ttu-id="7996b-127">Per abilitare il mapping a tipi di dati spaziali tramite NTS, chiamare il metodo UseNetTopologySuite sul generatore di opzioni del provider DbContext.</span><span class="sxs-lookup"><span data-stu-id="7996b-127">To enable mapping to spatial types via NTS, call the UseNetTopologySuite method on the provider's DbContext options builder.</span></span> <span data-ttu-id="7996b-128">Ad esempio, con SQL Server è necessario chiamarlo simile al seguente.</span><span class="sxs-lookup"><span data-stu-id="7996b-128">For example, with SQL Server you'd call it like this.</span></span>

``` csharp
optionsBuilder.UseSqlServer(
    @"Data Source=(localdb)\MSSQLLocalDB;Initial Catalog=WideWorldImporters",
    x => x.UseNetTopologySuite());
```

<span data-ttu-id="7996b-129">Esistono diversi tipi di dati spaziali.</span><span class="sxs-lookup"><span data-stu-id="7996b-129">There are several spatial data types.</span></span> <span data-ttu-id="7996b-130">Il tipo da utilizzare dipende dai tipi di forme che si desidera consentire.</span><span class="sxs-lookup"><span data-stu-id="7996b-130">Which type you use depends on the types of shapes you want to allow.</span></span> <span data-ttu-id="7996b-131">Di seguito è riportata la gerarchia dei tipi NTS che è possibile usare per le proprietà nel modello.</span><span class="sxs-lookup"><span data-stu-id="7996b-131">Here is the hierarchy of NTS types that you can use for properties in your model.</span></span> <span data-ttu-id="7996b-132">Si trovano all'interno di `NetTopologySuite.Geometries` dello spazio dei nomi.</span><span class="sxs-lookup"><span data-stu-id="7996b-132">They're located within the `NetTopologySuite.Geometries` namespace.</span></span> <span data-ttu-id="7996b-133">Interfacce corrispondenti nel pacchetto GeoAPI (`GeoAPI.Geometries` dello spazio dei nomi) utilizzabili.</span><span class="sxs-lookup"><span data-stu-id="7996b-133">Corresponding interfaces in the GeoAPI package (`GeoAPI.Geometries` namespace) can also be used.</span></span>

* <span data-ttu-id="7996b-134">Geometry</span><span class="sxs-lookup"><span data-stu-id="7996b-134">Geometry</span></span>
  * <span data-ttu-id="7996b-135">Punto</span><span class="sxs-lookup"><span data-stu-id="7996b-135">Point</span></span>
  * <span data-ttu-id="7996b-136">LineString</span><span class="sxs-lookup"><span data-stu-id="7996b-136">LineString</span></span>
  * <span data-ttu-id="7996b-137">Poligono</span><span class="sxs-lookup"><span data-stu-id="7996b-137">Polygon</span></span>
  * <span data-ttu-id="7996b-138">GeometryCollection</span><span class="sxs-lookup"><span data-stu-id="7996b-138">GeometryCollection</span></span>
    * <span data-ttu-id="7996b-139">MultiPoint</span><span class="sxs-lookup"><span data-stu-id="7996b-139">MultiPoint</span></span>
    * <span data-ttu-id="7996b-140">MultiLineString</span><span class="sxs-lookup"><span data-stu-id="7996b-140">MultiLineString</span></span>
    * <span data-ttu-id="7996b-141">MultiPolygon</span><span class="sxs-lookup"><span data-stu-id="7996b-141">MultiPolygon</span></span>

> [!WARNING]
> <span data-ttu-id="7996b-142">CircularString e CompoundCurve CurePolygon non sono supportati da NTS.</span><span class="sxs-lookup"><span data-stu-id="7996b-142">CircularString, CompoundCurve, and CurePolygon aren't supported by NTS.</span></span>

<span data-ttu-id="7996b-143">Utilizzando il tipo di geometria di base consente qualsiasi tipo di forma da essere specificato dalla proprietà.</span><span class="sxs-lookup"><span data-stu-id="7996b-143">Using the base Geometry type allows any type of shape to be specified by the property.</span></span>

<span data-ttu-id="7996b-144">Le seguenti classi di entità può essere utilizzate per eseguire il mapping alle tabelle nel [database di esempio Wide World Importers](http://go.microsoft.com/fwlink/?LinkID=800630).</span><span class="sxs-lookup"><span data-stu-id="7996b-144">The following entity classes could be used to map to tables in the [Wide World Importers sample database](http://go.microsoft.com/fwlink/?LinkID=800630).</span></span>

``` csharp
[Table("Cities", Schema = "Application"))]
class City
{
    public int CityID { get; set; }

    public string CityName { get; set; }

    public IPoint Location { get; set; }
}

[Table("Countries", Schema = "Application"))]
class Country
{
    public int CountryID { get; set; }

    public string CountryName { get; set; }

    // Database includes both Polygon and MultiPolygon values
    public IGeometry Border { get; set; }
}
```

### <a name="creating-values"></a><span data-ttu-id="7996b-145">Creazione di valori</span><span class="sxs-lookup"><span data-stu-id="7996b-145">Creating values</span></span>

<span data-ttu-id="7996b-146">È possibile usare i costruttori per creare oggetti geometry. Tuttavia, NTS consiglia di usare invece una factory di geometria.</span><span class="sxs-lookup"><span data-stu-id="7996b-146">You can use constructors to create geometry objects; however, NTS recommends using a geometry factory instead.</span></span> <span data-ttu-id="7996b-147">Ciò consente di specificare un identificatore predefinito SRID (il sistema di riferimento spaziale usato dalle coordinate) e consente di controllare aspetti più avanzati, ad esempio il modello di precisione (usato durante i calcoli) e la sequenza di coordinate (determina quali coordinate: le dimensioni e sono disponibili misure).</span><span class="sxs-lookup"><span data-stu-id="7996b-147">This lets you specify a default SRID (the spatial reference system used by the coordinates) and gives you control over more advanced things like the precision model (used during calculations) and the coordinate sequence (determines which ordinates--dimensions and measures--are available).</span></span>

``` csharp
var geometryFactory = NtsGeometryServices.Instance.CreateGeometryFactory(srid: 4326);
var currentLocation = geometryFactory.CreatePoint(-122.121512, 47.6739882);
```

> [!NOTE]
> <span data-ttu-id="7996b-148">4326 fa riferimento a WGS 84, uno standard usato in GPS e altri sistemi geografici.</span><span class="sxs-lookup"><span data-stu-id="7996b-148">4326 refers to WGS 84, a standard used in GPS and other geographic systems.</span></span>

### <a name="longitude-and-latitude"></a><span data-ttu-id="7996b-149">Indicata con longitudine e latitudine</span><span class="sxs-lookup"><span data-stu-id="7996b-149">Longitude and Latitude</span></span>

<span data-ttu-id="7996b-150">Coordinate NTS sono in termini di valori X e Y.</span><span class="sxs-lookup"><span data-stu-id="7996b-150">Coordinates in NTS are in terms of X and Y values.</span></span> <span data-ttu-id="7996b-151">Per rappresentare la longitudine e latitudine, usare X per Y e la longitudine della latitudine.</span><span class="sxs-lookup"><span data-stu-id="7996b-151">To represent longitude and latitude, use X for longitude and Y for latitude.</span></span> <span data-ttu-id="7996b-152">Si noti che si tratta **con le versioni precedenti** dal `latitude, longitude` formato in cui è in genere visualizzare questi valori.</span><span class="sxs-lookup"><span data-stu-id="7996b-152">Note that this is **backwards** from the `latitude, longitude` format in which you typically see these values.</span></span>

### <a name="srid-ignored-during-client-operations"></a><span data-ttu-id="7996b-153">Identificatore SRID ignorato durante le operazioni client</span><span class="sxs-lookup"><span data-stu-id="7996b-153">SRID Ignored during client operations</span></span>

<span data-ttu-id="7996b-154">NTS ignora i valori di identificatore SRID durante le operazioni.</span><span class="sxs-lookup"><span data-stu-id="7996b-154">NTS ignores SRID values during operations.</span></span> <span data-ttu-id="7996b-155">Si presuppone che un sistema di coordinate planare.</span><span class="sxs-lookup"><span data-stu-id="7996b-155">It assumes a planar coordinate system.</span></span> <span data-ttu-id="7996b-156">Ciò significa che se si specificano le coordinate in termini di longitudine e latitudine, alcuni valori valutati client, ad esempio distanza, la lunghezza e area sarà espresso in gradi, non i contatori.</span><span class="sxs-lookup"><span data-stu-id="7996b-156">This means that if you specify coordinates in terms of longitude and latitude, some client-evaluated values like distance, length, and area will be in degrees, not meters.</span></span> <span data-ttu-id="7996b-157">Per i valori più significativi, è innanzitutto necessario proiettare le coordinate da un altro sistema di coordinate tramite una libreria come [ProjNet4GeoAPI](https://github.com/NetTopologySuite/ProjNet4GeoAPI) prima di calcolare tali valori.</span><span class="sxs-lookup"><span data-stu-id="7996b-157">For more meaningful values, you first need to project the coordinates to another coordinate system using a library like [ProjNet4GeoAPI](https://github.com/NetTopologySuite/ProjNet4GeoAPI) before calculating these values.</span></span>

<span data-ttu-id="7996b-158">Se un'operazione è valutato da Entity Framework Core con SQL server, unità del risultato verrà determinato dal database.</span><span class="sxs-lookup"><span data-stu-id="7996b-158">If an operation is server-evaluated by EF Core via SQL, the result's unit will be determined by the database.</span></span>

<span data-ttu-id="7996b-159">Ecco un esempio dell'uso ProjNet4GeoAPI per calcolare la distanza tra due città.</span><span class="sxs-lookup"><span data-stu-id="7996b-159">Here is an example of using ProjNet4GeoAPI to calculate the distance between two cities.</span></span>

``` csharp
static class GeometryExtensions
{
    static readonly IGeometryServices _geometryServices = NtsGeometryServices.Instance;
    static readonly ICoordinateSystemServices _coordinateSystemServices
        = new CoordinateSystemServices(
            new CoordinateSystemFactory(),
            new CoordinateTransformationFactory(),
            new Dictionary<int, string>
            {
                // Coordinate systems:

                // (3857 and 4326 included automatically)

                // This coordinate system covers the area of our data.
                // Different data requires a different coordinate system.
                [2855] =
                @"
                    PROJCS[""NAD83(HARN) / Washington North"",
                        GEOGCS[""NAD83(HARN)"",
                            DATUM[""NAD83_High_Accuracy_Regional_Network"",
                                SPHEROID[""GRS 1980"",6378137,298.257222101,
                                    AUTHORITY[""EPSG"",""7019""]],
                                AUTHORITY[""EPSG"",""6152""]],
                            PRIMEM[""Greenwich"",0,
                                AUTHORITY[""EPSG"",""8901""]],
                            UNIT[""degree"",0.01745329251994328,
                                AUTHORITY[""EPSG"",""9122""]],
                            AUTHORITY[""EPSG"",""4152""]],
                        PROJECTION[""Lambert_Conformal_Conic_2SP""],
                        PARAMETER[""standard_parallel_1"",48.73333333333333],
                        PARAMETER[""standard_parallel_2"",47.5],
                        PARAMETER[""latitude_of_origin"",47],
                        PARAMETER[""central_meridian"",-120.8333333333333],
                        PARAMETER[""false_easting"",500000],
                        PARAMETER[""false_northing"",0],
                        UNIT[""metre"",1,
                            AUTHORITY[""EPSG"",""9001""]],
                        AUTHORITY[""EPSG"",""2855""]]
                "
            });

    public static IGeometry ProjectTo(this IGeometry geometry, int srid)
    {
        var geometryFactory = _geometryServices.CreateGeometryFactory(srid);
        var transformation = _coordinateSystemServices.CreateTransformation(geometry.SRID, srid);

        return GeometryTransform.TransformGeometry(
            geometryFactory,
            geometry,
            transformation.MathTransform);
    }
}
```

``` csharp
var seattle = new Point(-122.333056, 47.609722) { SRID = 4326 };
var redmond = new Point(-122.123889, 47.669444) { SRID = 4326 };

var distance = seattle.ProjectTo(2855).Distance(redmond.ProjectTo(2855));
```

## <a name="querying-data"></a><span data-ttu-id="7996b-160">Esecuzione di query su dati</span><span class="sxs-lookup"><span data-stu-id="7996b-160">Querying Data</span></span>

<span data-ttu-id="7996b-161">In LINQ, i metodi NTS e le proprietà disponibili come funzioni di database verranno tradotto in SQL.</span><span class="sxs-lookup"><span data-stu-id="7996b-161">In LINQ, the NTS methods and properties available as database functions will be translated to SQL.</span></span> <span data-ttu-id="7996b-162">Ad esempio, i metodi di distanza e Contains vengono convertiti nelle query seguenti.</span><span class="sxs-lookup"><span data-stu-id="7996b-162">For example, the Distance and Contains methods are translated in the following queries.</span></span> <span data-ttu-id="7996b-163">La tabella alla fine di questo articolo illustra quali membri sono supportati dai diversi provider di EF Core.</span><span class="sxs-lookup"><span data-stu-id="7996b-163">The table at the end of this article shows which members are supported by various EF Core providers.</span></span>

``` csharp
var nearestCity = db.Cities
    .OrderBy(c => c.Location.Distance(currentLocation))
    .FirstOrDefault();

var currentCountry = db.Countries
    .FirstOrDefault(c => c.Border.Contains(currentLocation));
```

## <a name="sql-server"></a><span data-ttu-id="7996b-164">SQL Server</span><span class="sxs-lookup"><span data-stu-id="7996b-164">SQL Server</span></span>

<span data-ttu-id="7996b-165">Se si usa SQL Server, esistono alcuni aspetti aggiuntivi di che è necessario essere consapevoli.</span><span class="sxs-lookup"><span data-stu-id="7996b-165">If you're using SQL Server, there are some additional things you should be aware of.</span></span>

### <a name="geography-or-geometry"></a><span data-ttu-id="7996b-166">Geografia o geometria</span><span class="sxs-lookup"><span data-stu-id="7996b-166">Geography or geometry</span></span>

<span data-ttu-id="7996b-167">Per impostazione predefinita, le proprietà spaziali sono mappate a `geography` colonne in SQL Server.</span><span class="sxs-lookup"><span data-stu-id="7996b-167">By default, spatial properties are mapped to `geography` columns in SQL Server.</span></span> <span data-ttu-id="7996b-168">Per utilizzare `geometry`, [configurare il tipo di colonna](xref:core/modeling/relational/data-types) nel modello.</span><span class="sxs-lookup"><span data-stu-id="7996b-168">To use `geometry`, [configure the column type](xref:core/modeling/relational/data-types) in your model.</span></span>

### <a name="geography-polygon-rings"></a><span data-ttu-id="7996b-169">Anelli di poligono di geografia</span><span class="sxs-lookup"><span data-stu-id="7996b-169">Geography polygon rings</span></span>

<span data-ttu-id="7996b-170">Quando si usa il `geography` tipo di colonna, SQL Server impone requisiti aggiuntivi per l'anello esterno (o shell) e interni anelli (o aree libere).</span><span class="sxs-lookup"><span data-stu-id="7996b-170">When using the `geography` column type, SQL Server imposes additional requirements on the exterior ring (or shell) and interior rings (or holes).</span></span> <span data-ttu-id="7996b-171">L'anello esterno deve essere orientata in senso antiorario e l'interno gli anelli in senso orario.</span><span class="sxs-lookup"><span data-stu-id="7996b-171">The exterior ring must be oriented counterclockwise and the interior rings clockwise.</span></span> <span data-ttu-id="7996b-172">NTS questa convalida prima di inviare i valori per il database.</span><span class="sxs-lookup"><span data-stu-id="7996b-172">NTS validates this before sending values to the database.</span></span>

### <a name="fullglobe"></a><span data-ttu-id="7996b-173">FullGlobe</span><span class="sxs-lookup"><span data-stu-id="7996b-173">FullGlobe</span></span>

<span data-ttu-id="7996b-174">SQL Server dispone di un tipo di geometria non standard per rappresentare l'intero globo quando si usa il `geography` tipo della colonna.</span><span class="sxs-lookup"><span data-stu-id="7996b-174">SQL Server has a non-standard geometry type to represent the full globe when using the `geography` column type.</span></span> <span data-ttu-id="7996b-175">Include inoltre un modo per rappresentare i poligoni in base l'intero globo (senza un anello esterno).</span><span class="sxs-lookup"><span data-stu-id="7996b-175">It also has a way to represent polygons based on the full globe (without an exterior ring).</span></span> <span data-ttu-id="7996b-176">Nessuna di queste sono supportata da NTS.</span><span class="sxs-lookup"><span data-stu-id="7996b-176">Neither of these are supported by NTS.</span></span>

> [!WARNING]
> <span data-ttu-id="7996b-177">FullGlobe e poligoni basati su di essa non sono supportati da NTS.</span><span class="sxs-lookup"><span data-stu-id="7996b-177">FullGlobe and polygons based on it aren't supported by NTS.</span></span>

## <a name="sqlite"></a><span data-ttu-id="7996b-178">SQLite</span><span class="sxs-lookup"><span data-stu-id="7996b-178">SQLite</span></span>

<span data-ttu-id="7996b-179">Ecco alcune informazioni aggiuntive per chi usa SQLite.</span><span class="sxs-lookup"><span data-stu-id="7996b-179">Here is some additional information for those using SQLite.</span></span>

### <a name="installing-spatialite"></a><span data-ttu-id="7996b-180">Installazione SpatiaLite</span><span class="sxs-lookup"><span data-stu-id="7996b-180">Installing SpatiaLite</span></span>

<span data-ttu-id="7996b-181">In Windows, la libreria nativa mod_spatialite viene distribuita come una dipendenza del pacchetto NuGet.</span><span class="sxs-lookup"><span data-stu-id="7996b-181">On Windows, the native mod_spatialite library is distributed as a NuGet package dependency.</span></span> <span data-ttu-id="7996b-182">Altre piattaforme è necessario installarlo separatamente.</span><span class="sxs-lookup"><span data-stu-id="7996b-182">Other platforms need to install it separately.</span></span> <span data-ttu-id="7996b-183">Questa operazione viene in genere eseguita usando un gestore di pacchetti software.</span><span class="sxs-lookup"><span data-stu-id="7996b-183">This is typically done using a software package manager.</span></span> <span data-ttu-id="7996b-184">Ad esempio, è possibile usare APT in Ubuntu e Homebrew in MacOS.</span><span class="sxs-lookup"><span data-stu-id="7996b-184">For example, you can use APT on Ubuntu and Homebrew on MacOS.</span></span>

``` sh
# Ubuntu
apt-get install libsqlite3-mod-spatialite

# macOS
brew install libspatialite
```

### <a name="configuring-srid"></a><span data-ttu-id="7996b-185">Configurazione di identificatore SRID</span><span class="sxs-lookup"><span data-stu-id="7996b-185">Configuring SRID</span></span>

<span data-ttu-id="7996b-186">In SpatiaLite, le colonne necessarie specificare un identificatore SRID per ogni colonna.</span><span class="sxs-lookup"><span data-stu-id="7996b-186">In SpatiaLite, columns need to specify an SRID per column.</span></span> <span data-ttu-id="7996b-187">Il valore predefinito è SRID `0`.</span><span class="sxs-lookup"><span data-stu-id="7996b-187">The default SRID is `0`.</span></span> <span data-ttu-id="7996b-188">Specificare un diverso SRID utilizzando il metodo ForSqliteHasSrid.</span><span class="sxs-lookup"><span data-stu-id="7996b-188">Specify a different SRID using the ForSqliteHasSrid method.</span></span>

``` csharp
modelBuilder.Entity<City>().Property(c => c.Location)
    .ForSqliteHasSrid(4326);
```

### <a name="dimension"></a><span data-ttu-id="7996b-189">Dimensione</span><span class="sxs-lookup"><span data-stu-id="7996b-189">Dimension</span></span>

<span data-ttu-id="7996b-190">Simile a identificatore SRID, dimensione di una colonna (o nelle coordinate) viene specificate anche come parte della colonna.</span><span class="sxs-lookup"><span data-stu-id="7996b-190">Similar to SRID, a column's dimension (or ordinates) is also specified as part of the column.</span></span> <span data-ttu-id="7996b-191">Le coordinate predefinito sono X e y. per abilitare aggiuntive nelle coordinate (Z e M) usando il metodo ForSqliteHasDimension.</span><span class="sxs-lookup"><span data-stu-id="7996b-191">The default ordinates are X and Y. Enable additional ordinates (Z and M) using the ForSqliteHasDimension method.</span></span>

``` csharp
modelBuilder.Entity<City>().Property(c => c.Location)
    .ForSqliteHasDimension(Ordinates.XYZ);
```

## <a name="translated-operations"></a><span data-ttu-id="7996b-192">Operazioni tradotte</span><span class="sxs-lookup"><span data-stu-id="7996b-192">Translated Operations</span></span>

<span data-ttu-id="7996b-193">Questa tabella mostra i membri cui NTS vengono convertiti in SQL da ogni provider di EF Core.</span><span class="sxs-lookup"><span data-stu-id="7996b-193">This table shows which NTS members are translated into SQL by each EF Core provider.</span></span>

<span data-ttu-id="7996b-194">NetTopologySuite</span><span class="sxs-lookup"><span data-stu-id="7996b-194">NetTopologySuite</span></span> | <span data-ttu-id="7996b-195">SQL Server (geometry)</span><span class="sxs-lookup"><span data-stu-id="7996b-195">SQL Server (geometry)</span></span> | <span data-ttu-id="7996b-196">SQL Server (geography)</span><span class="sxs-lookup"><span data-stu-id="7996b-196">SQL Server (geography)</span></span> | <span data-ttu-id="7996b-197">SQLite</span><span class="sxs-lookup"><span data-stu-id="7996b-197">SQLite</span></span> | <span data-ttu-id="7996b-198">Npgsql</span><span class="sxs-lookup"><span data-stu-id="7996b-198">Npgsql</span></span>
--- |:---:|:---:|:---:|:---:
<span data-ttu-id="7996b-199">Geometry.Area</span><span class="sxs-lookup"><span data-stu-id="7996b-199">Geometry.Area</span></span> | <span data-ttu-id="7996b-200">✔</span><span class="sxs-lookup"><span data-stu-id="7996b-200">✔</span></span> | <span data-ttu-id="7996b-201">✔</span><span class="sxs-lookup"><span data-stu-id="7996b-201">✔</span></span> | <span data-ttu-id="7996b-202">✔</span><span class="sxs-lookup"><span data-stu-id="7996b-202">✔</span></span> | <span data-ttu-id="7996b-203">✔</span><span class="sxs-lookup"><span data-stu-id="7996b-203">✔</span></span>
<span data-ttu-id="7996b-204">Geometry.AsBinary()</span><span class="sxs-lookup"><span data-stu-id="7996b-204">Geometry.AsBinary()</span></span> | <span data-ttu-id="7996b-205">✔</span><span class="sxs-lookup"><span data-stu-id="7996b-205">✔</span></span> | <span data-ttu-id="7996b-206">✔</span><span class="sxs-lookup"><span data-stu-id="7996b-206">✔</span></span> | <span data-ttu-id="7996b-207">✔</span><span class="sxs-lookup"><span data-stu-id="7996b-207">✔</span></span> | <span data-ttu-id="7996b-208">✔</span><span class="sxs-lookup"><span data-stu-id="7996b-208">✔</span></span>
<span data-ttu-id="7996b-209">Geometry.AsText()</span><span class="sxs-lookup"><span data-stu-id="7996b-209">Geometry.AsText()</span></span> | <span data-ttu-id="7996b-210">✔</span><span class="sxs-lookup"><span data-stu-id="7996b-210">✔</span></span> | <span data-ttu-id="7996b-211">✔</span><span class="sxs-lookup"><span data-stu-id="7996b-211">✔</span></span> | <span data-ttu-id="7996b-212">✔</span><span class="sxs-lookup"><span data-stu-id="7996b-212">✔</span></span> | <span data-ttu-id="7996b-213">✔</span><span class="sxs-lookup"><span data-stu-id="7996b-213">✔</span></span>
<span data-ttu-id="7996b-214">Geometry.Boundary</span><span class="sxs-lookup"><span data-stu-id="7996b-214">Geometry.Boundary</span></span> | <span data-ttu-id="7996b-215">✔</span><span class="sxs-lookup"><span data-stu-id="7996b-215">✔</span></span> | | <span data-ttu-id="7996b-216">✔</span><span class="sxs-lookup"><span data-stu-id="7996b-216">✔</span></span> | <span data-ttu-id="7996b-217">✔</span><span class="sxs-lookup"><span data-stu-id="7996b-217">✔</span></span>
<span data-ttu-id="7996b-218">Geometry.Buffer(double)</span><span class="sxs-lookup"><span data-stu-id="7996b-218">Geometry.Buffer(double)</span></span> | <span data-ttu-id="7996b-219">✔</span><span class="sxs-lookup"><span data-stu-id="7996b-219">✔</span></span> | <span data-ttu-id="7996b-220">✔</span><span class="sxs-lookup"><span data-stu-id="7996b-220">✔</span></span> | <span data-ttu-id="7996b-221">✔</span><span class="sxs-lookup"><span data-stu-id="7996b-221">✔</span></span> | <span data-ttu-id="7996b-222">✔</span><span class="sxs-lookup"><span data-stu-id="7996b-222">✔</span></span>
<span data-ttu-id="7996b-223">Geometry.Buffer (double, int)</span><span class="sxs-lookup"><span data-stu-id="7996b-223">Geometry.Buffer(double, int)</span></span> | | | <span data-ttu-id="7996b-224">✔</span><span class="sxs-lookup"><span data-stu-id="7996b-224">✔</span></span>
<span data-ttu-id="7996b-225">Geometry.Centroid</span><span class="sxs-lookup"><span data-stu-id="7996b-225">Geometry.Centroid</span></span> | <span data-ttu-id="7996b-226">✔</span><span class="sxs-lookup"><span data-stu-id="7996b-226">✔</span></span> | | <span data-ttu-id="7996b-227">✔</span><span class="sxs-lookup"><span data-stu-id="7996b-227">✔</span></span> | <span data-ttu-id="7996b-228">✔</span><span class="sxs-lookup"><span data-stu-id="7996b-228">✔</span></span>
<span data-ttu-id="7996b-229">Geometry.Contains(Geometry)</span><span class="sxs-lookup"><span data-stu-id="7996b-229">Geometry.Contains(Geometry)</span></span> | <span data-ttu-id="7996b-230">✔</span><span class="sxs-lookup"><span data-stu-id="7996b-230">✔</span></span> | <span data-ttu-id="7996b-231">✔</span><span class="sxs-lookup"><span data-stu-id="7996b-231">✔</span></span> | <span data-ttu-id="7996b-232">✔</span><span class="sxs-lookup"><span data-stu-id="7996b-232">✔</span></span> | <span data-ttu-id="7996b-233">✔</span><span class="sxs-lookup"><span data-stu-id="7996b-233">✔</span></span>
<span data-ttu-id="7996b-234">Geometry.ConvexHull()</span><span class="sxs-lookup"><span data-stu-id="7996b-234">Geometry.ConvexHull()</span></span> | <span data-ttu-id="7996b-235">✔</span><span class="sxs-lookup"><span data-stu-id="7996b-235">✔</span></span> | <span data-ttu-id="7996b-236">✔</span><span class="sxs-lookup"><span data-stu-id="7996b-236">✔</span></span> | <span data-ttu-id="7996b-237">✔</span><span class="sxs-lookup"><span data-stu-id="7996b-237">✔</span></span> | <span data-ttu-id="7996b-238">✔</span><span class="sxs-lookup"><span data-stu-id="7996b-238">✔</span></span>
<span data-ttu-id="7996b-239">Geometry.CoveredBy(Geometry)</span><span class="sxs-lookup"><span data-stu-id="7996b-239">Geometry.CoveredBy(Geometry)</span></span> | | | <span data-ttu-id="7996b-240">✔</span><span class="sxs-lookup"><span data-stu-id="7996b-240">✔</span></span> | <span data-ttu-id="7996b-241">✔</span><span class="sxs-lookup"><span data-stu-id="7996b-241">✔</span></span>
<span data-ttu-id="7996b-242">Geometry.Covers(Geometry)</span><span class="sxs-lookup"><span data-stu-id="7996b-242">Geometry.Covers(Geometry)</span></span> | | | <span data-ttu-id="7996b-243">✔</span><span class="sxs-lookup"><span data-stu-id="7996b-243">✔</span></span> | <span data-ttu-id="7996b-244">✔</span><span class="sxs-lookup"><span data-stu-id="7996b-244">✔</span></span>
<span data-ttu-id="7996b-245">Geometry.Crosses(Geometry)</span><span class="sxs-lookup"><span data-stu-id="7996b-245">Geometry.Crosses(Geometry)</span></span> | <span data-ttu-id="7996b-246">✔</span><span class="sxs-lookup"><span data-stu-id="7996b-246">✔</span></span> | | <span data-ttu-id="7996b-247">✔</span><span class="sxs-lookup"><span data-stu-id="7996b-247">✔</span></span> | <span data-ttu-id="7996b-248">✔</span><span class="sxs-lookup"><span data-stu-id="7996b-248">✔</span></span>
<span data-ttu-id="7996b-249">Geometry.Difference(Geometry)</span><span class="sxs-lookup"><span data-stu-id="7996b-249">Geometry.Difference(Geometry)</span></span> | <span data-ttu-id="7996b-250">✔</span><span class="sxs-lookup"><span data-stu-id="7996b-250">✔</span></span> | <span data-ttu-id="7996b-251">✔</span><span class="sxs-lookup"><span data-stu-id="7996b-251">✔</span></span> | <span data-ttu-id="7996b-252">✔</span><span class="sxs-lookup"><span data-stu-id="7996b-252">✔</span></span> | <span data-ttu-id="7996b-253">✔</span><span class="sxs-lookup"><span data-stu-id="7996b-253">✔</span></span>
<span data-ttu-id="7996b-254">Geometry.Dimension</span><span class="sxs-lookup"><span data-stu-id="7996b-254">Geometry.Dimension</span></span> | <span data-ttu-id="7996b-255">✔</span><span class="sxs-lookup"><span data-stu-id="7996b-255">✔</span></span> | <span data-ttu-id="7996b-256">✔</span><span class="sxs-lookup"><span data-stu-id="7996b-256">✔</span></span> | <span data-ttu-id="7996b-257">✔</span><span class="sxs-lookup"><span data-stu-id="7996b-257">✔</span></span> | <span data-ttu-id="7996b-258">✔</span><span class="sxs-lookup"><span data-stu-id="7996b-258">✔</span></span>
<span data-ttu-id="7996b-259">Geometry.Disjoint(Geometry)</span><span class="sxs-lookup"><span data-stu-id="7996b-259">Geometry.Disjoint(Geometry)</span></span> | <span data-ttu-id="7996b-260">✔</span><span class="sxs-lookup"><span data-stu-id="7996b-260">✔</span></span> | <span data-ttu-id="7996b-261">✔</span><span class="sxs-lookup"><span data-stu-id="7996b-261">✔</span></span> | <span data-ttu-id="7996b-262">✔</span><span class="sxs-lookup"><span data-stu-id="7996b-262">✔</span></span> | <span data-ttu-id="7996b-263">✔</span><span class="sxs-lookup"><span data-stu-id="7996b-263">✔</span></span>
<span data-ttu-id="7996b-264">Geometry.Distance(Geometry)</span><span class="sxs-lookup"><span data-stu-id="7996b-264">Geometry.Distance(Geometry)</span></span> | <span data-ttu-id="7996b-265">✔</span><span class="sxs-lookup"><span data-stu-id="7996b-265">✔</span></span> | <span data-ttu-id="7996b-266">✔</span><span class="sxs-lookup"><span data-stu-id="7996b-266">✔</span></span> | <span data-ttu-id="7996b-267">✔</span><span class="sxs-lookup"><span data-stu-id="7996b-267">✔</span></span> | <span data-ttu-id="7996b-268">✔</span><span class="sxs-lookup"><span data-stu-id="7996b-268">✔</span></span>
<span data-ttu-id="7996b-269">Geometry.Envelope</span><span class="sxs-lookup"><span data-stu-id="7996b-269">Geometry.Envelope</span></span> | <span data-ttu-id="7996b-270">✔</span><span class="sxs-lookup"><span data-stu-id="7996b-270">✔</span></span> | | <span data-ttu-id="7996b-271">✔</span><span class="sxs-lookup"><span data-stu-id="7996b-271">✔</span></span> | <span data-ttu-id="7996b-272">✔</span><span class="sxs-lookup"><span data-stu-id="7996b-272">✔</span></span>
<span data-ttu-id="7996b-273">Geometry.EqualsExact(Geometry)</span><span class="sxs-lookup"><span data-stu-id="7996b-273">Geometry.EqualsExact(Geometry)</span></span> | | | | <span data-ttu-id="7996b-274">✔</span><span class="sxs-lookup"><span data-stu-id="7996b-274">✔</span></span>
<span data-ttu-id="7996b-275">Geometry.EqualsTopologically(Geometry)</span><span class="sxs-lookup"><span data-stu-id="7996b-275">Geometry.EqualsTopologically(Geometry)</span></span> | <span data-ttu-id="7996b-276">✔</span><span class="sxs-lookup"><span data-stu-id="7996b-276">✔</span></span> | <span data-ttu-id="7996b-277">✔</span><span class="sxs-lookup"><span data-stu-id="7996b-277">✔</span></span> | <span data-ttu-id="7996b-278">✔</span><span class="sxs-lookup"><span data-stu-id="7996b-278">✔</span></span> | <span data-ttu-id="7996b-279">✔</span><span class="sxs-lookup"><span data-stu-id="7996b-279">✔</span></span>
<span data-ttu-id="7996b-280">Geometry.GeometryType</span><span class="sxs-lookup"><span data-stu-id="7996b-280">Geometry.GeometryType</span></span> | <span data-ttu-id="7996b-281">✔</span><span class="sxs-lookup"><span data-stu-id="7996b-281">✔</span></span> | <span data-ttu-id="7996b-282">✔</span><span class="sxs-lookup"><span data-stu-id="7996b-282">✔</span></span> | <span data-ttu-id="7996b-283">✔</span><span class="sxs-lookup"><span data-stu-id="7996b-283">✔</span></span> | <span data-ttu-id="7996b-284">✔</span><span class="sxs-lookup"><span data-stu-id="7996b-284">✔</span></span>
<span data-ttu-id="7996b-285">Geometry.GetGeometryN(int)</span><span class="sxs-lookup"><span data-stu-id="7996b-285">Geometry.GetGeometryN(int)</span></span> | <span data-ttu-id="7996b-286">✔</span><span class="sxs-lookup"><span data-stu-id="7996b-286">✔</span></span> | | <span data-ttu-id="7996b-287">✔</span><span class="sxs-lookup"><span data-stu-id="7996b-287">✔</span></span> | <span data-ttu-id="7996b-288">✔</span><span class="sxs-lookup"><span data-stu-id="7996b-288">✔</span></span>
<span data-ttu-id="7996b-289">Geometry.InteriorPoint</span><span class="sxs-lookup"><span data-stu-id="7996b-289">Geometry.InteriorPoint</span></span> | <span data-ttu-id="7996b-290">✔</span><span class="sxs-lookup"><span data-stu-id="7996b-290">✔</span></span> | | <span data-ttu-id="7996b-291">✔</span><span class="sxs-lookup"><span data-stu-id="7996b-291">✔</span></span>
<span data-ttu-id="7996b-292">Geometry.Intersection(Geometry)</span><span class="sxs-lookup"><span data-stu-id="7996b-292">Geometry.Intersection(Geometry)</span></span> | <span data-ttu-id="7996b-293">✔</span><span class="sxs-lookup"><span data-stu-id="7996b-293">✔</span></span> | <span data-ttu-id="7996b-294">✔</span><span class="sxs-lookup"><span data-stu-id="7996b-294">✔</span></span> | <span data-ttu-id="7996b-295">✔</span><span class="sxs-lookup"><span data-stu-id="7996b-295">✔</span></span> | <span data-ttu-id="7996b-296">✔</span><span class="sxs-lookup"><span data-stu-id="7996b-296">✔</span></span>
<span data-ttu-id="7996b-297">Geometry.Intersects(Geometry)</span><span class="sxs-lookup"><span data-stu-id="7996b-297">Geometry.Intersects(Geometry)</span></span> | <span data-ttu-id="7996b-298">✔</span><span class="sxs-lookup"><span data-stu-id="7996b-298">✔</span></span> | <span data-ttu-id="7996b-299">✔</span><span class="sxs-lookup"><span data-stu-id="7996b-299">✔</span></span> | <span data-ttu-id="7996b-300">✔</span><span class="sxs-lookup"><span data-stu-id="7996b-300">✔</span></span> | <span data-ttu-id="7996b-301">✔</span><span class="sxs-lookup"><span data-stu-id="7996b-301">✔</span></span>
<span data-ttu-id="7996b-302">Geometry.IsEmpty</span><span class="sxs-lookup"><span data-stu-id="7996b-302">Geometry.IsEmpty</span></span> | <span data-ttu-id="7996b-303">✔</span><span class="sxs-lookup"><span data-stu-id="7996b-303">✔</span></span> | <span data-ttu-id="7996b-304">✔</span><span class="sxs-lookup"><span data-stu-id="7996b-304">✔</span></span> | <span data-ttu-id="7996b-305">✔</span><span class="sxs-lookup"><span data-stu-id="7996b-305">✔</span></span> | <span data-ttu-id="7996b-306">✔</span><span class="sxs-lookup"><span data-stu-id="7996b-306">✔</span></span>
<span data-ttu-id="7996b-307">Geometry.IsSimple</span><span class="sxs-lookup"><span data-stu-id="7996b-307">Geometry.IsSimple</span></span> | <span data-ttu-id="7996b-308">✔</span><span class="sxs-lookup"><span data-stu-id="7996b-308">✔</span></span> | | <span data-ttu-id="7996b-309">✔</span><span class="sxs-lookup"><span data-stu-id="7996b-309">✔</span></span> | <span data-ttu-id="7996b-310">✔</span><span class="sxs-lookup"><span data-stu-id="7996b-310">✔</span></span>
<span data-ttu-id="7996b-311">Geometry.IsValid</span><span class="sxs-lookup"><span data-stu-id="7996b-311">Geometry.IsValid</span></span> | <span data-ttu-id="7996b-312">✔</span><span class="sxs-lookup"><span data-stu-id="7996b-312">✔</span></span> | <span data-ttu-id="7996b-313">✔</span><span class="sxs-lookup"><span data-stu-id="7996b-313">✔</span></span> | <span data-ttu-id="7996b-314">✔</span><span class="sxs-lookup"><span data-stu-id="7996b-314">✔</span></span> | <span data-ttu-id="7996b-315">✔</span><span class="sxs-lookup"><span data-stu-id="7996b-315">✔</span></span>
<span data-ttu-id="7996b-316">Geometry.IsWithinDistance double (Geometry)</span><span class="sxs-lookup"><span data-stu-id="7996b-316">Geometry.IsWithinDistance(Geometry, double)</span></span> | <span data-ttu-id="7996b-317">✔</span><span class="sxs-lookup"><span data-stu-id="7996b-317">✔</span></span> | | <span data-ttu-id="7996b-318">✔</span><span class="sxs-lookup"><span data-stu-id="7996b-318">✔</span></span>
<span data-ttu-id="7996b-319">Geometry.Length</span><span class="sxs-lookup"><span data-stu-id="7996b-319">Geometry.Length</span></span> | <span data-ttu-id="7996b-320">✔</span><span class="sxs-lookup"><span data-stu-id="7996b-320">✔</span></span> | <span data-ttu-id="7996b-321">✔</span><span class="sxs-lookup"><span data-stu-id="7996b-321">✔</span></span> | <span data-ttu-id="7996b-322">✔</span><span class="sxs-lookup"><span data-stu-id="7996b-322">✔</span></span> | <span data-ttu-id="7996b-323">✔</span><span class="sxs-lookup"><span data-stu-id="7996b-323">✔</span></span>
<span data-ttu-id="7996b-324">Geometry.NumGeometries</span><span class="sxs-lookup"><span data-stu-id="7996b-324">Geometry.NumGeometries</span></span> | <span data-ttu-id="7996b-325">✔</span><span class="sxs-lookup"><span data-stu-id="7996b-325">✔</span></span> | <span data-ttu-id="7996b-326">✔</span><span class="sxs-lookup"><span data-stu-id="7996b-326">✔</span></span> | <span data-ttu-id="7996b-327">✔</span><span class="sxs-lookup"><span data-stu-id="7996b-327">✔</span></span> | <span data-ttu-id="7996b-328">✔</span><span class="sxs-lookup"><span data-stu-id="7996b-328">✔</span></span>
<span data-ttu-id="7996b-329">Geometry.NumPoints</span><span class="sxs-lookup"><span data-stu-id="7996b-329">Geometry.NumPoints</span></span> | <span data-ttu-id="7996b-330">✔</span><span class="sxs-lookup"><span data-stu-id="7996b-330">✔</span></span> | <span data-ttu-id="7996b-331">✔</span><span class="sxs-lookup"><span data-stu-id="7996b-331">✔</span></span> | <span data-ttu-id="7996b-332">✔</span><span class="sxs-lookup"><span data-stu-id="7996b-332">✔</span></span> | <span data-ttu-id="7996b-333">✔</span><span class="sxs-lookup"><span data-stu-id="7996b-333">✔</span></span>
<span data-ttu-id="7996b-334">Geometry.OgcGeometryType</span><span class="sxs-lookup"><span data-stu-id="7996b-334">Geometry.OgcGeometryType</span></span> | <span data-ttu-id="7996b-335">✔</span><span class="sxs-lookup"><span data-stu-id="7996b-335">✔</span></span> | <span data-ttu-id="7996b-336">✔</span><span class="sxs-lookup"><span data-stu-id="7996b-336">✔</span></span> | <span data-ttu-id="7996b-337">✔</span><span class="sxs-lookup"><span data-stu-id="7996b-337">✔</span></span>
<span data-ttu-id="7996b-338">Geometry.Overlaps(Geometry)</span><span class="sxs-lookup"><span data-stu-id="7996b-338">Geometry.Overlaps(Geometry)</span></span> | <span data-ttu-id="7996b-339">✔</span><span class="sxs-lookup"><span data-stu-id="7996b-339">✔</span></span> | <span data-ttu-id="7996b-340">✔</span><span class="sxs-lookup"><span data-stu-id="7996b-340">✔</span></span> | <span data-ttu-id="7996b-341">✔</span><span class="sxs-lookup"><span data-stu-id="7996b-341">✔</span></span> | <span data-ttu-id="7996b-342">✔</span><span class="sxs-lookup"><span data-stu-id="7996b-342">✔</span></span>
<span data-ttu-id="7996b-343">Geometry.PointOnSurface</span><span class="sxs-lookup"><span data-stu-id="7996b-343">Geometry.PointOnSurface</span></span> | <span data-ttu-id="7996b-344">✔</span><span class="sxs-lookup"><span data-stu-id="7996b-344">✔</span></span> | | <span data-ttu-id="7996b-345">✔</span><span class="sxs-lookup"><span data-stu-id="7996b-345">✔</span></span> | <span data-ttu-id="7996b-346">✔</span><span class="sxs-lookup"><span data-stu-id="7996b-346">✔</span></span>
<span data-ttu-id="7996b-347">Geometry.Relate (Geometry, stringa)</span><span class="sxs-lookup"><span data-stu-id="7996b-347">Geometry.Relate(Geometry, string)</span></span> | <span data-ttu-id="7996b-348">✔</span><span class="sxs-lookup"><span data-stu-id="7996b-348">✔</span></span> | | <span data-ttu-id="7996b-349">✔</span><span class="sxs-lookup"><span data-stu-id="7996b-349">✔</span></span> | <span data-ttu-id="7996b-350">✔</span><span class="sxs-lookup"><span data-stu-id="7996b-350">✔</span></span>
<span data-ttu-id="7996b-351">Geometry.Reverse()</span><span class="sxs-lookup"><span data-stu-id="7996b-351">Geometry.Reverse()</span></span> | | | <span data-ttu-id="7996b-352">✔</span><span class="sxs-lookup"><span data-stu-id="7996b-352">✔</span></span> | <span data-ttu-id="7996b-353">✔</span><span class="sxs-lookup"><span data-stu-id="7996b-353">✔</span></span>
<span data-ttu-id="7996b-354">Geometry.SRID</span><span class="sxs-lookup"><span data-stu-id="7996b-354">Geometry.SRID</span></span> | <span data-ttu-id="7996b-355">✔</span><span class="sxs-lookup"><span data-stu-id="7996b-355">✔</span></span> | <span data-ttu-id="7996b-356">✔</span><span class="sxs-lookup"><span data-stu-id="7996b-356">✔</span></span> | <span data-ttu-id="7996b-357">✔</span><span class="sxs-lookup"><span data-stu-id="7996b-357">✔</span></span> | <span data-ttu-id="7996b-358">✔</span><span class="sxs-lookup"><span data-stu-id="7996b-358">✔</span></span>
<span data-ttu-id="7996b-359">Geometry.SymmetricDifference(Geometry)</span><span class="sxs-lookup"><span data-stu-id="7996b-359">Geometry.SymmetricDifference(Geometry)</span></span> | <span data-ttu-id="7996b-360">✔</span><span class="sxs-lookup"><span data-stu-id="7996b-360">✔</span></span> | <span data-ttu-id="7996b-361">✔</span><span class="sxs-lookup"><span data-stu-id="7996b-361">✔</span></span> | <span data-ttu-id="7996b-362">✔</span><span class="sxs-lookup"><span data-stu-id="7996b-362">✔</span></span> | <span data-ttu-id="7996b-363">✔</span><span class="sxs-lookup"><span data-stu-id="7996b-363">✔</span></span>
<span data-ttu-id="7996b-364">Geometry.ToBinary()</span><span class="sxs-lookup"><span data-stu-id="7996b-364">Geometry.ToBinary()</span></span> | <span data-ttu-id="7996b-365">✔</span><span class="sxs-lookup"><span data-stu-id="7996b-365">✔</span></span> | <span data-ttu-id="7996b-366">✔</span><span class="sxs-lookup"><span data-stu-id="7996b-366">✔</span></span> | <span data-ttu-id="7996b-367">✔</span><span class="sxs-lookup"><span data-stu-id="7996b-367">✔</span></span> | <span data-ttu-id="7996b-368">✔</span><span class="sxs-lookup"><span data-stu-id="7996b-368">✔</span></span>
<span data-ttu-id="7996b-369">Geometry.ToText()</span><span class="sxs-lookup"><span data-stu-id="7996b-369">Geometry.ToText()</span></span> | <span data-ttu-id="7996b-370">✔</span><span class="sxs-lookup"><span data-stu-id="7996b-370">✔</span></span> | <span data-ttu-id="7996b-371">✔</span><span class="sxs-lookup"><span data-stu-id="7996b-371">✔</span></span> | <span data-ttu-id="7996b-372">✔</span><span class="sxs-lookup"><span data-stu-id="7996b-372">✔</span></span> | <span data-ttu-id="7996b-373">✔</span><span class="sxs-lookup"><span data-stu-id="7996b-373">✔</span></span>
<span data-ttu-id="7996b-374">Geometry.Touches(Geometry)</span><span class="sxs-lookup"><span data-stu-id="7996b-374">Geometry.Touches(Geometry)</span></span> | <span data-ttu-id="7996b-375">✔</span><span class="sxs-lookup"><span data-stu-id="7996b-375">✔</span></span> | | <span data-ttu-id="7996b-376">✔</span><span class="sxs-lookup"><span data-stu-id="7996b-376">✔</span></span> | <span data-ttu-id="7996b-377">✔</span><span class="sxs-lookup"><span data-stu-id="7996b-377">✔</span></span>
<span data-ttu-id="7996b-378">Geometry.Union()</span><span class="sxs-lookup"><span data-stu-id="7996b-378">Geometry.Union()</span></span> | | | <span data-ttu-id="7996b-379">✔</span><span class="sxs-lookup"><span data-stu-id="7996b-379">✔</span></span>
<span data-ttu-id="7996b-380">Geometry.Union(Geometry)</span><span class="sxs-lookup"><span data-stu-id="7996b-380">Geometry.Union(Geometry)</span></span> | <span data-ttu-id="7996b-381">✔</span><span class="sxs-lookup"><span data-stu-id="7996b-381">✔</span></span> | <span data-ttu-id="7996b-382">✔</span><span class="sxs-lookup"><span data-stu-id="7996b-382">✔</span></span> | <span data-ttu-id="7996b-383">✔</span><span class="sxs-lookup"><span data-stu-id="7996b-383">✔</span></span> | <span data-ttu-id="7996b-384">✔</span><span class="sxs-lookup"><span data-stu-id="7996b-384">✔</span></span>
<span data-ttu-id="7996b-385">Geometry.Within(Geometry)</span><span class="sxs-lookup"><span data-stu-id="7996b-385">Geometry.Within(Geometry)</span></span> | <span data-ttu-id="7996b-386">✔</span><span class="sxs-lookup"><span data-stu-id="7996b-386">✔</span></span> | <span data-ttu-id="7996b-387">✔</span><span class="sxs-lookup"><span data-stu-id="7996b-387">✔</span></span> | <span data-ttu-id="7996b-388">✔</span><span class="sxs-lookup"><span data-stu-id="7996b-388">✔</span></span> | <span data-ttu-id="7996b-389">✔</span><span class="sxs-lookup"><span data-stu-id="7996b-389">✔</span></span>
<span data-ttu-id="7996b-390">GeometryCollection.Count</span><span class="sxs-lookup"><span data-stu-id="7996b-390">GeometryCollection.Count</span></span> | <span data-ttu-id="7996b-391">✔</span><span class="sxs-lookup"><span data-stu-id="7996b-391">✔</span></span> | <span data-ttu-id="7996b-392">✔</span><span class="sxs-lookup"><span data-stu-id="7996b-392">✔</span></span> | <span data-ttu-id="7996b-393">✔</span><span class="sxs-lookup"><span data-stu-id="7996b-393">✔</span></span> | <span data-ttu-id="7996b-394">✔</span><span class="sxs-lookup"><span data-stu-id="7996b-394">✔</span></span>
<span data-ttu-id="7996b-395">GeometryCollection di tipo [int]</span><span class="sxs-lookup"><span data-stu-id="7996b-395">GeometryCollection[int]</span></span> | <span data-ttu-id="7996b-396">✔</span><span class="sxs-lookup"><span data-stu-id="7996b-396">✔</span></span> | <span data-ttu-id="7996b-397">✔</span><span class="sxs-lookup"><span data-stu-id="7996b-397">✔</span></span> | <span data-ttu-id="7996b-398">✔</span><span class="sxs-lookup"><span data-stu-id="7996b-398">✔</span></span> | <span data-ttu-id="7996b-399">✔</span><span class="sxs-lookup"><span data-stu-id="7996b-399">✔</span></span>
<span data-ttu-id="7996b-400">LineString.Count</span><span class="sxs-lookup"><span data-stu-id="7996b-400">LineString.Count</span></span> | <span data-ttu-id="7996b-401">✔</span><span class="sxs-lookup"><span data-stu-id="7996b-401">✔</span></span> | <span data-ttu-id="7996b-402">✔</span><span class="sxs-lookup"><span data-stu-id="7996b-402">✔</span></span> | <span data-ttu-id="7996b-403">✔</span><span class="sxs-lookup"><span data-stu-id="7996b-403">✔</span></span> | <span data-ttu-id="7996b-404">✔</span><span class="sxs-lookup"><span data-stu-id="7996b-404">✔</span></span>
<span data-ttu-id="7996b-405">LineString.EndPoint</span><span class="sxs-lookup"><span data-stu-id="7996b-405">LineString.EndPoint</span></span> | <span data-ttu-id="7996b-406">✔</span><span class="sxs-lookup"><span data-stu-id="7996b-406">✔</span></span> | <span data-ttu-id="7996b-407">✔</span><span class="sxs-lookup"><span data-stu-id="7996b-407">✔</span></span> | <span data-ttu-id="7996b-408">✔</span><span class="sxs-lookup"><span data-stu-id="7996b-408">✔</span></span> | <span data-ttu-id="7996b-409">✔</span><span class="sxs-lookup"><span data-stu-id="7996b-409">✔</span></span>
<span data-ttu-id="7996b-410">LineString.GetPointN(int)</span><span class="sxs-lookup"><span data-stu-id="7996b-410">LineString.GetPointN(int)</span></span> | <span data-ttu-id="7996b-411">✔</span><span class="sxs-lookup"><span data-stu-id="7996b-411">✔</span></span> | <span data-ttu-id="7996b-412">✔</span><span class="sxs-lookup"><span data-stu-id="7996b-412">✔</span></span> | <span data-ttu-id="7996b-413">✔</span><span class="sxs-lookup"><span data-stu-id="7996b-413">✔</span></span> | <span data-ttu-id="7996b-414">✔</span><span class="sxs-lookup"><span data-stu-id="7996b-414">✔</span></span>
<span data-ttu-id="7996b-415">LineString.IsClosed</span><span class="sxs-lookup"><span data-stu-id="7996b-415">LineString.IsClosed</span></span> | <span data-ttu-id="7996b-416">✔</span><span class="sxs-lookup"><span data-stu-id="7996b-416">✔</span></span> | <span data-ttu-id="7996b-417">✔</span><span class="sxs-lookup"><span data-stu-id="7996b-417">✔</span></span> | <span data-ttu-id="7996b-418">✔</span><span class="sxs-lookup"><span data-stu-id="7996b-418">✔</span></span> | <span data-ttu-id="7996b-419">✔</span><span class="sxs-lookup"><span data-stu-id="7996b-419">✔</span></span>
<span data-ttu-id="7996b-420">LineString.IsRing</span><span class="sxs-lookup"><span data-stu-id="7996b-420">LineString.IsRing</span></span> | <span data-ttu-id="7996b-421">✔</span><span class="sxs-lookup"><span data-stu-id="7996b-421">✔</span></span> | | <span data-ttu-id="7996b-422">✔</span><span class="sxs-lookup"><span data-stu-id="7996b-422">✔</span></span> | <span data-ttu-id="7996b-423">✔</span><span class="sxs-lookup"><span data-stu-id="7996b-423">✔</span></span>
<span data-ttu-id="7996b-424">LineString.StartPoint</span><span class="sxs-lookup"><span data-stu-id="7996b-424">LineString.StartPoint</span></span> | <span data-ttu-id="7996b-425">✔</span><span class="sxs-lookup"><span data-stu-id="7996b-425">✔</span></span> | <span data-ttu-id="7996b-426">✔</span><span class="sxs-lookup"><span data-stu-id="7996b-426">✔</span></span> | <span data-ttu-id="7996b-427">✔</span><span class="sxs-lookup"><span data-stu-id="7996b-427">✔</span></span> | <span data-ttu-id="7996b-428">✔</span><span class="sxs-lookup"><span data-stu-id="7996b-428">✔</span></span>
<span data-ttu-id="7996b-429">MultiLineString.IsClosed</span><span class="sxs-lookup"><span data-stu-id="7996b-429">MultiLineString.IsClosed</span></span> | <span data-ttu-id="7996b-430">✔</span><span class="sxs-lookup"><span data-stu-id="7996b-430">✔</span></span> | <span data-ttu-id="7996b-431">✔</span><span class="sxs-lookup"><span data-stu-id="7996b-431">✔</span></span> | <span data-ttu-id="7996b-432">✔</span><span class="sxs-lookup"><span data-stu-id="7996b-432">✔</span></span> | <span data-ttu-id="7996b-433">✔</span><span class="sxs-lookup"><span data-stu-id="7996b-433">✔</span></span>
<span data-ttu-id="7996b-434">Point.M</span><span class="sxs-lookup"><span data-stu-id="7996b-434">Point.M</span></span> | <span data-ttu-id="7996b-435">✔</span><span class="sxs-lookup"><span data-stu-id="7996b-435">✔</span></span> | <span data-ttu-id="7996b-436">✔</span><span class="sxs-lookup"><span data-stu-id="7996b-436">✔</span></span> | <span data-ttu-id="7996b-437">✔</span><span class="sxs-lookup"><span data-stu-id="7996b-437">✔</span></span> | <span data-ttu-id="7996b-438">✔</span><span class="sxs-lookup"><span data-stu-id="7996b-438">✔</span></span>
<span data-ttu-id="7996b-439">Point</span><span class="sxs-lookup"><span data-stu-id="7996b-439">Point.X</span></span> | <span data-ttu-id="7996b-440">✔</span><span class="sxs-lookup"><span data-stu-id="7996b-440">✔</span></span> | <span data-ttu-id="7996b-441">✔</span><span class="sxs-lookup"><span data-stu-id="7996b-441">✔</span></span> | <span data-ttu-id="7996b-442">✔</span><span class="sxs-lookup"><span data-stu-id="7996b-442">✔</span></span> | <span data-ttu-id="7996b-443">✔</span><span class="sxs-lookup"><span data-stu-id="7996b-443">✔</span></span>
<span data-ttu-id="7996b-444">Point</span><span class="sxs-lookup"><span data-stu-id="7996b-444">Point.Y</span></span> | <span data-ttu-id="7996b-445">✔</span><span class="sxs-lookup"><span data-stu-id="7996b-445">✔</span></span> | <span data-ttu-id="7996b-446">✔</span><span class="sxs-lookup"><span data-stu-id="7996b-446">✔</span></span> | <span data-ttu-id="7996b-447">✔</span><span class="sxs-lookup"><span data-stu-id="7996b-447">✔</span></span> | <span data-ttu-id="7996b-448">✔</span><span class="sxs-lookup"><span data-stu-id="7996b-448">✔</span></span>
<span data-ttu-id="7996b-449">Point.Z</span><span class="sxs-lookup"><span data-stu-id="7996b-449">Point.Z</span></span> | <span data-ttu-id="7996b-450">✔</span><span class="sxs-lookup"><span data-stu-id="7996b-450">✔</span></span> | <span data-ttu-id="7996b-451">✔</span><span class="sxs-lookup"><span data-stu-id="7996b-451">✔</span></span> | <span data-ttu-id="7996b-452">✔</span><span class="sxs-lookup"><span data-stu-id="7996b-452">✔</span></span> | <span data-ttu-id="7996b-453">✔</span><span class="sxs-lookup"><span data-stu-id="7996b-453">✔</span></span>
<span data-ttu-id="7996b-454">Polygon.ExteriorRing</span><span class="sxs-lookup"><span data-stu-id="7996b-454">Polygon.ExteriorRing</span></span> | <span data-ttu-id="7996b-455">✔</span><span class="sxs-lookup"><span data-stu-id="7996b-455">✔</span></span> | <span data-ttu-id="7996b-456">✔</span><span class="sxs-lookup"><span data-stu-id="7996b-456">✔</span></span> | <span data-ttu-id="7996b-457">✔</span><span class="sxs-lookup"><span data-stu-id="7996b-457">✔</span></span> | <span data-ttu-id="7996b-458">✔</span><span class="sxs-lookup"><span data-stu-id="7996b-458">✔</span></span>
<span data-ttu-id="7996b-459">Polygon.GetInteriorRingN(int)</span><span class="sxs-lookup"><span data-stu-id="7996b-459">Polygon.GetInteriorRingN(int)</span></span> | <span data-ttu-id="7996b-460">✔</span><span class="sxs-lookup"><span data-stu-id="7996b-460">✔</span></span> | <span data-ttu-id="7996b-461">✔</span><span class="sxs-lookup"><span data-stu-id="7996b-461">✔</span></span> | <span data-ttu-id="7996b-462">✔</span><span class="sxs-lookup"><span data-stu-id="7996b-462">✔</span></span> | <span data-ttu-id="7996b-463">✔</span><span class="sxs-lookup"><span data-stu-id="7996b-463">✔</span></span>
<span data-ttu-id="7996b-464">Polygon.NumInteriorRings</span><span class="sxs-lookup"><span data-stu-id="7996b-464">Polygon.NumInteriorRings</span></span> | <span data-ttu-id="7996b-465">✔</span><span class="sxs-lookup"><span data-stu-id="7996b-465">✔</span></span> | <span data-ttu-id="7996b-466">✔</span><span class="sxs-lookup"><span data-stu-id="7996b-466">✔</span></span> | <span data-ttu-id="7996b-467">✔</span><span class="sxs-lookup"><span data-stu-id="7996b-467">✔</span></span> | <span data-ttu-id="7996b-468">✔</span><span class="sxs-lookup"><span data-stu-id="7996b-468">✔</span></span>

## <a name="additional-resources"></a><span data-ttu-id="7996b-469">Risorse aggiuntive</span><span class="sxs-lookup"><span data-stu-id="7996b-469">Additional resources</span></span>

* [<span data-ttu-id="7996b-470">Dati spaziali in SQL Server</span><span class="sxs-lookup"><span data-stu-id="7996b-470">Spatial Data in SQL Server</span></span>](https://docs.microsoft.com/sql/relational-databases/spatial/spatial-data-sql-server)
* [<span data-ttu-id="7996b-471">Home page SpatiaLite</span><span class="sxs-lookup"><span data-stu-id="7996b-471">SpatiaLite Homepage</span></span>](https://www.gaia-gis.it/fossil/libspatialite)
* [<span data-ttu-id="7996b-472">Documentazione di Npgsql spaziali</span><span class="sxs-lookup"><span data-stu-id="7996b-472">Npgsql Spatial Documentation</span></span>](http://www.npgsql.org/efcore/mapping/nts.html)
* [<span data-ttu-id="7996b-473">Documentazione di PostGIS</span><span class="sxs-lookup"><span data-stu-id="7996b-473">PostGIS Documentation</span></span>](http://postgis.net/documentation/)