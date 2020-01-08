---
title: Dati spaziali-EF Core
author: bricelam
ms.author: bricelam
ms.date: 11/01/2018
ms.assetid: 2BDE29FC-4161-41A0-841E-69F51CCD9341
uid: core/modeling/spatial
ms.openlocfilehash: 8dae1ab949c77ffa08904b12a5716b729e6913a1
ms.sourcegitcommit: 32c51c22988c6f83ed4f8e50a1d01be3f4114e81
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 12/27/2019
ms.locfileid: "75502240"
---
# <a name="spatial-data"></a><span data-ttu-id="eb9ba-102">Dati spaziali</span><span class="sxs-lookup"><span data-stu-id="eb9ba-102">Spatial Data</span></span>

> [!NOTE]
> <span data-ttu-id="eb9ba-103">Questa funzionalità è stata aggiunta in EF Core 2,2.</span><span class="sxs-lookup"><span data-stu-id="eb9ba-103">This feature was added in EF Core 2.2.</span></span>

<span data-ttu-id="eb9ba-104">I dati spaziali rappresentano la posizione fisica e la forma degli oggetti.</span><span class="sxs-lookup"><span data-stu-id="eb9ba-104">Spatial data represents the physical location and the shape of objects.</span></span> <span data-ttu-id="eb9ba-105">Molti database forniscono supporto per questo tipo di dati, in modo che possa essere indicizzato e sottoposto a query insieme ad altri dati.</span><span class="sxs-lookup"><span data-stu-id="eb9ba-105">Many databases provide support for this type of data so it can be indexed and queried alongside other data.</span></span> <span data-ttu-id="eb9ba-106">Gli scenari comuni includono l'esecuzione di query per gli oggetti all'interno di una distanza specificata da una posizione o la selezione dell'oggetto il cui bordo contiene una determinata posizione.</span><span class="sxs-lookup"><span data-stu-id="eb9ba-106">Common scenarios include querying for objects within a given distance from a location, or selecting the object whose border contains a given location.</span></span> <span data-ttu-id="eb9ba-107">EF Core supporta il mapping ai tipi di dati spaziali mediante la libreria spaziale [NetTopologySuite](https://github.com/NetTopologySuite/NetTopologySuite) .</span><span class="sxs-lookup"><span data-stu-id="eb9ba-107">EF Core supports mapping to spatial data types using the [NetTopologySuite](https://github.com/NetTopologySuite/NetTopologySuite) spatial library.</span></span>

## <a name="installing"></a><span data-ttu-id="eb9ba-108">Installazione del</span><span class="sxs-lookup"><span data-stu-id="eb9ba-108">Installing</span></span>

<span data-ttu-id="eb9ba-109">Per usare i dati spaziali con EF Core, è necessario installare il pacchetto NuGet di supporto appropriato.</span><span class="sxs-lookup"><span data-stu-id="eb9ba-109">In order to use spatial data with EF Core, you need to install the appropriate supporting NuGet package.</span></span> <span data-ttu-id="eb9ba-110">Il pacchetto che è necessario installare dipende dal provider in uso.</span><span class="sxs-lookup"><span data-stu-id="eb9ba-110">Which package you need to install depends on the provider you're using.</span></span>

<span data-ttu-id="eb9ba-111">Provider EF Core</span><span class="sxs-lookup"><span data-stu-id="eb9ba-111">EF Core Provider</span></span>                        | <span data-ttu-id="eb9ba-112">Pacchetto NuGet spaziale</span><span class="sxs-lookup"><span data-stu-id="eb9ba-112">Spatial NuGet Package</span></span>
--------------------------------------- | ---------------------
<span data-ttu-id="eb9ba-113">Microsoft.EntityFrameworkCore.SqlServer</span><span class="sxs-lookup"><span data-stu-id="eb9ba-113">Microsoft.EntityFrameworkCore.SqlServer</span></span> | [<span data-ttu-id="eb9ba-114">Microsoft. EntityFrameworkCore. SqlServer. NetTopologySuite</span><span class="sxs-lookup"><span data-stu-id="eb9ba-114">Microsoft.EntityFrameworkCore.SqlServer.NetTopologySuite</span></span>](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.SqlServer.NetTopologySuite)
<span data-ttu-id="eb9ba-115">Microsoft.EntityFrameworkCore.Sqlite</span><span class="sxs-lookup"><span data-stu-id="eb9ba-115">Microsoft.EntityFrameworkCore.Sqlite</span></span>    | [<span data-ttu-id="eb9ba-116">Microsoft. EntityFrameworkCore. sqlite. NetTopologySuite</span><span class="sxs-lookup"><span data-stu-id="eb9ba-116">Microsoft.EntityFrameworkCore.Sqlite.NetTopologySuite</span></span>](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Sqlite.NetTopologySuite)
<span data-ttu-id="eb9ba-117">Microsoft.EntityFrameworkCore.InMemory</span><span class="sxs-lookup"><span data-stu-id="eb9ba-117">Microsoft.EntityFrameworkCore.InMemory</span></span>  | [<span data-ttu-id="eb9ba-118">NetTopologySuite</span><span class="sxs-lookup"><span data-stu-id="eb9ba-118">NetTopologySuite</span></span>](https://www.nuget.org/packages/NetTopologySuite)
<span data-ttu-id="eb9ba-119">Npgsql.EntityFrameworkCore.PostgreSQL</span><span class="sxs-lookup"><span data-stu-id="eb9ba-119">Npgsql.EntityFrameworkCore.PostgreSQL</span></span>   | [<span data-ttu-id="eb9ba-120">Npgsql. EntityFrameworkCore. PostgreSQL. NetTopologySuite</span><span class="sxs-lookup"><span data-stu-id="eb9ba-120">Npgsql.EntityFrameworkCore.PostgreSQL.NetTopologySuite</span></span>](https://www.nuget.org/packages/Npgsql.EntityFrameworkCore.PostgreSQL.NetTopologySuite)

## <a name="reverse-engineering"></a><span data-ttu-id="eb9ba-121">Reverse Engineering</span><span class="sxs-lookup"><span data-stu-id="eb9ba-121">Reverse engineering</span></span>

<span data-ttu-id="eb9ba-122">I pacchetti NuGet spaziali abilitano anche [Reverse Engineering](../managing-schemas/scaffolding.md) modelli con proprietà spaziali, ma è necessario installare il pacchetto ***prima*** di eseguire `Scaffold-DbContext` o `dotnet ef dbcontext scaffold`.</span><span class="sxs-lookup"><span data-stu-id="eb9ba-122">The spatial NuGet packages also enable [reverse engineering](../managing-schemas/scaffolding.md) models with spatial properties, but you need to install the package ***before*** running `Scaffold-DbContext` or `dotnet ef dbcontext scaffold`.</span></span> <span data-ttu-id="eb9ba-123">In caso contrario, si riceveranno avvisi relativi alla mancata individuazione dei mapping dei tipi per le colonne e le colonne verranno ignorate.</span><span class="sxs-lookup"><span data-stu-id="eb9ba-123">If you don't, you'll receive warnings about not finding type mappings for the columns and the columns will be skipped.</span></span>

## <a name="nettopologysuite-nts"></a><span data-ttu-id="eb9ba-124">NetTopologySuite (NTS)</span><span class="sxs-lookup"><span data-stu-id="eb9ba-124">NetTopologySuite (NTS)</span></span>

<span data-ttu-id="eb9ba-125">NetTopologySuite è una libreria spaziale per .NET.</span><span class="sxs-lookup"><span data-stu-id="eb9ba-125">NetTopologySuite is a spatial library for .NET.</span></span> <span data-ttu-id="eb9ba-126">EF Core Abilita il mapping ai tipi di dati spaziali nel database usando i tipi NTS nel modello.</span><span class="sxs-lookup"><span data-stu-id="eb9ba-126">EF Core enables mapping to spatial data types in the database by using NTS types in your model.</span></span>

<span data-ttu-id="eb9ba-127">Per abilitare il mapping ai tipi spaziali tramite NTS, chiamare il metodo UseNetTopologySuite nel generatore di opzioni DbContext del provider.</span><span class="sxs-lookup"><span data-stu-id="eb9ba-127">To enable mapping to spatial types via NTS, call the UseNetTopologySuite method on the provider's DbContext options builder.</span></span> <span data-ttu-id="eb9ba-128">Ad esempio, con SQL Server è possibile chiamarlo come questo.</span><span class="sxs-lookup"><span data-stu-id="eb9ba-128">For example, with SQL Server you'd call it like this.</span></span>

``` csharp
optionsBuilder.UseSqlServer(
    @"Data Source=(localdb)\MSSQLLocalDB;Initial Catalog=WideWorldImporters",
    x => x.UseNetTopologySuite());
```

<span data-ttu-id="eb9ba-129">Esistono diversi tipi di dati spaziali.</span><span class="sxs-lookup"><span data-stu-id="eb9ba-129">There are several spatial data types.</span></span> <span data-ttu-id="eb9ba-130">Il tipo da utilizzare dipende dai tipi di forme che si desidera consentire.</span><span class="sxs-lookup"><span data-stu-id="eb9ba-130">Which type you use depends on the types of shapes you want to allow.</span></span> <span data-ttu-id="eb9ba-131">Di seguito è illustrata la gerarchia di tipi NTS che è possibile usare per le proprietà nel modello.</span><span class="sxs-lookup"><span data-stu-id="eb9ba-131">Here is the hierarchy of NTS types that you can use for properties in your model.</span></span> <span data-ttu-id="eb9ba-132">Si trovano nello spazio dei nomi `NetTopologySuite.Geometries`.</span><span class="sxs-lookup"><span data-stu-id="eb9ba-132">They're located within the `NetTopologySuite.Geometries` namespace.</span></span>

* <span data-ttu-id="eb9ba-133">Geometria</span><span class="sxs-lookup"><span data-stu-id="eb9ba-133">Geometry</span></span>
  * <span data-ttu-id="eb9ba-134">Punto</span><span class="sxs-lookup"><span data-stu-id="eb9ba-134">Point</span></span>
  * <span data-ttu-id="eb9ba-135">LineString</span><span class="sxs-lookup"><span data-stu-id="eb9ba-135">LineString</span></span>
  * <span data-ttu-id="eb9ba-136">Poligono</span><span class="sxs-lookup"><span data-stu-id="eb9ba-136">Polygon</span></span>
  * <span data-ttu-id="eb9ba-137">GeometryCollection</span><span class="sxs-lookup"><span data-stu-id="eb9ba-137">GeometryCollection</span></span>
    * <span data-ttu-id="eb9ba-138">MultiPoint</span><span class="sxs-lookup"><span data-stu-id="eb9ba-138">MultiPoint</span></span>
    * <span data-ttu-id="eb9ba-139">MultiLineString</span><span class="sxs-lookup"><span data-stu-id="eb9ba-139">MultiLineString</span></span>
    * <span data-ttu-id="eb9ba-140">MultiPolygon</span><span class="sxs-lookup"><span data-stu-id="eb9ba-140">MultiPolygon</span></span>

> [!WARNING]
> <span data-ttu-id="eb9ba-141">CircularString, CompoundCurve e CurePolygon non sono supportati da NTS.</span><span class="sxs-lookup"><span data-stu-id="eb9ba-141">CircularString, CompoundCurve, and CurePolygon aren't supported by NTS.</span></span>

<span data-ttu-id="eb9ba-142">L'utilizzo del tipo Geometry di base consente di specificare qualsiasi tipo di forma dalla proprietà.</span><span class="sxs-lookup"><span data-stu-id="eb9ba-142">Using the base Geometry type allows any type of shape to be specified by the property.</span></span>

<span data-ttu-id="eb9ba-143">Per eseguire il mapping alle tabelle nel [database di esempio Wide World Importers](https://go.microsoft.com/fwlink/?LinkID=800630), è possibile usare le classi di entità seguenti.</span><span class="sxs-lookup"><span data-stu-id="eb9ba-143">The following entity classes could be used to map to tables in the [Wide World Importers sample database](https://go.microsoft.com/fwlink/?LinkID=800630).</span></span>

``` csharp
[Table("Cities", Schema = "Application"))]
class City
{
    public int CityID { get; set; }

    public string CityName { get; set; }

    public Point Location { get; set; }
}

[Table("Countries", Schema = "Application"))]
class Country
{
    public int CountryID { get; set; }

    public string CountryName { get; set; }

    // Database includes both Polygon and MultiPolygon values
    public Geometry Border { get; set; }
}
```

### <a name="creating-values"></a><span data-ttu-id="eb9ba-144">Creazione di valori</span><span class="sxs-lookup"><span data-stu-id="eb9ba-144">Creating values</span></span>

<span data-ttu-id="eb9ba-145">È possibile utilizzare i costruttori per creare oggetti Geometry; Tuttavia, NTS consiglia di usare invece una factory Geometry.</span><span class="sxs-lookup"><span data-stu-id="eb9ba-145">You can use constructors to create geometry objects; however, NTS recommends using a geometry factory instead.</span></span> <span data-ttu-id="eb9ba-146">In questo modo è possibile specificare un SRID predefinito (il sistema di riferimento spaziale usato dalle coordinate) e il controllo su elementi più avanzati, come il modello di precisione (usato durante i calcoli) e la sequenza di coordinate (determina le ordinate-dimensioni sono disponibili le misure e.</span><span class="sxs-lookup"><span data-stu-id="eb9ba-146">This lets you specify a default SRID (the spatial reference system used by the coordinates) and gives you control over more advanced things like the precision model (used during calculations) and the coordinate sequence (determines which ordinates--dimensions and measures--are available).</span></span>

``` csharp
var geometryFactory = NtsGeometryServices.Instance.CreateGeometryFactory(srid: 4326);
var currentLocation = geometryFactory.CreatePoint(-122.121512, 47.6739882);
```

> [!NOTE]
> <span data-ttu-id="eb9ba-147">4326 si riferisce a WGS 84, uno standard usato in GPS e in altri sistemi geografici.</span><span class="sxs-lookup"><span data-stu-id="eb9ba-147">4326 refers to WGS 84, a standard used in GPS and other geographic systems.</span></span>

### <a name="longitude-and-latitude"></a><span data-ttu-id="eb9ba-148">Longitudine e latitudine</span><span class="sxs-lookup"><span data-stu-id="eb9ba-148">Longitude and Latitude</span></span>

<span data-ttu-id="eb9ba-149">Le coordinate in NTS sono espresse in termini di valori X e Y.</span><span class="sxs-lookup"><span data-stu-id="eb9ba-149">Coordinates in NTS are in terms of X and Y values.</span></span> <span data-ttu-id="eb9ba-150">Per rappresentare la longitudine e la latitudine, usare X per longitudine e Y per la latitudine.</span><span class="sxs-lookup"><span data-stu-id="eb9ba-150">To represent longitude and latitude, use X for longitude and Y for latitude.</span></span> <span data-ttu-id="eb9ba-151">Si noti che questa operazione è all' **indietro** rispetto al formato `latitude, longitude` in cui vengono in genere visualizzati questi valori.</span><span class="sxs-lookup"><span data-stu-id="eb9ba-151">Note that this is **backwards** from the `latitude, longitude` format in which you typically see these values.</span></span>

### <a name="srid-ignored-during-client-operations"></a><span data-ttu-id="eb9ba-152">SRID ignorato durante le operazioni client</span><span class="sxs-lookup"><span data-stu-id="eb9ba-152">SRID Ignored during client operations</span></span>

<span data-ttu-id="eb9ba-153">NTS ignora i valori SRID durante le operazioni.</span><span class="sxs-lookup"><span data-stu-id="eb9ba-153">NTS ignores SRID values during operations.</span></span> <span data-ttu-id="eb9ba-154">Si presuppone un sistema di coordinate planare.</span><span class="sxs-lookup"><span data-stu-id="eb9ba-154">It assumes a planar coordinate system.</span></span> <span data-ttu-id="eb9ba-155">Ciò significa che se si specificano le coordinate in termini di longitudine e latitudine, alcuni valori valutati dal client, ad esempio distanza, lunghezza e area, saranno in gradi, non in metri.</span><span class="sxs-lookup"><span data-stu-id="eb9ba-155">This means that if you specify coordinates in terms of longitude and latitude, some client-evaluated values like distance, length, and area will be in degrees, not meters.</span></span> <span data-ttu-id="eb9ba-156">Per valori più significativi, è necessario innanzitutto proiettare le coordinate in un altro sistema di coordinate usando una libreria come [ProjNet4GeoAPI](https://github.com/NetTopologySuite/ProjNet4GeoAPI) prima di calcolare questi valori.</span><span class="sxs-lookup"><span data-stu-id="eb9ba-156">For more meaningful values, you first need to project the coordinates to another coordinate system using a library like [ProjNet4GeoAPI](https://github.com/NetTopologySuite/ProjNet4GeoAPI) before calculating these values.</span></span>

<span data-ttu-id="eb9ba-157">Se un'operazione viene valutata dal server EF Core tramite SQL, l'unità risultante sarà determinata dal database.</span><span class="sxs-lookup"><span data-stu-id="eb9ba-157">If an operation is server-evaluated by EF Core via SQL, the result's unit will be determined by the database.</span></span>

<span data-ttu-id="eb9ba-158">Di seguito è riportato un esempio di utilizzo di ProjNet4GeoAPI per calcolare la distanza tra due città.</span><span class="sxs-lookup"><span data-stu-id="eb9ba-158">Here is an example of using ProjNet4GeoAPI to calculate the distance between two cities.</span></span>

``` csharp
static class GeometryExtensions
{
    static readonly CoordinateSystemServices _coordinateSystemServices
        = new CoordinateSystemServices(
            new CoordinateSystemFactory(),
            new CoordinateTransformationFactory(),
            new Dictionary<int, string>
            {
                // Coordinate systems:

                [4326] = GeographicCoordinateSystem.WGS84.WKT,

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

    public static Geometry ProjectTo(this Geometry geometry, int srid)
    {
        var transformation = _coordinateSystemServices.CreateTransformation(geometry.SRID, srid);

        var result = geometry.Copy();
        result.Apply(new MathTransformFilter(transformation.MathTransform));

        return result;
    }

    class MathTransformFilter : ICoordinateSequenceFilter
    {
        readonly MathTransform _transform;

        public MathTransformFilter(MathTransform transform)
            => _transform = transform;

        public bool Done => false;
        public bool GeometryChanged => true;

        public void Filter(CoordinateSequence seq, int i)
        {
            var result = _transform.Transform(
                new[]
                {
                    seq.GetOrdinate(i, Ordinate.X),
                    seq.GetOrdinate(i, Ordinate.Y)
                });
            seq.SetOrdinate(i, Ordinate.X, result[0]);
            seq.SetOrdinate(i, Ordinate.Y, result[1]);
        }
    }
}
```

``` csharp
var seattle = new Point(-122.333056, 47.609722) { SRID = 4326 };
var redmond = new Point(-122.123889, 47.669444) { SRID = 4326 };

var distance = seattle.ProjectTo(2855).Distance(redmond.ProjectTo(2855));
```

## <a name="querying-data"></a><span data-ttu-id="eb9ba-159">Esecuzione di query su dati</span><span class="sxs-lookup"><span data-stu-id="eb9ba-159">Querying Data</span></span>

<span data-ttu-id="eb9ba-160">In LINQ, i metodi e le proprietà NTS disponibili come funzioni di database verranno convertiti in SQL.</span><span class="sxs-lookup"><span data-stu-id="eb9ba-160">In LINQ, the NTS methods and properties available as database functions will be translated to SQL.</span></span> <span data-ttu-id="eb9ba-161">Ad esempio, i metodi Distance e Contains vengono convertiti nelle query seguenti.</span><span class="sxs-lookup"><span data-stu-id="eb9ba-161">For example, the Distance and Contains methods are translated in the following queries.</span></span> <span data-ttu-id="eb9ba-162">La tabella alla fine di questo articolo illustra i membri supportati da vari provider di EF Core.</span><span class="sxs-lookup"><span data-stu-id="eb9ba-162">The table at the end of this article shows which members are supported by various EF Core providers.</span></span>

``` csharp
var nearestCity = db.Cities
    .OrderBy(c => c.Location.Distance(currentLocation))
    .FirstOrDefault();

var currentCountry = db.Countries
    .FirstOrDefault(c => c.Border.Contains(currentLocation));
```

## <a name="sql-server"></a><span data-ttu-id="eb9ba-163">SQL Server</span><span class="sxs-lookup"><span data-stu-id="eb9ba-163">SQL Server</span></span>

<span data-ttu-id="eb9ba-164">Se si usa SQL Server, è necessario tenere presenti alcuni aspetti aggiuntivi.</span><span class="sxs-lookup"><span data-stu-id="eb9ba-164">If you're using SQL Server, there are some additional things you should be aware of.</span></span>

### <a name="geography-or-geometry"></a><span data-ttu-id="eb9ba-165">Geografia o geometria</span><span class="sxs-lookup"><span data-stu-id="eb9ba-165">Geography or geometry</span></span>

<span data-ttu-id="eb9ba-166">Per impostazione predefinita, viene eseguito il mapping delle proprietà spaziali a `geography` colonne SQL Server.</span><span class="sxs-lookup"><span data-stu-id="eb9ba-166">By default, spatial properties are mapped to `geography` columns in SQL Server.</span></span> <span data-ttu-id="eb9ba-167">Per utilizzare `geometry`, [configurare il tipo di colonna](xref:core/modeling/entity-properties#column-data-types) nel modello.</span><span class="sxs-lookup"><span data-stu-id="eb9ba-167">To use `geometry`, [configure the column type](xref:core/modeling/entity-properties#column-data-types) in your model.</span></span>

### <a name="geography-polygon-rings"></a><span data-ttu-id="eb9ba-168">Anelli del poligono geografico</span><span class="sxs-lookup"><span data-stu-id="eb9ba-168">Geography polygon rings</span></span>

<span data-ttu-id="eb9ba-169">Quando si usa il tipo di colonna `geography`, SQL Server impone requisiti aggiuntivi sull'anello esterno (o sulla Shell) e sugli anelli interni (o buchi).</span><span class="sxs-lookup"><span data-stu-id="eb9ba-169">When using the `geography` column type, SQL Server imposes additional requirements on the exterior ring (or shell) and interior rings (or holes).</span></span> <span data-ttu-id="eb9ba-170">L'anello esterno deve essere orientato in senso antiorario e gli anelli interni in senso orario.</span><span class="sxs-lookup"><span data-stu-id="eb9ba-170">The exterior ring must be oriented counterclockwise and the interior rings clockwise.</span></span> <span data-ttu-id="eb9ba-171">NTS convalida questa operazione prima di inviare valori al database.</span><span class="sxs-lookup"><span data-stu-id="eb9ba-171">NTS validates this before sending values to the database.</span></span>

### <a name="fullglobe"></a><span data-ttu-id="eb9ba-172">FullGlobe</span><span class="sxs-lookup"><span data-stu-id="eb9ba-172">FullGlobe</span></span>

<span data-ttu-id="eb9ba-173">SQL Server dispone di un tipo geometry non standard per rappresentare l'intero globo quando si usa il tipo di colonna `geography`.</span><span class="sxs-lookup"><span data-stu-id="eb9ba-173">SQL Server has a non-standard geometry type to represent the full globe when using the `geography` column type.</span></span> <span data-ttu-id="eb9ba-174">Offre anche un modo per rappresentare i poligoni in base al globo completo (senza anello esterno).</span><span class="sxs-lookup"><span data-stu-id="eb9ba-174">It also has a way to represent polygons based on the full globe (without an exterior ring).</span></span> <span data-ttu-id="eb9ba-175">Nessuno di questi è supportato da NTS.</span><span class="sxs-lookup"><span data-stu-id="eb9ba-175">Neither of these are supported by NTS.</span></span>

> [!WARNING]
> <span data-ttu-id="eb9ba-176">FullGlobe e i poligoni basati su di essi non sono supportati da NTS.</span><span class="sxs-lookup"><span data-stu-id="eb9ba-176">FullGlobe and polygons based on it aren't supported by NTS.</span></span>

## <a name="sqlite"></a><span data-ttu-id="eb9ba-177">SQLite</span><span class="sxs-lookup"><span data-stu-id="eb9ba-177">SQLite</span></span>

<span data-ttu-id="eb9ba-178">Di seguito sono riportate alcune informazioni aggiuntive per quelle che usano SQLite.</span><span class="sxs-lookup"><span data-stu-id="eb9ba-178">Here is some additional information for those using SQLite.</span></span>

### <a name="installing-spatialite"></a><span data-ttu-id="eb9ba-179">Installazione di SpatiaLite</span><span class="sxs-lookup"><span data-stu-id="eb9ba-179">Installing SpatiaLite</span></span>

<span data-ttu-id="eb9ba-180">In Windows la libreria mod_spatialite nativa viene distribuita come dipendenza del pacchetto NuGet.</span><span class="sxs-lookup"><span data-stu-id="eb9ba-180">On Windows, the native mod_spatialite library is distributed as a NuGet package dependency.</span></span> <span data-ttu-id="eb9ba-181">Altre piattaforme devono essere installate separatamente.</span><span class="sxs-lookup"><span data-stu-id="eb9ba-181">Other platforms need to install it separately.</span></span> <span data-ttu-id="eb9ba-182">Questa operazione viene in genere eseguita utilizzando una gestione pacchetti software.</span><span class="sxs-lookup"><span data-stu-id="eb9ba-182">This is typically done using a software package manager.</span></span> <span data-ttu-id="eb9ba-183">Ad esempio, è possibile usare APT in Ubuntu e homebrew in MacOS.</span><span class="sxs-lookup"><span data-stu-id="eb9ba-183">For example, you can use APT on Ubuntu and Homebrew on MacOS.</span></span>

``` sh
# Ubuntu
apt-get install libsqlite3-mod-spatialite

# macOS
brew install libspatialite
```

### <a name="configuring-srid"></a><span data-ttu-id="eb9ba-184">Configurazione di SRID</span><span class="sxs-lookup"><span data-stu-id="eb9ba-184">Configuring SRID</span></span>

<span data-ttu-id="eb9ba-185">In SpatiaLite le colonne devono specificare un identificatore SRID per colonna.</span><span class="sxs-lookup"><span data-stu-id="eb9ba-185">In SpatiaLite, columns need to specify an SRID per column.</span></span> <span data-ttu-id="eb9ba-186">L'identificatore SRID predefinito è `0`.</span><span class="sxs-lookup"><span data-stu-id="eb9ba-186">The default SRID is `0`.</span></span> <span data-ttu-id="eb9ba-187">Specificare un identificatore SRID diverso usando il metodo ForSqliteHasSrid.</span><span class="sxs-lookup"><span data-stu-id="eb9ba-187">Specify a different SRID using the ForSqliteHasSrid method.</span></span>

``` csharp
modelBuilder.Entity<City>().Property(c => c.Location)
    .ForSqliteHasSrid(4326);
```

### <a name="dimension"></a><span data-ttu-id="eb9ba-188">Dimensione</span><span class="sxs-lookup"><span data-stu-id="eb9ba-188">Dimension</span></span>

<span data-ttu-id="eb9ba-189">Analogamente a SRID, viene specificata anche una dimensione (o ordinata) di una colonna come parte della colonna.</span><span class="sxs-lookup"><span data-stu-id="eb9ba-189">Similar to SRID, a column's dimension (or ordinates) is also specified as part of the column.</span></span> <span data-ttu-id="eb9ba-190">Le ordinate predefinite sono X e Y. abilitare le coordinate aggiuntive (Z e M) usando il metodo ForSqliteHasDimension.</span><span class="sxs-lookup"><span data-stu-id="eb9ba-190">The default ordinates are X and Y. Enable additional ordinates (Z and M) using the ForSqliteHasDimension method.</span></span>

``` csharp
modelBuilder.Entity<City>().Property(c => c.Location)
    .ForSqliteHasDimension(Ordinates.XYZ);
```

## <a name="translated-operations"></a><span data-ttu-id="eb9ba-191">Operazioni convertite</span><span class="sxs-lookup"><span data-stu-id="eb9ba-191">Translated Operations</span></span>

<span data-ttu-id="eb9ba-192">Questa tabella mostra i membri NTS convertiti in SQL da ogni provider di EF Core.</span><span class="sxs-lookup"><span data-stu-id="eb9ba-192">This table shows which NTS members are translated into SQL by each EF Core provider.</span></span>

<span data-ttu-id="eb9ba-193">NetTopologySuite</span><span class="sxs-lookup"><span data-stu-id="eb9ba-193">NetTopologySuite</span></span> | <span data-ttu-id="eb9ba-194">SQL Server (geometria)</span><span class="sxs-lookup"><span data-stu-id="eb9ba-194">SQL Server (geometry)</span></span> | <span data-ttu-id="eb9ba-195">SQL Server (geography)</span><span class="sxs-lookup"><span data-stu-id="eb9ba-195">SQL Server (geography)</span></span> | <span data-ttu-id="eb9ba-196">SQLite</span><span class="sxs-lookup"><span data-stu-id="eb9ba-196">SQLite</span></span> | <span data-ttu-id="eb9ba-197">Npgsql</span><span class="sxs-lookup"><span data-stu-id="eb9ba-197">Npgsql</span></span>
--- |:---:|:---:|:---:|:---:
<span data-ttu-id="eb9ba-198">Geometry. area</span><span class="sxs-lookup"><span data-stu-id="eb9ba-198">Geometry.Area</span></span> | <span data-ttu-id="eb9ba-199">✔</span><span class="sxs-lookup"><span data-stu-id="eb9ba-199">✔</span></span> | <span data-ttu-id="eb9ba-200">✔</span><span class="sxs-lookup"><span data-stu-id="eb9ba-200">✔</span></span> | <span data-ttu-id="eb9ba-201">✔</span><span class="sxs-lookup"><span data-stu-id="eb9ba-201">✔</span></span> | <span data-ttu-id="eb9ba-202">✔</span><span class="sxs-lookup"><span data-stu-id="eb9ba-202">✔</span></span>
<span data-ttu-id="eb9ba-203">Geometry. AsBinary ()</span><span class="sxs-lookup"><span data-stu-id="eb9ba-203">Geometry.AsBinary()</span></span> | <span data-ttu-id="eb9ba-204">✔</span><span class="sxs-lookup"><span data-stu-id="eb9ba-204">✔</span></span> | <span data-ttu-id="eb9ba-205">✔</span><span class="sxs-lookup"><span data-stu-id="eb9ba-205">✔</span></span> | <span data-ttu-id="eb9ba-206">✔</span><span class="sxs-lookup"><span data-stu-id="eb9ba-206">✔</span></span> | <span data-ttu-id="eb9ba-207">✔</span><span class="sxs-lookup"><span data-stu-id="eb9ba-207">✔</span></span>
<span data-ttu-id="eb9ba-208">Geometry. AsText ()</span><span class="sxs-lookup"><span data-stu-id="eb9ba-208">Geometry.AsText()</span></span> | <span data-ttu-id="eb9ba-209">✔</span><span class="sxs-lookup"><span data-stu-id="eb9ba-209">✔</span></span> | <span data-ttu-id="eb9ba-210">✔</span><span class="sxs-lookup"><span data-stu-id="eb9ba-210">✔</span></span> | <span data-ttu-id="eb9ba-211">✔</span><span class="sxs-lookup"><span data-stu-id="eb9ba-211">✔</span></span> | <span data-ttu-id="eb9ba-212">✔</span><span class="sxs-lookup"><span data-stu-id="eb9ba-212">✔</span></span>
<span data-ttu-id="eb9ba-213">Geometry. Boundary</span><span class="sxs-lookup"><span data-stu-id="eb9ba-213">Geometry.Boundary</span></span> | <span data-ttu-id="eb9ba-214">✔</span><span class="sxs-lookup"><span data-stu-id="eb9ba-214">✔</span></span> | | <span data-ttu-id="eb9ba-215">✔</span><span class="sxs-lookup"><span data-stu-id="eb9ba-215">✔</span></span> | <span data-ttu-id="eb9ba-216">✔</span><span class="sxs-lookup"><span data-stu-id="eb9ba-216">✔</span></span>
<span data-ttu-id="eb9ba-217">Geometry. buffer (Double)</span><span class="sxs-lookup"><span data-stu-id="eb9ba-217">Geometry.Buffer(double)</span></span> | <span data-ttu-id="eb9ba-218">✔</span><span class="sxs-lookup"><span data-stu-id="eb9ba-218">✔</span></span> | <span data-ttu-id="eb9ba-219">✔</span><span class="sxs-lookup"><span data-stu-id="eb9ba-219">✔</span></span> | <span data-ttu-id="eb9ba-220">✔</span><span class="sxs-lookup"><span data-stu-id="eb9ba-220">✔</span></span> | <span data-ttu-id="eb9ba-221">✔</span><span class="sxs-lookup"><span data-stu-id="eb9ba-221">✔</span></span>
<span data-ttu-id="eb9ba-222">Geometry. buffer (Double, int)</span><span class="sxs-lookup"><span data-stu-id="eb9ba-222">Geometry.Buffer(double, int)</span></span> | | | <span data-ttu-id="eb9ba-223">✔</span><span class="sxs-lookup"><span data-stu-id="eb9ba-223">✔</span></span> | <span data-ttu-id="eb9ba-224">✔</span><span class="sxs-lookup"><span data-stu-id="eb9ba-224">✔</span></span>
<span data-ttu-id="eb9ba-225">Geometry. baricentro</span><span class="sxs-lookup"><span data-stu-id="eb9ba-225">Geometry.Centroid</span></span> | <span data-ttu-id="eb9ba-226">✔</span><span class="sxs-lookup"><span data-stu-id="eb9ba-226">✔</span></span> | | <span data-ttu-id="eb9ba-227">✔</span><span class="sxs-lookup"><span data-stu-id="eb9ba-227">✔</span></span> | <span data-ttu-id="eb9ba-228">✔</span><span class="sxs-lookup"><span data-stu-id="eb9ba-228">✔</span></span>
<span data-ttu-id="eb9ba-229">Geometry. Contains (Geometry)</span><span class="sxs-lookup"><span data-stu-id="eb9ba-229">Geometry.Contains(Geometry)</span></span> | <span data-ttu-id="eb9ba-230">✔</span><span class="sxs-lookup"><span data-stu-id="eb9ba-230">✔</span></span> | <span data-ttu-id="eb9ba-231">✔</span><span class="sxs-lookup"><span data-stu-id="eb9ba-231">✔</span></span> | <span data-ttu-id="eb9ba-232">✔</span><span class="sxs-lookup"><span data-stu-id="eb9ba-232">✔</span></span> | <span data-ttu-id="eb9ba-233">✔</span><span class="sxs-lookup"><span data-stu-id="eb9ba-233">✔</span></span>
<span data-ttu-id="eb9ba-234">Geometry. ConvexHull ()</span><span class="sxs-lookup"><span data-stu-id="eb9ba-234">Geometry.ConvexHull()</span></span> | <span data-ttu-id="eb9ba-235">✔</span><span class="sxs-lookup"><span data-stu-id="eb9ba-235">✔</span></span> | <span data-ttu-id="eb9ba-236">✔</span><span class="sxs-lookup"><span data-stu-id="eb9ba-236">✔</span></span> | <span data-ttu-id="eb9ba-237">✔</span><span class="sxs-lookup"><span data-stu-id="eb9ba-237">✔</span></span> | <span data-ttu-id="eb9ba-238">✔</span><span class="sxs-lookup"><span data-stu-id="eb9ba-238">✔</span></span>
<span data-ttu-id="eb9ba-239">Geometry. CoveredBy (Geometry)</span><span class="sxs-lookup"><span data-stu-id="eb9ba-239">Geometry.CoveredBy(Geometry)</span></span> | | | <span data-ttu-id="eb9ba-240">✔</span><span class="sxs-lookup"><span data-stu-id="eb9ba-240">✔</span></span> | <span data-ttu-id="eb9ba-241">✔</span><span class="sxs-lookup"><span data-stu-id="eb9ba-241">✔</span></span>
<span data-ttu-id="eb9ba-242">Geometry. Covers (geometria)</span><span class="sxs-lookup"><span data-stu-id="eb9ba-242">Geometry.Covers(Geometry)</span></span> | | | <span data-ttu-id="eb9ba-243">✔</span><span class="sxs-lookup"><span data-stu-id="eb9ba-243">✔</span></span> | <span data-ttu-id="eb9ba-244">✔</span><span class="sxs-lookup"><span data-stu-id="eb9ba-244">✔</span></span>
<span data-ttu-id="eb9ba-245">Geometry. Crosses (geometria)</span><span class="sxs-lookup"><span data-stu-id="eb9ba-245">Geometry.Crosses(Geometry)</span></span> | <span data-ttu-id="eb9ba-246">✔</span><span class="sxs-lookup"><span data-stu-id="eb9ba-246">✔</span></span> | | <span data-ttu-id="eb9ba-247">✔</span><span class="sxs-lookup"><span data-stu-id="eb9ba-247">✔</span></span> | <span data-ttu-id="eb9ba-248">✔</span><span class="sxs-lookup"><span data-stu-id="eb9ba-248">✔</span></span>
<span data-ttu-id="eb9ba-249">Geometry. Difference (Geometry)</span><span class="sxs-lookup"><span data-stu-id="eb9ba-249">Geometry.Difference(Geometry)</span></span> | <span data-ttu-id="eb9ba-250">✔</span><span class="sxs-lookup"><span data-stu-id="eb9ba-250">✔</span></span> | <span data-ttu-id="eb9ba-251">✔</span><span class="sxs-lookup"><span data-stu-id="eb9ba-251">✔</span></span> | <span data-ttu-id="eb9ba-252">✔</span><span class="sxs-lookup"><span data-stu-id="eb9ba-252">✔</span></span> | <span data-ttu-id="eb9ba-253">✔</span><span class="sxs-lookup"><span data-stu-id="eb9ba-253">✔</span></span>
<span data-ttu-id="eb9ba-254">Geometry. Dimension</span><span class="sxs-lookup"><span data-stu-id="eb9ba-254">Geometry.Dimension</span></span> | <span data-ttu-id="eb9ba-255">✔</span><span class="sxs-lookup"><span data-stu-id="eb9ba-255">✔</span></span> | <span data-ttu-id="eb9ba-256">✔</span><span class="sxs-lookup"><span data-stu-id="eb9ba-256">✔</span></span> | <span data-ttu-id="eb9ba-257">✔</span><span class="sxs-lookup"><span data-stu-id="eb9ba-257">✔</span></span> | <span data-ttu-id="eb9ba-258">✔</span><span class="sxs-lookup"><span data-stu-id="eb9ba-258">✔</span></span>
<span data-ttu-id="eb9ba-259">Geometry. unjoint (Geometry)</span><span class="sxs-lookup"><span data-stu-id="eb9ba-259">Geometry.Disjoint(Geometry)</span></span> | <span data-ttu-id="eb9ba-260">✔</span><span class="sxs-lookup"><span data-stu-id="eb9ba-260">✔</span></span> | <span data-ttu-id="eb9ba-261">✔</span><span class="sxs-lookup"><span data-stu-id="eb9ba-261">✔</span></span> | <span data-ttu-id="eb9ba-262">✔</span><span class="sxs-lookup"><span data-stu-id="eb9ba-262">✔</span></span> | <span data-ttu-id="eb9ba-263">✔</span><span class="sxs-lookup"><span data-stu-id="eb9ba-263">✔</span></span>
<span data-ttu-id="eb9ba-264">Geometry. distance (Geometry)</span><span class="sxs-lookup"><span data-stu-id="eb9ba-264">Geometry.Distance(Geometry)</span></span> | <span data-ttu-id="eb9ba-265">✔</span><span class="sxs-lookup"><span data-stu-id="eb9ba-265">✔</span></span> | <span data-ttu-id="eb9ba-266">✔</span><span class="sxs-lookup"><span data-stu-id="eb9ba-266">✔</span></span> | <span data-ttu-id="eb9ba-267">✔</span><span class="sxs-lookup"><span data-stu-id="eb9ba-267">✔</span></span> | <span data-ttu-id="eb9ba-268">✔</span><span class="sxs-lookup"><span data-stu-id="eb9ba-268">✔</span></span>
<span data-ttu-id="eb9ba-269">Geometry. Envelope</span><span class="sxs-lookup"><span data-stu-id="eb9ba-269">Geometry.Envelope</span></span> | <span data-ttu-id="eb9ba-270">✔</span><span class="sxs-lookup"><span data-stu-id="eb9ba-270">✔</span></span> | | <span data-ttu-id="eb9ba-271">✔</span><span class="sxs-lookup"><span data-stu-id="eb9ba-271">✔</span></span> | <span data-ttu-id="eb9ba-272">✔</span><span class="sxs-lookup"><span data-stu-id="eb9ba-272">✔</span></span>
<span data-ttu-id="eb9ba-273">Geometry. EqualsExact (Geometry)</span><span class="sxs-lookup"><span data-stu-id="eb9ba-273">Geometry.EqualsExact(Geometry)</span></span> | | | | <span data-ttu-id="eb9ba-274">✔</span><span class="sxs-lookup"><span data-stu-id="eb9ba-274">✔</span></span>
<span data-ttu-id="eb9ba-275">Geometry. EqualsTopologically (Geometry)</span><span class="sxs-lookup"><span data-stu-id="eb9ba-275">Geometry.EqualsTopologically(Geometry)</span></span> | <span data-ttu-id="eb9ba-276">✔</span><span class="sxs-lookup"><span data-stu-id="eb9ba-276">✔</span></span> | <span data-ttu-id="eb9ba-277">✔</span><span class="sxs-lookup"><span data-stu-id="eb9ba-277">✔</span></span> | <span data-ttu-id="eb9ba-278">✔</span><span class="sxs-lookup"><span data-stu-id="eb9ba-278">✔</span></span> | <span data-ttu-id="eb9ba-279">✔</span><span class="sxs-lookup"><span data-stu-id="eb9ba-279">✔</span></span>
<span data-ttu-id="eb9ba-280">Geometry. GeometryType</span><span class="sxs-lookup"><span data-stu-id="eb9ba-280">Geometry.GeometryType</span></span> | <span data-ttu-id="eb9ba-281">✔</span><span class="sxs-lookup"><span data-stu-id="eb9ba-281">✔</span></span> | <span data-ttu-id="eb9ba-282">✔</span><span class="sxs-lookup"><span data-stu-id="eb9ba-282">✔</span></span> | <span data-ttu-id="eb9ba-283">✔</span><span class="sxs-lookup"><span data-stu-id="eb9ba-283">✔</span></span> | <span data-ttu-id="eb9ba-284">✔</span><span class="sxs-lookup"><span data-stu-id="eb9ba-284">✔</span></span>
<span data-ttu-id="eb9ba-285">Geometry. getgeometryn (int)</span><span class="sxs-lookup"><span data-stu-id="eb9ba-285">Geometry.GetGeometryN(int)</span></span> | <span data-ttu-id="eb9ba-286">✔</span><span class="sxs-lookup"><span data-stu-id="eb9ba-286">✔</span></span> | | <span data-ttu-id="eb9ba-287">✔</span><span class="sxs-lookup"><span data-stu-id="eb9ba-287">✔</span></span> | <span data-ttu-id="eb9ba-288">✔</span><span class="sxs-lookup"><span data-stu-id="eb9ba-288">✔</span></span>
<span data-ttu-id="eb9ba-289">Geometry. InteriorPoint</span><span class="sxs-lookup"><span data-stu-id="eb9ba-289">Geometry.InteriorPoint</span></span> | <span data-ttu-id="eb9ba-290">✔</span><span class="sxs-lookup"><span data-stu-id="eb9ba-290">✔</span></span> | | <span data-ttu-id="eb9ba-291">✔</span><span class="sxs-lookup"><span data-stu-id="eb9ba-291">✔</span></span> | <span data-ttu-id="eb9ba-292">✔</span><span class="sxs-lookup"><span data-stu-id="eb9ba-292">✔</span></span>
<span data-ttu-id="eb9ba-293">Geometry. intersezione (Geometry)</span><span class="sxs-lookup"><span data-stu-id="eb9ba-293">Geometry.Intersection(Geometry)</span></span> | <span data-ttu-id="eb9ba-294">✔</span><span class="sxs-lookup"><span data-stu-id="eb9ba-294">✔</span></span> | <span data-ttu-id="eb9ba-295">✔</span><span class="sxs-lookup"><span data-stu-id="eb9ba-295">✔</span></span> | <span data-ttu-id="eb9ba-296">✔</span><span class="sxs-lookup"><span data-stu-id="eb9ba-296">✔</span></span> | <span data-ttu-id="eb9ba-297">✔</span><span class="sxs-lookup"><span data-stu-id="eb9ba-297">✔</span></span>
<span data-ttu-id="eb9ba-298">Geometry. Intersects (Geometry)</span><span class="sxs-lookup"><span data-stu-id="eb9ba-298">Geometry.Intersects(Geometry)</span></span> | <span data-ttu-id="eb9ba-299">✔</span><span class="sxs-lookup"><span data-stu-id="eb9ba-299">✔</span></span> | <span data-ttu-id="eb9ba-300">✔</span><span class="sxs-lookup"><span data-stu-id="eb9ba-300">✔</span></span> | <span data-ttu-id="eb9ba-301">✔</span><span class="sxs-lookup"><span data-stu-id="eb9ba-301">✔</span></span> | <span data-ttu-id="eb9ba-302">✔</span><span class="sxs-lookup"><span data-stu-id="eb9ba-302">✔</span></span>
<span data-ttu-id="eb9ba-303">Geometry. IsEmpty</span><span class="sxs-lookup"><span data-stu-id="eb9ba-303">Geometry.IsEmpty</span></span> | <span data-ttu-id="eb9ba-304">✔</span><span class="sxs-lookup"><span data-stu-id="eb9ba-304">✔</span></span> | <span data-ttu-id="eb9ba-305">✔</span><span class="sxs-lookup"><span data-stu-id="eb9ba-305">✔</span></span> | <span data-ttu-id="eb9ba-306">✔</span><span class="sxs-lookup"><span data-stu-id="eb9ba-306">✔</span></span> | <span data-ttu-id="eb9ba-307">✔</span><span class="sxs-lookup"><span data-stu-id="eb9ba-307">✔</span></span>
<span data-ttu-id="eb9ba-308">Geometry. Simple</span><span class="sxs-lookup"><span data-stu-id="eb9ba-308">Geometry.IsSimple</span></span> | <span data-ttu-id="eb9ba-309">✔</span><span class="sxs-lookup"><span data-stu-id="eb9ba-309">✔</span></span> | | <span data-ttu-id="eb9ba-310">✔</span><span class="sxs-lookup"><span data-stu-id="eb9ba-310">✔</span></span> | <span data-ttu-id="eb9ba-311">✔</span><span class="sxs-lookup"><span data-stu-id="eb9ba-311">✔</span></span>
<span data-ttu-id="eb9ba-312">Geometry. IsValid</span><span class="sxs-lookup"><span data-stu-id="eb9ba-312">Geometry.IsValid</span></span> | <span data-ttu-id="eb9ba-313">✔</span><span class="sxs-lookup"><span data-stu-id="eb9ba-313">✔</span></span> | <span data-ttu-id="eb9ba-314">✔</span><span class="sxs-lookup"><span data-stu-id="eb9ba-314">✔</span></span> | <span data-ttu-id="eb9ba-315">✔</span><span class="sxs-lookup"><span data-stu-id="eb9ba-315">✔</span></span> | <span data-ttu-id="eb9ba-316">✔</span><span class="sxs-lookup"><span data-stu-id="eb9ba-316">✔</span></span>
<span data-ttu-id="eb9ba-317">Geometry. IsWithinDistance (Geometry, Double)</span><span class="sxs-lookup"><span data-stu-id="eb9ba-317">Geometry.IsWithinDistance(Geometry, double)</span></span> | <span data-ttu-id="eb9ba-318">✔</span><span class="sxs-lookup"><span data-stu-id="eb9ba-318">✔</span></span> | | <span data-ttu-id="eb9ba-319">✔</span><span class="sxs-lookup"><span data-stu-id="eb9ba-319">✔</span></span> | <span data-ttu-id="eb9ba-320">✔</span><span class="sxs-lookup"><span data-stu-id="eb9ba-320">✔</span></span>
<span data-ttu-id="eb9ba-321">Geometry. length</span><span class="sxs-lookup"><span data-stu-id="eb9ba-321">Geometry.Length</span></span> | <span data-ttu-id="eb9ba-322">✔</span><span class="sxs-lookup"><span data-stu-id="eb9ba-322">✔</span></span> | <span data-ttu-id="eb9ba-323">✔</span><span class="sxs-lookup"><span data-stu-id="eb9ba-323">✔</span></span> | <span data-ttu-id="eb9ba-324">✔</span><span class="sxs-lookup"><span data-stu-id="eb9ba-324">✔</span></span> | <span data-ttu-id="eb9ba-325">✔</span><span class="sxs-lookup"><span data-stu-id="eb9ba-325">✔</span></span>
<span data-ttu-id="eb9ba-326">Geometry. NumGeometries</span><span class="sxs-lookup"><span data-stu-id="eb9ba-326">Geometry.NumGeometries</span></span> | <span data-ttu-id="eb9ba-327">✔</span><span class="sxs-lookup"><span data-stu-id="eb9ba-327">✔</span></span> | <span data-ttu-id="eb9ba-328">✔</span><span class="sxs-lookup"><span data-stu-id="eb9ba-328">✔</span></span> | <span data-ttu-id="eb9ba-329">✔</span><span class="sxs-lookup"><span data-stu-id="eb9ba-329">✔</span></span> | <span data-ttu-id="eb9ba-330">✔</span><span class="sxs-lookup"><span data-stu-id="eb9ba-330">✔</span></span>
<span data-ttu-id="eb9ba-331">Geometry. NumPoints</span><span class="sxs-lookup"><span data-stu-id="eb9ba-331">Geometry.NumPoints</span></span> | <span data-ttu-id="eb9ba-332">✔</span><span class="sxs-lookup"><span data-stu-id="eb9ba-332">✔</span></span> | <span data-ttu-id="eb9ba-333">✔</span><span class="sxs-lookup"><span data-stu-id="eb9ba-333">✔</span></span> | <span data-ttu-id="eb9ba-334">✔</span><span class="sxs-lookup"><span data-stu-id="eb9ba-334">✔</span></span> | <span data-ttu-id="eb9ba-335">✔</span><span class="sxs-lookup"><span data-stu-id="eb9ba-335">✔</span></span>
<span data-ttu-id="eb9ba-336">Geometry. OgcGeometryType</span><span class="sxs-lookup"><span data-stu-id="eb9ba-336">Geometry.OgcGeometryType</span></span> | <span data-ttu-id="eb9ba-337">✔</span><span class="sxs-lookup"><span data-stu-id="eb9ba-337">✔</span></span> | <span data-ttu-id="eb9ba-338">✔</span><span class="sxs-lookup"><span data-stu-id="eb9ba-338">✔</span></span> | <span data-ttu-id="eb9ba-339">✔</span><span class="sxs-lookup"><span data-stu-id="eb9ba-339">✔</span></span> | <span data-ttu-id="eb9ba-340">✔</span><span class="sxs-lookup"><span data-stu-id="eb9ba-340">✔</span></span>
<span data-ttu-id="eb9ba-341">Geometry. sovrapposizioni (Geometry)</span><span class="sxs-lookup"><span data-stu-id="eb9ba-341">Geometry.Overlaps(Geometry)</span></span> | <span data-ttu-id="eb9ba-342">✔</span><span class="sxs-lookup"><span data-stu-id="eb9ba-342">✔</span></span> | <span data-ttu-id="eb9ba-343">✔</span><span class="sxs-lookup"><span data-stu-id="eb9ba-343">✔</span></span> | <span data-ttu-id="eb9ba-344">✔</span><span class="sxs-lookup"><span data-stu-id="eb9ba-344">✔</span></span> | <span data-ttu-id="eb9ba-345">✔</span><span class="sxs-lookup"><span data-stu-id="eb9ba-345">✔</span></span>
<span data-ttu-id="eb9ba-346">Geometry. PointOnSurface</span><span class="sxs-lookup"><span data-stu-id="eb9ba-346">Geometry.PointOnSurface</span></span> | <span data-ttu-id="eb9ba-347">✔</span><span class="sxs-lookup"><span data-stu-id="eb9ba-347">✔</span></span> | | <span data-ttu-id="eb9ba-348">✔</span><span class="sxs-lookup"><span data-stu-id="eb9ba-348">✔</span></span> | <span data-ttu-id="eb9ba-349">✔</span><span class="sxs-lookup"><span data-stu-id="eb9ba-349">✔</span></span>
<span data-ttu-id="eb9ba-350">Geometry. correlate (Geometry, String)</span><span class="sxs-lookup"><span data-stu-id="eb9ba-350">Geometry.Relate(Geometry, string)</span></span> | <span data-ttu-id="eb9ba-351">✔</span><span class="sxs-lookup"><span data-stu-id="eb9ba-351">✔</span></span> | | <span data-ttu-id="eb9ba-352">✔</span><span class="sxs-lookup"><span data-stu-id="eb9ba-352">✔</span></span> | <span data-ttu-id="eb9ba-353">✔</span><span class="sxs-lookup"><span data-stu-id="eb9ba-353">✔</span></span>
<span data-ttu-id="eb9ba-354">Geometry. Reverse ()</span><span class="sxs-lookup"><span data-stu-id="eb9ba-354">Geometry.Reverse()</span></span> | | | <span data-ttu-id="eb9ba-355">✔</span><span class="sxs-lookup"><span data-stu-id="eb9ba-355">✔</span></span> | <span data-ttu-id="eb9ba-356">✔</span><span class="sxs-lookup"><span data-stu-id="eb9ba-356">✔</span></span>
<span data-ttu-id="eb9ba-357">Geometry. SRID</span><span class="sxs-lookup"><span data-stu-id="eb9ba-357">Geometry.SRID</span></span> | <span data-ttu-id="eb9ba-358">✔</span><span class="sxs-lookup"><span data-stu-id="eb9ba-358">✔</span></span> | <span data-ttu-id="eb9ba-359">✔</span><span class="sxs-lookup"><span data-stu-id="eb9ba-359">✔</span></span> | <span data-ttu-id="eb9ba-360">✔</span><span class="sxs-lookup"><span data-stu-id="eb9ba-360">✔</span></span> | <span data-ttu-id="eb9ba-361">✔</span><span class="sxs-lookup"><span data-stu-id="eb9ba-361">✔</span></span>
<span data-ttu-id="eb9ba-362">Geometry. SymmetricDifference (Geometry)</span><span class="sxs-lookup"><span data-stu-id="eb9ba-362">Geometry.SymmetricDifference(Geometry)</span></span> | <span data-ttu-id="eb9ba-363">✔</span><span class="sxs-lookup"><span data-stu-id="eb9ba-363">✔</span></span> | <span data-ttu-id="eb9ba-364">✔</span><span class="sxs-lookup"><span data-stu-id="eb9ba-364">✔</span></span> | <span data-ttu-id="eb9ba-365">✔</span><span class="sxs-lookup"><span data-stu-id="eb9ba-365">✔</span></span> | <span data-ttu-id="eb9ba-366">✔</span><span class="sxs-lookup"><span data-stu-id="eb9ba-366">✔</span></span>
<span data-ttu-id="eb9ba-367">Geometry. ToBinary ()</span><span class="sxs-lookup"><span data-stu-id="eb9ba-367">Geometry.ToBinary()</span></span> | <span data-ttu-id="eb9ba-368">✔</span><span class="sxs-lookup"><span data-stu-id="eb9ba-368">✔</span></span> | <span data-ttu-id="eb9ba-369">✔</span><span class="sxs-lookup"><span data-stu-id="eb9ba-369">✔</span></span> | <span data-ttu-id="eb9ba-370">✔</span><span class="sxs-lookup"><span data-stu-id="eb9ba-370">✔</span></span> | <span data-ttu-id="eb9ba-371">✔</span><span class="sxs-lookup"><span data-stu-id="eb9ba-371">✔</span></span>
<span data-ttu-id="eb9ba-372">Geometry. ToText ()</span><span class="sxs-lookup"><span data-stu-id="eb9ba-372">Geometry.ToText()</span></span> | <span data-ttu-id="eb9ba-373">✔</span><span class="sxs-lookup"><span data-stu-id="eb9ba-373">✔</span></span> | <span data-ttu-id="eb9ba-374">✔</span><span class="sxs-lookup"><span data-stu-id="eb9ba-374">✔</span></span> | <span data-ttu-id="eb9ba-375">✔</span><span class="sxs-lookup"><span data-stu-id="eb9ba-375">✔</span></span> | <span data-ttu-id="eb9ba-376">✔</span><span class="sxs-lookup"><span data-stu-id="eb9ba-376">✔</span></span>
<span data-ttu-id="eb9ba-377">Geometry. touchs (Geometry)</span><span class="sxs-lookup"><span data-stu-id="eb9ba-377">Geometry.Touches(Geometry)</span></span> | <span data-ttu-id="eb9ba-378">✔</span><span class="sxs-lookup"><span data-stu-id="eb9ba-378">✔</span></span> | | <span data-ttu-id="eb9ba-379">✔</span><span class="sxs-lookup"><span data-stu-id="eb9ba-379">✔</span></span> | <span data-ttu-id="eb9ba-380">✔</span><span class="sxs-lookup"><span data-stu-id="eb9ba-380">✔</span></span>
<span data-ttu-id="eb9ba-381">Geometry. Union ()</span><span class="sxs-lookup"><span data-stu-id="eb9ba-381">Geometry.Union()</span></span> | | | <span data-ttu-id="eb9ba-382">✔</span><span class="sxs-lookup"><span data-stu-id="eb9ba-382">✔</span></span> | <span data-ttu-id="eb9ba-383">✔</span><span class="sxs-lookup"><span data-stu-id="eb9ba-383">✔</span></span>
<span data-ttu-id="eb9ba-384">Geometry. Union (Geometry)</span><span class="sxs-lookup"><span data-stu-id="eb9ba-384">Geometry.Union(Geometry)</span></span> | <span data-ttu-id="eb9ba-385">✔</span><span class="sxs-lookup"><span data-stu-id="eb9ba-385">✔</span></span> | <span data-ttu-id="eb9ba-386">✔</span><span class="sxs-lookup"><span data-stu-id="eb9ba-386">✔</span></span> | <span data-ttu-id="eb9ba-387">✔</span><span class="sxs-lookup"><span data-stu-id="eb9ba-387">✔</span></span> | <span data-ttu-id="eb9ba-388">✔</span><span class="sxs-lookup"><span data-stu-id="eb9ba-388">✔</span></span>
<span data-ttu-id="eb9ba-389">Geometry. Within (Geometry)</span><span class="sxs-lookup"><span data-stu-id="eb9ba-389">Geometry.Within(Geometry)</span></span> | <span data-ttu-id="eb9ba-390">✔</span><span class="sxs-lookup"><span data-stu-id="eb9ba-390">✔</span></span> | <span data-ttu-id="eb9ba-391">✔</span><span class="sxs-lookup"><span data-stu-id="eb9ba-391">✔</span></span> | <span data-ttu-id="eb9ba-392">✔</span><span class="sxs-lookup"><span data-stu-id="eb9ba-392">✔</span></span> | <span data-ttu-id="eb9ba-393">✔</span><span class="sxs-lookup"><span data-stu-id="eb9ba-393">✔</span></span>
<span data-ttu-id="eb9ba-394">GeometryCollection. Count</span><span class="sxs-lookup"><span data-stu-id="eb9ba-394">GeometryCollection.Count</span></span> | <span data-ttu-id="eb9ba-395">✔</span><span class="sxs-lookup"><span data-stu-id="eb9ba-395">✔</span></span> | <span data-ttu-id="eb9ba-396">✔</span><span class="sxs-lookup"><span data-stu-id="eb9ba-396">✔</span></span> | <span data-ttu-id="eb9ba-397">✔</span><span class="sxs-lookup"><span data-stu-id="eb9ba-397">✔</span></span> | <span data-ttu-id="eb9ba-398">✔</span><span class="sxs-lookup"><span data-stu-id="eb9ba-398">✔</span></span>
<span data-ttu-id="eb9ba-399">GeometryCollection [int]</span><span class="sxs-lookup"><span data-stu-id="eb9ba-399">GeometryCollection[int]</span></span> | <span data-ttu-id="eb9ba-400">✔</span><span class="sxs-lookup"><span data-stu-id="eb9ba-400">✔</span></span> | <span data-ttu-id="eb9ba-401">✔</span><span class="sxs-lookup"><span data-stu-id="eb9ba-401">✔</span></span> | <span data-ttu-id="eb9ba-402">✔</span><span class="sxs-lookup"><span data-stu-id="eb9ba-402">✔</span></span> | <span data-ttu-id="eb9ba-403">✔</span><span class="sxs-lookup"><span data-stu-id="eb9ba-403">✔</span></span>
<span data-ttu-id="eb9ba-404">LineString. Count</span><span class="sxs-lookup"><span data-stu-id="eb9ba-404">LineString.Count</span></span> | <span data-ttu-id="eb9ba-405">✔</span><span class="sxs-lookup"><span data-stu-id="eb9ba-405">✔</span></span> | <span data-ttu-id="eb9ba-406">✔</span><span class="sxs-lookup"><span data-stu-id="eb9ba-406">✔</span></span> | <span data-ttu-id="eb9ba-407">✔</span><span class="sxs-lookup"><span data-stu-id="eb9ba-407">✔</span></span> | <span data-ttu-id="eb9ba-408">✔</span><span class="sxs-lookup"><span data-stu-id="eb9ba-408">✔</span></span>
<span data-ttu-id="eb9ba-409">LineString. EndPoint</span><span class="sxs-lookup"><span data-stu-id="eb9ba-409">LineString.EndPoint</span></span> | <span data-ttu-id="eb9ba-410">✔</span><span class="sxs-lookup"><span data-stu-id="eb9ba-410">✔</span></span> | <span data-ttu-id="eb9ba-411">✔</span><span class="sxs-lookup"><span data-stu-id="eb9ba-411">✔</span></span> | <span data-ttu-id="eb9ba-412">✔</span><span class="sxs-lookup"><span data-stu-id="eb9ba-412">✔</span></span> | <span data-ttu-id="eb9ba-413">✔</span><span class="sxs-lookup"><span data-stu-id="eb9ba-413">✔</span></span>
<span data-ttu-id="eb9ba-414">LineString. getpointn (int)</span><span class="sxs-lookup"><span data-stu-id="eb9ba-414">LineString.GetPointN(int)</span></span> | <span data-ttu-id="eb9ba-415">✔</span><span class="sxs-lookup"><span data-stu-id="eb9ba-415">✔</span></span> | <span data-ttu-id="eb9ba-416">✔</span><span class="sxs-lookup"><span data-stu-id="eb9ba-416">✔</span></span> | <span data-ttu-id="eb9ba-417">✔</span><span class="sxs-lookup"><span data-stu-id="eb9ba-417">✔</span></span> | <span data-ttu-id="eb9ba-418">✔</span><span class="sxs-lookup"><span data-stu-id="eb9ba-418">✔</span></span>
<span data-ttu-id="eb9ba-419">LineString. di chiusura</span><span class="sxs-lookup"><span data-stu-id="eb9ba-419">LineString.IsClosed</span></span> | <span data-ttu-id="eb9ba-420">✔</span><span class="sxs-lookup"><span data-stu-id="eb9ba-420">✔</span></span> | <span data-ttu-id="eb9ba-421">✔</span><span class="sxs-lookup"><span data-stu-id="eb9ba-421">✔</span></span> | <span data-ttu-id="eb9ba-422">✔</span><span class="sxs-lookup"><span data-stu-id="eb9ba-422">✔</span></span> | <span data-ttu-id="eb9ba-423">✔</span><span class="sxs-lookup"><span data-stu-id="eb9ba-423">✔</span></span>
<span data-ttu-id="eb9ba-424">LineString. Ring</span><span class="sxs-lookup"><span data-stu-id="eb9ba-424">LineString.IsRing</span></span> | <span data-ttu-id="eb9ba-425">✔</span><span class="sxs-lookup"><span data-stu-id="eb9ba-425">✔</span></span> | | <span data-ttu-id="eb9ba-426">✔</span><span class="sxs-lookup"><span data-stu-id="eb9ba-426">✔</span></span> | <span data-ttu-id="eb9ba-427">✔</span><span class="sxs-lookup"><span data-stu-id="eb9ba-427">✔</span></span>
<span data-ttu-id="eb9ba-428">LineString. StartPoint</span><span class="sxs-lookup"><span data-stu-id="eb9ba-428">LineString.StartPoint</span></span> | <span data-ttu-id="eb9ba-429">✔</span><span class="sxs-lookup"><span data-stu-id="eb9ba-429">✔</span></span> | <span data-ttu-id="eb9ba-430">✔</span><span class="sxs-lookup"><span data-stu-id="eb9ba-430">✔</span></span> | <span data-ttu-id="eb9ba-431">✔</span><span class="sxs-lookup"><span data-stu-id="eb9ba-431">✔</span></span> | <span data-ttu-id="eb9ba-432">✔</span><span class="sxs-lookup"><span data-stu-id="eb9ba-432">✔</span></span>
<span data-ttu-id="eb9ba-433">MultiLineString. noclosed</span><span class="sxs-lookup"><span data-stu-id="eb9ba-433">MultiLineString.IsClosed</span></span> | <span data-ttu-id="eb9ba-434">✔</span><span class="sxs-lookup"><span data-stu-id="eb9ba-434">✔</span></span> | <span data-ttu-id="eb9ba-435">✔</span><span class="sxs-lookup"><span data-stu-id="eb9ba-435">✔</span></span> | <span data-ttu-id="eb9ba-436">✔</span><span class="sxs-lookup"><span data-stu-id="eb9ba-436">✔</span></span> | <span data-ttu-id="eb9ba-437">✔</span><span class="sxs-lookup"><span data-stu-id="eb9ba-437">✔</span></span>
<span data-ttu-id="eb9ba-438">Punto. M</span><span class="sxs-lookup"><span data-stu-id="eb9ba-438">Point.M</span></span> | <span data-ttu-id="eb9ba-439">✔</span><span class="sxs-lookup"><span data-stu-id="eb9ba-439">✔</span></span> | <span data-ttu-id="eb9ba-440">✔</span><span class="sxs-lookup"><span data-stu-id="eb9ba-440">✔</span></span> | <span data-ttu-id="eb9ba-441">✔</span><span class="sxs-lookup"><span data-stu-id="eb9ba-441">✔</span></span> | <span data-ttu-id="eb9ba-442">✔</span><span class="sxs-lookup"><span data-stu-id="eb9ba-442">✔</span></span>
<span data-ttu-id="eb9ba-443">Punto. X</span><span class="sxs-lookup"><span data-stu-id="eb9ba-443">Point.X</span></span> | <span data-ttu-id="eb9ba-444">✔</span><span class="sxs-lookup"><span data-stu-id="eb9ba-444">✔</span></span> | <span data-ttu-id="eb9ba-445">✔</span><span class="sxs-lookup"><span data-stu-id="eb9ba-445">✔</span></span> | <span data-ttu-id="eb9ba-446">✔</span><span class="sxs-lookup"><span data-stu-id="eb9ba-446">✔</span></span> | <span data-ttu-id="eb9ba-447">✔</span><span class="sxs-lookup"><span data-stu-id="eb9ba-447">✔</span></span>
<span data-ttu-id="eb9ba-448">Punto. Y</span><span class="sxs-lookup"><span data-stu-id="eb9ba-448">Point.Y</span></span> | <span data-ttu-id="eb9ba-449">✔</span><span class="sxs-lookup"><span data-stu-id="eb9ba-449">✔</span></span> | <span data-ttu-id="eb9ba-450">✔</span><span class="sxs-lookup"><span data-stu-id="eb9ba-450">✔</span></span> | <span data-ttu-id="eb9ba-451">✔</span><span class="sxs-lookup"><span data-stu-id="eb9ba-451">✔</span></span> | <span data-ttu-id="eb9ba-452">✔</span><span class="sxs-lookup"><span data-stu-id="eb9ba-452">✔</span></span>
<span data-ttu-id="eb9ba-453">Point. Z</span><span class="sxs-lookup"><span data-stu-id="eb9ba-453">Point.Z</span></span> | <span data-ttu-id="eb9ba-454">✔</span><span class="sxs-lookup"><span data-stu-id="eb9ba-454">✔</span></span> | <span data-ttu-id="eb9ba-455">✔</span><span class="sxs-lookup"><span data-stu-id="eb9ba-455">✔</span></span> | <span data-ttu-id="eb9ba-456">✔</span><span class="sxs-lookup"><span data-stu-id="eb9ba-456">✔</span></span> | <span data-ttu-id="eb9ba-457">✔</span><span class="sxs-lookup"><span data-stu-id="eb9ba-457">✔</span></span>
<span data-ttu-id="eb9ba-458">Poligono. ExteriorRing</span><span class="sxs-lookup"><span data-stu-id="eb9ba-458">Polygon.ExteriorRing</span></span> | <span data-ttu-id="eb9ba-459">✔</span><span class="sxs-lookup"><span data-stu-id="eb9ba-459">✔</span></span> | <span data-ttu-id="eb9ba-460">✔</span><span class="sxs-lookup"><span data-stu-id="eb9ba-460">✔</span></span> | <span data-ttu-id="eb9ba-461">✔</span><span class="sxs-lookup"><span data-stu-id="eb9ba-461">✔</span></span> | <span data-ttu-id="eb9ba-462">✔</span><span class="sxs-lookup"><span data-stu-id="eb9ba-462">✔</span></span>
<span data-ttu-id="eb9ba-463">Poligono. GetInteriorRingN (int)</span><span class="sxs-lookup"><span data-stu-id="eb9ba-463">Polygon.GetInteriorRingN(int)</span></span> | <span data-ttu-id="eb9ba-464">✔</span><span class="sxs-lookup"><span data-stu-id="eb9ba-464">✔</span></span> | <span data-ttu-id="eb9ba-465">✔</span><span class="sxs-lookup"><span data-stu-id="eb9ba-465">✔</span></span> | <span data-ttu-id="eb9ba-466">✔</span><span class="sxs-lookup"><span data-stu-id="eb9ba-466">✔</span></span> | <span data-ttu-id="eb9ba-467">✔</span><span class="sxs-lookup"><span data-stu-id="eb9ba-467">✔</span></span>
<span data-ttu-id="eb9ba-468">Poligono. NumInteriorRings</span><span class="sxs-lookup"><span data-stu-id="eb9ba-468">Polygon.NumInteriorRings</span></span> | <span data-ttu-id="eb9ba-469">✔</span><span class="sxs-lookup"><span data-stu-id="eb9ba-469">✔</span></span> | <span data-ttu-id="eb9ba-470">✔</span><span class="sxs-lookup"><span data-stu-id="eb9ba-470">✔</span></span> | <span data-ttu-id="eb9ba-471">✔</span><span class="sxs-lookup"><span data-stu-id="eb9ba-471">✔</span></span> | <span data-ttu-id="eb9ba-472">✔</span><span class="sxs-lookup"><span data-stu-id="eb9ba-472">✔</span></span>

## <a name="additional-resources"></a><span data-ttu-id="eb9ba-473">Risorse aggiuntive</span><span class="sxs-lookup"><span data-stu-id="eb9ba-473">Additional resources</span></span>

* [<span data-ttu-id="eb9ba-474">Dati spaziali in SQL Server</span><span class="sxs-lookup"><span data-stu-id="eb9ba-474">Spatial Data in SQL Server</span></span>](https://docs.microsoft.com/sql/relational-databases/spatial/spatial-data-sql-server)
* [<span data-ttu-id="eb9ba-475">Home page di SpatiaLite</span><span class="sxs-lookup"><span data-stu-id="eb9ba-475">SpatiaLite Homepage</span></span>](https://www.gaia-gis.it/fossil/libspatialite)
* [<span data-ttu-id="eb9ba-476">Documentazione spaziale di npgsql</span><span class="sxs-lookup"><span data-stu-id="eb9ba-476">Npgsql Spatial Documentation</span></span>](https://www.npgsql.org/efcore/mapping/nts.html)
* [<span data-ttu-id="eb9ba-477">Documentazione di PostGIS</span><span class="sxs-lookup"><span data-stu-id="eb9ba-477">PostGIS Documentation</span></span>](https://postgis.net/documentation/)
