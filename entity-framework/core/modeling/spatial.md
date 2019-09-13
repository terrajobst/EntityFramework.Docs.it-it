---
title: Dati spaziali-EF Core
author: bricelam
ms.author: bricelam
ms.date: 11/01/2018
ms.assetid: 2BDE29FC-4161-41A0-841E-69F51CCD9341
uid: core/modeling/spatial
ms.openlocfilehash: 026df735473e31f1c1463c1fbc6f46c4fd6dfd4f
ms.sourcegitcommit: b2b9468de2cf930687f8b85c3ce54ff8c449f644
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/12/2019
ms.locfileid: "70921727"
---
# <a name="spatial-data"></a><span data-ttu-id="9ea8b-102">Dati spaziali</span><span class="sxs-lookup"><span data-stu-id="9ea8b-102">Spatial Data</span></span>

> [!NOTE]
> <span data-ttu-id="9ea8b-103">Questa funzionalità è stata aggiunta in EF Core 2,2.</span><span class="sxs-lookup"><span data-stu-id="9ea8b-103">This feature was added in EF Core 2.2.</span></span>

<span data-ttu-id="9ea8b-104">I dati spaziali rappresentano la posizione fisica e la forma degli oggetti.</span><span class="sxs-lookup"><span data-stu-id="9ea8b-104">Spatial data represents the physical location and the shape of objects.</span></span> <span data-ttu-id="9ea8b-105">Molti database forniscono supporto per questo tipo di dati, in modo che possa essere indicizzato e sottoposto a query insieme ad altri dati.</span><span class="sxs-lookup"><span data-stu-id="9ea8b-105">Many databases provide support for this type of data so it can be indexed and queried alongside other data.</span></span> <span data-ttu-id="9ea8b-106">Gli scenari comuni includono l'esecuzione di query per gli oggetti all'interno di una distanza specificata da una posizione o la selezione dell'oggetto il cui bordo contiene una determinata posizione.</span><span class="sxs-lookup"><span data-stu-id="9ea8b-106">Common scenarios include querying for objects within a given distance from a location, or selecting the object whose border contains a given location.</span></span> <span data-ttu-id="9ea8b-107">EF Core supporta il mapping ai tipi di dati spaziali mediante la libreria spaziale [NetTopologySuite](https://github.com/NetTopologySuite/NetTopologySuite) .</span><span class="sxs-lookup"><span data-stu-id="9ea8b-107">EF Core supports mapping to spatial data types using the [NetTopologySuite](https://github.com/NetTopologySuite/NetTopologySuite) spatial library.</span></span>

## <a name="installing"></a><span data-ttu-id="9ea8b-108">Installazione di</span><span class="sxs-lookup"><span data-stu-id="9ea8b-108">Installing</span></span>

<span data-ttu-id="9ea8b-109">Per usare i dati spaziali con EF Core, è necessario installare il pacchetto NuGet di supporto appropriato.</span><span class="sxs-lookup"><span data-stu-id="9ea8b-109">In order to use spatial data with EF Core, you need to install the appropriate supporting NuGet package.</span></span> <span data-ttu-id="9ea8b-110">Il pacchetto che è necessario installare dipende dal provider in uso.</span><span class="sxs-lookup"><span data-stu-id="9ea8b-110">Which package you need to install depends on the provider you're using.</span></span>

<span data-ttu-id="9ea8b-111">Provider di EF Core</span><span class="sxs-lookup"><span data-stu-id="9ea8b-111">EF Core Provider</span></span>                        | <span data-ttu-id="9ea8b-112">Pacchetto NuGet spaziale</span><span class="sxs-lookup"><span data-stu-id="9ea8b-112">Spatial NuGet Package</span></span>
--------------------------------------- | ---------------------
<span data-ttu-id="9ea8b-113">Microsoft.EntityFrameworkCore.SqlServer</span><span class="sxs-lookup"><span data-stu-id="9ea8b-113">Microsoft.EntityFrameworkCore.SqlServer</span></span> | [<span data-ttu-id="9ea8b-114">Microsoft. EntityFrameworkCore. SqlServer. NetTopologySuite</span><span class="sxs-lookup"><span data-stu-id="9ea8b-114">Microsoft.EntityFrameworkCore.SqlServer.NetTopologySuite</span></span>](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.SqlServer.NetTopologySuite)
<span data-ttu-id="9ea8b-115">Microsoft.EntityFrameworkCore.Sqlite</span><span class="sxs-lookup"><span data-stu-id="9ea8b-115">Microsoft.EntityFrameworkCore.Sqlite</span></span>    | [<span data-ttu-id="9ea8b-116">Microsoft. EntityFrameworkCore. sqlite. NetTopologySuite</span><span class="sxs-lookup"><span data-stu-id="9ea8b-116">Microsoft.EntityFrameworkCore.Sqlite.NetTopologySuite</span></span>](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Sqlite.NetTopologySuite)
<span data-ttu-id="9ea8b-117">Microsoft.EntityFrameworkCore.InMemory</span><span class="sxs-lookup"><span data-stu-id="9ea8b-117">Microsoft.EntityFrameworkCore.InMemory</span></span>  | [<span data-ttu-id="9ea8b-118">NetTopologySuite</span><span class="sxs-lookup"><span data-stu-id="9ea8b-118">NetTopologySuite</span></span>](https://www.nuget.org/packages/NetTopologySuite)
<span data-ttu-id="9ea8b-119">Npgsql.EntityFrameworkCore.PostgreSQL</span><span class="sxs-lookup"><span data-stu-id="9ea8b-119">Npgsql.EntityFrameworkCore.PostgreSQL</span></span>   | [<span data-ttu-id="9ea8b-120">Npgsql. EntityFrameworkCore. PostgreSQL. NetTopologySuite</span><span class="sxs-lookup"><span data-stu-id="9ea8b-120">Npgsql.EntityFrameworkCore.PostgreSQL.NetTopologySuite</span></span>](https://www.nuget.org/packages/Npgsql.EntityFrameworkCore.PostgreSQL.NetTopologySuite)

## <a name="reverse-engineering"></a><span data-ttu-id="9ea8b-121">Reverse Engineering</span><span class="sxs-lookup"><span data-stu-id="9ea8b-121">Reverse engineering</span></span>

<span data-ttu-id="9ea8b-122">I pacchetti NuGet spaziali abilitano anche [Reverse Engineering](../managing-schemas/scaffolding.md) modelli con proprietà spaziali, ma è necessario installare il pacchetto ***prima*** di `Scaffold-DbContext` eseguire `dotnet ef dbcontext scaffold`o.</span><span class="sxs-lookup"><span data-stu-id="9ea8b-122">The spatial NuGet packages also enable [reverse engineering](../managing-schemas/scaffolding.md) models with spatial properties, but you need to install the package ***before*** running `Scaffold-DbContext` or `dotnet ef dbcontext scaffold`.</span></span> <span data-ttu-id="9ea8b-123">In caso contrario, si riceveranno avvisi relativi alla mancata individuazione dei mapping dei tipi per le colonne e le colonne verranno ignorate.</span><span class="sxs-lookup"><span data-stu-id="9ea8b-123">If you don't, you'll receive warnings about not finding type mappings for the columns and the columns will be skipped.</span></span>

## <a name="nettopologysuite-nts"></a><span data-ttu-id="9ea8b-124">NetTopologySuite (NTS)</span><span class="sxs-lookup"><span data-stu-id="9ea8b-124">NetTopologySuite (NTS)</span></span>

<span data-ttu-id="9ea8b-125">NetTopologySuite è una libreria spaziale per .NET.</span><span class="sxs-lookup"><span data-stu-id="9ea8b-125">NetTopologySuite is a spatial library for .NET.</span></span> <span data-ttu-id="9ea8b-126">EF Core Abilita il mapping ai tipi di dati spaziali nel database usando i tipi NTS nel modello.</span><span class="sxs-lookup"><span data-stu-id="9ea8b-126">EF Core enables mapping to spatial data types in the database by using NTS types in your model.</span></span>

<span data-ttu-id="9ea8b-127">Per abilitare il mapping ai tipi spaziali tramite NTS, chiamare il metodo UseNetTopologySuite nel generatore di opzioni DbContext del provider.</span><span class="sxs-lookup"><span data-stu-id="9ea8b-127">To enable mapping to spatial types via NTS, call the UseNetTopologySuite method on the provider's DbContext options builder.</span></span> <span data-ttu-id="9ea8b-128">Ad esempio, con SQL Server è possibile chiamarlo come questo.</span><span class="sxs-lookup"><span data-stu-id="9ea8b-128">For example, with SQL Server you'd call it like this.</span></span>

``` csharp
optionsBuilder.UseSqlServer(
    @"Data Source=(localdb)\MSSQLLocalDB;Initial Catalog=WideWorldImporters",
    x => x.UseNetTopologySuite());
```

<span data-ttu-id="9ea8b-129">Esistono diversi tipi di dati spaziali.</span><span class="sxs-lookup"><span data-stu-id="9ea8b-129">There are several spatial data types.</span></span> <span data-ttu-id="9ea8b-130">Il tipo da utilizzare dipende dai tipi di forme che si desidera consentire.</span><span class="sxs-lookup"><span data-stu-id="9ea8b-130">Which type you use depends on the types of shapes you want to allow.</span></span> <span data-ttu-id="9ea8b-131">Di seguito è illustrata la gerarchia di tipi NTS che è possibile usare per le proprietà nel modello.</span><span class="sxs-lookup"><span data-stu-id="9ea8b-131">Here is the hierarchy of NTS types that you can use for properties in your model.</span></span> <span data-ttu-id="9ea8b-132">Si trovano nello `NetTopologySuite.Geometries` spazio dei nomi.</span><span class="sxs-lookup"><span data-stu-id="9ea8b-132">They're located within the `NetTopologySuite.Geometries` namespace.</span></span>

* <span data-ttu-id="9ea8b-133">Geometria</span><span class="sxs-lookup"><span data-stu-id="9ea8b-133">Geometry</span></span>
  * <span data-ttu-id="9ea8b-134">Punto</span><span class="sxs-lookup"><span data-stu-id="9ea8b-134">Point</span></span>
  * <span data-ttu-id="9ea8b-135">LineString</span><span class="sxs-lookup"><span data-stu-id="9ea8b-135">LineString</span></span>
  * <span data-ttu-id="9ea8b-136">Poligono</span><span class="sxs-lookup"><span data-stu-id="9ea8b-136">Polygon</span></span>
  * <span data-ttu-id="9ea8b-137">GeometryCollection</span><span class="sxs-lookup"><span data-stu-id="9ea8b-137">GeometryCollection</span></span>
    * <span data-ttu-id="9ea8b-138">MultiPoint</span><span class="sxs-lookup"><span data-stu-id="9ea8b-138">MultiPoint</span></span>
    * <span data-ttu-id="9ea8b-139">MultiLineString</span><span class="sxs-lookup"><span data-stu-id="9ea8b-139">MultiLineString</span></span>
    * <span data-ttu-id="9ea8b-140">MultiPolygon</span><span class="sxs-lookup"><span data-stu-id="9ea8b-140">MultiPolygon</span></span>

> [!WARNING]
> <span data-ttu-id="9ea8b-141">CircularString, CompoundCurve e CurePolygon non sono supportati da NTS.</span><span class="sxs-lookup"><span data-stu-id="9ea8b-141">CircularString, CompoundCurve, and CurePolygon aren't supported by NTS.</span></span>

<span data-ttu-id="9ea8b-142">L'utilizzo del tipo Geometry di base consente di specificare qualsiasi tipo di forma dalla proprietà.</span><span class="sxs-lookup"><span data-stu-id="9ea8b-142">Using the base Geometry type allows any type of shape to be specified by the property.</span></span>

<span data-ttu-id="9ea8b-143">Per eseguire il mapping alle tabelle nel [database di esempio Wide World Importers](http://go.microsoft.com/fwlink/?LinkID=800630), è possibile usare le classi di entità seguenti.</span><span class="sxs-lookup"><span data-stu-id="9ea8b-143">The following entity classes could be used to map to tables in the [Wide World Importers sample database](http://go.microsoft.com/fwlink/?LinkID=800630).</span></span>

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

### <a name="creating-values"></a><span data-ttu-id="9ea8b-144">Creazione di valori</span><span class="sxs-lookup"><span data-stu-id="9ea8b-144">Creating values</span></span>

<span data-ttu-id="9ea8b-145">È possibile utilizzare i costruttori per creare oggetti Geometry; Tuttavia, NTS consiglia di usare invece una factory Geometry.</span><span class="sxs-lookup"><span data-stu-id="9ea8b-145">You can use constructors to create geometry objects; however, NTS recommends using a geometry factory instead.</span></span> <span data-ttu-id="9ea8b-146">In questo modo è possibile specificare un SRID predefinito (il sistema di riferimento spaziale usato dalle coordinate) e il controllo su elementi più avanzati, come il modello di precisione (usato durante i calcoli) e la sequenza di coordinate (determina le ordinate-dimensioni sono disponibili le misure e.</span><span class="sxs-lookup"><span data-stu-id="9ea8b-146">This lets you specify a default SRID (the spatial reference system used by the coordinates) and gives you control over more advanced things like the precision model (used during calculations) and the coordinate sequence (determines which ordinates--dimensions and measures--are available).</span></span>

``` csharp
var geometryFactory = NtsGeometryServices.Instance.CreateGeometryFactory(srid: 4326);
var currentLocation = geometryFactory.CreatePoint(-122.121512, 47.6739882);
```

> [!NOTE]
> <span data-ttu-id="9ea8b-147">4326 si riferisce a WGS 84, uno standard usato in GPS e in altri sistemi geografici.</span><span class="sxs-lookup"><span data-stu-id="9ea8b-147">4326 refers to WGS 84, a standard used in GPS and other geographic systems.</span></span>

### <a name="longitude-and-latitude"></a><span data-ttu-id="9ea8b-148">Longitudine e latitudine</span><span class="sxs-lookup"><span data-stu-id="9ea8b-148">Longitude and Latitude</span></span>

<span data-ttu-id="9ea8b-149">Le coordinate in NTS sono espresse in termini di valori X e Y.</span><span class="sxs-lookup"><span data-stu-id="9ea8b-149">Coordinates in NTS are in terms of X and Y values.</span></span> <span data-ttu-id="9ea8b-150">Per rappresentare la longitudine e la latitudine, usare X per longitudine e Y per la latitudine.</span><span class="sxs-lookup"><span data-stu-id="9ea8b-150">To represent longitude and latitude, use X for longitude and Y for latitude.</span></span> <span data-ttu-id="9ea8b-151">Si noti che questa operazione è all' **indietro** rispetto `latitude, longitude` al formato in cui vengono in genere visualizzati questi valori.</span><span class="sxs-lookup"><span data-stu-id="9ea8b-151">Note that this is **backwards** from the `latitude, longitude` format in which you typically see these values.</span></span>

### <a name="srid-ignored-during-client-operations"></a><span data-ttu-id="9ea8b-152">SRID ignorato durante le operazioni client</span><span class="sxs-lookup"><span data-stu-id="9ea8b-152">SRID Ignored during client operations</span></span>

<span data-ttu-id="9ea8b-153">NTS ignora i valori SRID durante le operazioni.</span><span class="sxs-lookup"><span data-stu-id="9ea8b-153">NTS ignores SRID values during operations.</span></span> <span data-ttu-id="9ea8b-154">Si presuppone un sistema di coordinate planare.</span><span class="sxs-lookup"><span data-stu-id="9ea8b-154">It assumes a planar coordinate system.</span></span> <span data-ttu-id="9ea8b-155">Ciò significa che se si specificano le coordinate in termini di longitudine e latitudine, alcuni valori valutati dal client, ad esempio distanza, lunghezza e area, saranno in gradi, non in metri.</span><span class="sxs-lookup"><span data-stu-id="9ea8b-155">This means that if you specify coordinates in terms of longitude and latitude, some client-evaluated values like distance, length, and area will be in degrees, not meters.</span></span> <span data-ttu-id="9ea8b-156">Per valori più significativi, è necessario innanzitutto proiettare le coordinate in un altro sistema di coordinate usando una libreria come [ProjNet4GeoAPI](https://github.com/NetTopologySuite/ProjNet4GeoAPI) prima di calcolare questi valori.</span><span class="sxs-lookup"><span data-stu-id="9ea8b-156">For more meaningful values, you first need to project the coordinates to another coordinate system using a library like [ProjNet4GeoAPI](https://github.com/NetTopologySuite/ProjNet4GeoAPI) before calculating these values.</span></span>

<span data-ttu-id="9ea8b-157">Se un'operazione viene valutata dal server EF Core tramite SQL, l'unità risultante sarà determinata dal database.</span><span class="sxs-lookup"><span data-stu-id="9ea8b-157">If an operation is server-evaluated by EF Core via SQL, the result's unit will be determined by the database.</span></span>

<span data-ttu-id="9ea8b-158">Di seguito è riportato un esempio di utilizzo di ProjNet4GeoAPI per calcolare la distanza tra due città.</span><span class="sxs-lookup"><span data-stu-id="9ea8b-158">Here is an example of using ProjNet4GeoAPI to calculate the distance between two cities.</span></span>

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

## <a name="querying-data"></a><span data-ttu-id="9ea8b-159">Esecuzione di query su dati</span><span class="sxs-lookup"><span data-stu-id="9ea8b-159">Querying Data</span></span>

<span data-ttu-id="9ea8b-160">In LINQ, i metodi e le proprietà NTS disponibili come funzioni di database verranno convertiti in SQL.</span><span class="sxs-lookup"><span data-stu-id="9ea8b-160">In LINQ, the NTS methods and properties available as database functions will be translated to SQL.</span></span> <span data-ttu-id="9ea8b-161">Ad esempio, i metodi Distance e Contains vengono convertiti nelle query seguenti.</span><span class="sxs-lookup"><span data-stu-id="9ea8b-161">For example, the Distance and Contains methods are translated in the following queries.</span></span> <span data-ttu-id="9ea8b-162">La tabella alla fine di questo articolo illustra i membri supportati da vari provider di EF Core.</span><span class="sxs-lookup"><span data-stu-id="9ea8b-162">The table at the end of this article shows which members are supported by various EF Core providers.</span></span>

``` csharp
var nearestCity = db.Cities
    .OrderBy(c => c.Location.Distance(currentLocation))
    .FirstOrDefault();

var currentCountry = db.Countries
    .FirstOrDefault(c => c.Border.Contains(currentLocation));
```

## <a name="sql-server"></a><span data-ttu-id="9ea8b-163">SQL Server</span><span class="sxs-lookup"><span data-stu-id="9ea8b-163">SQL Server</span></span>

<span data-ttu-id="9ea8b-164">Se si usa SQL Server, è necessario tenere presenti alcuni aspetti aggiuntivi.</span><span class="sxs-lookup"><span data-stu-id="9ea8b-164">If you're using SQL Server, there are some additional things you should be aware of.</span></span>

### <a name="geography-or-geometry"></a><span data-ttu-id="9ea8b-165">Geografia o geometria</span><span class="sxs-lookup"><span data-stu-id="9ea8b-165">Geography or geometry</span></span>

<span data-ttu-id="9ea8b-166">Per impostazione predefinita, viene eseguito il mapping `geography` delle proprietà spaziali alle colonne in SQL Server.</span><span class="sxs-lookup"><span data-stu-id="9ea8b-166">By default, spatial properties are mapped to `geography` columns in SQL Server.</span></span> <span data-ttu-id="9ea8b-167">Per utilizzare `geometry`, [configurare il tipo di colonna](xref:core/modeling/relational/data-types) nel modello.</span><span class="sxs-lookup"><span data-stu-id="9ea8b-167">To use `geometry`, [configure the column type](xref:core/modeling/relational/data-types) in your model.</span></span>

### <a name="geography-polygon-rings"></a><span data-ttu-id="9ea8b-168">Anelli del poligono geografico</span><span class="sxs-lookup"><span data-stu-id="9ea8b-168">Geography polygon rings</span></span>

<span data-ttu-id="9ea8b-169">Quando si usa `geography` il tipo di colonna, SQL Server impone requisiti aggiuntivi sull'anello esterno (o sulla Shell) e sugli anelli interni (o buchi).</span><span class="sxs-lookup"><span data-stu-id="9ea8b-169">When using the `geography` column type, SQL Server imposes additional requirements on the exterior ring (or shell) and interior rings (or holes).</span></span> <span data-ttu-id="9ea8b-170">L'anello esterno deve essere orientato in senso antiorario e gli anelli interni in senso orario.</span><span class="sxs-lookup"><span data-stu-id="9ea8b-170">The exterior ring must be oriented counterclockwise and the interior rings clockwise.</span></span> <span data-ttu-id="9ea8b-171">NTS convalida questa operazione prima di inviare valori al database.</span><span class="sxs-lookup"><span data-stu-id="9ea8b-171">NTS validates this before sending values to the database.</span></span>

### <a name="fullglobe"></a><span data-ttu-id="9ea8b-172">FullGlobe</span><span class="sxs-lookup"><span data-stu-id="9ea8b-172">FullGlobe</span></span>

<span data-ttu-id="9ea8b-173">SQL Server dispone di un tipo geometry non standard per rappresentare l'intero globo quando si utilizza `geography` il tipo di colonna.</span><span class="sxs-lookup"><span data-stu-id="9ea8b-173">SQL Server has a non-standard geometry type to represent the full globe when using the `geography` column type.</span></span> <span data-ttu-id="9ea8b-174">Offre anche un modo per rappresentare i poligoni in base al globo completo (senza anello esterno).</span><span class="sxs-lookup"><span data-stu-id="9ea8b-174">It also has a way to represent polygons based on the full globe (without an exterior ring).</span></span> <span data-ttu-id="9ea8b-175">Nessuno di questi è supportato da NTS.</span><span class="sxs-lookup"><span data-stu-id="9ea8b-175">Neither of these are supported by NTS.</span></span>

> [!WARNING]
> <span data-ttu-id="9ea8b-176">FullGlobe e i poligoni basati su di essi non sono supportati da NTS.</span><span class="sxs-lookup"><span data-stu-id="9ea8b-176">FullGlobe and polygons based on it aren't supported by NTS.</span></span>

## <a name="sqlite"></a><span data-ttu-id="9ea8b-177">SQLite</span><span class="sxs-lookup"><span data-stu-id="9ea8b-177">SQLite</span></span>

<span data-ttu-id="9ea8b-178">Di seguito sono riportate alcune informazioni aggiuntive per quelle che usano SQLite.</span><span class="sxs-lookup"><span data-stu-id="9ea8b-178">Here is some additional information for those using SQLite.</span></span>

### <a name="installing-spatialite"></a><span data-ttu-id="9ea8b-179">Installazione di SpatiaLite</span><span class="sxs-lookup"><span data-stu-id="9ea8b-179">Installing SpatiaLite</span></span>

<span data-ttu-id="9ea8b-180">In Windows la libreria mod_spatialite nativa viene distribuita come dipendenza del pacchetto NuGet.</span><span class="sxs-lookup"><span data-stu-id="9ea8b-180">On Windows, the native mod_spatialite library is distributed as a NuGet package dependency.</span></span> <span data-ttu-id="9ea8b-181">Altre piattaforme devono essere installate separatamente.</span><span class="sxs-lookup"><span data-stu-id="9ea8b-181">Other platforms need to install it separately.</span></span> <span data-ttu-id="9ea8b-182">Questa operazione viene in genere eseguita utilizzando una gestione pacchetti software.</span><span class="sxs-lookup"><span data-stu-id="9ea8b-182">This is typically done using a software package manager.</span></span> <span data-ttu-id="9ea8b-183">Ad esempio, è possibile usare APT in Ubuntu e homebrew in MacOS.</span><span class="sxs-lookup"><span data-stu-id="9ea8b-183">For example, you can use APT on Ubuntu and Homebrew on MacOS.</span></span>

``` sh
# Ubuntu
apt-get install libsqlite3-mod-spatialite

# macOS
brew install libspatialite
```

### <a name="configuring-srid"></a><span data-ttu-id="9ea8b-184">Configurazione di SRID</span><span class="sxs-lookup"><span data-stu-id="9ea8b-184">Configuring SRID</span></span>

<span data-ttu-id="9ea8b-185">In SpatiaLite le colonne devono specificare un identificatore SRID per colonna.</span><span class="sxs-lookup"><span data-stu-id="9ea8b-185">In SpatiaLite, columns need to specify an SRID per column.</span></span> <span data-ttu-id="9ea8b-186">L'identificatore SRID predefinito `0`è.</span><span class="sxs-lookup"><span data-stu-id="9ea8b-186">The default SRID is `0`.</span></span> <span data-ttu-id="9ea8b-187">Specificare un identificatore SRID diverso usando il metodo ForSqliteHasSrid.</span><span class="sxs-lookup"><span data-stu-id="9ea8b-187">Specify a different SRID using the ForSqliteHasSrid method.</span></span>

``` csharp
modelBuilder.Entity<City>().Property(c => c.Location)
    .ForSqliteHasSrid(4326);
```

### <a name="dimension"></a><span data-ttu-id="9ea8b-188">Dimensione</span><span class="sxs-lookup"><span data-stu-id="9ea8b-188">Dimension</span></span>

<span data-ttu-id="9ea8b-189">Analogamente a SRID, viene specificata anche una dimensione (o ordinata) di una colonna come parte della colonna.</span><span class="sxs-lookup"><span data-stu-id="9ea8b-189">Similar to SRID, a column's dimension (or ordinates) is also specified as part of the column.</span></span> <span data-ttu-id="9ea8b-190">Le ordinate predefinite sono X e Y. abilitare le coordinate aggiuntive (Z e M) usando il metodo ForSqliteHasDimension.</span><span class="sxs-lookup"><span data-stu-id="9ea8b-190">The default ordinates are X and Y. Enable additional ordinates (Z and M) using the ForSqliteHasDimension method.</span></span>

``` csharp
modelBuilder.Entity<City>().Property(c => c.Location)
    .ForSqliteHasDimension(Ordinates.XYZ);
```

## <a name="translated-operations"></a><span data-ttu-id="9ea8b-191">Operazioni convertite</span><span class="sxs-lookup"><span data-stu-id="9ea8b-191">Translated Operations</span></span>

<span data-ttu-id="9ea8b-192">Questa tabella mostra i membri NTS convertiti in SQL da ogni provider di EF Core.</span><span class="sxs-lookup"><span data-stu-id="9ea8b-192">This table shows which NTS members are translated into SQL by each EF Core provider.</span></span>

<span data-ttu-id="9ea8b-193">NetTopologySuite</span><span class="sxs-lookup"><span data-stu-id="9ea8b-193">NetTopologySuite</span></span> | <span data-ttu-id="9ea8b-194">SQL Server (geometria)</span><span class="sxs-lookup"><span data-stu-id="9ea8b-194">SQL Server (geometry)</span></span> | <span data-ttu-id="9ea8b-195">SQL Server (geography)</span><span class="sxs-lookup"><span data-stu-id="9ea8b-195">SQL Server (geography)</span></span> | <span data-ttu-id="9ea8b-196">SQLite</span><span class="sxs-lookup"><span data-stu-id="9ea8b-196">SQLite</span></span> | <span data-ttu-id="9ea8b-197">Npgsql</span><span class="sxs-lookup"><span data-stu-id="9ea8b-197">Npgsql</span></span>
--- |:---:|:---:|:---:|:---:
<span data-ttu-id="9ea8b-198">Geometry. area</span><span class="sxs-lookup"><span data-stu-id="9ea8b-198">Geometry.Area</span></span> | <span data-ttu-id="9ea8b-199">✔</span><span class="sxs-lookup"><span data-stu-id="9ea8b-199">✔</span></span> | <span data-ttu-id="9ea8b-200">✔</span><span class="sxs-lookup"><span data-stu-id="9ea8b-200">✔</span></span> | <span data-ttu-id="9ea8b-201">✔</span><span class="sxs-lookup"><span data-stu-id="9ea8b-201">✔</span></span> | <span data-ttu-id="9ea8b-202">✔</span><span class="sxs-lookup"><span data-stu-id="9ea8b-202">✔</span></span>
<span data-ttu-id="9ea8b-203">Geometry. AsBinary ()</span><span class="sxs-lookup"><span data-stu-id="9ea8b-203">Geometry.AsBinary()</span></span> | <span data-ttu-id="9ea8b-204">✔</span><span class="sxs-lookup"><span data-stu-id="9ea8b-204">✔</span></span> | <span data-ttu-id="9ea8b-205">✔</span><span class="sxs-lookup"><span data-stu-id="9ea8b-205">✔</span></span> | <span data-ttu-id="9ea8b-206">✔</span><span class="sxs-lookup"><span data-stu-id="9ea8b-206">✔</span></span> | <span data-ttu-id="9ea8b-207">✔</span><span class="sxs-lookup"><span data-stu-id="9ea8b-207">✔</span></span>
<span data-ttu-id="9ea8b-208">Geometry. AsText ()</span><span class="sxs-lookup"><span data-stu-id="9ea8b-208">Geometry.AsText()</span></span> | <span data-ttu-id="9ea8b-209">✔</span><span class="sxs-lookup"><span data-stu-id="9ea8b-209">✔</span></span> | <span data-ttu-id="9ea8b-210">✔</span><span class="sxs-lookup"><span data-stu-id="9ea8b-210">✔</span></span> | <span data-ttu-id="9ea8b-211">✔</span><span class="sxs-lookup"><span data-stu-id="9ea8b-211">✔</span></span> | <span data-ttu-id="9ea8b-212">✔</span><span class="sxs-lookup"><span data-stu-id="9ea8b-212">✔</span></span>
<span data-ttu-id="9ea8b-213">Geometry. Boundary</span><span class="sxs-lookup"><span data-stu-id="9ea8b-213">Geometry.Boundary</span></span> | <span data-ttu-id="9ea8b-214">✔</span><span class="sxs-lookup"><span data-stu-id="9ea8b-214">✔</span></span> | | <span data-ttu-id="9ea8b-215">✔</span><span class="sxs-lookup"><span data-stu-id="9ea8b-215">✔</span></span> | <span data-ttu-id="9ea8b-216">✔</span><span class="sxs-lookup"><span data-stu-id="9ea8b-216">✔</span></span>
<span data-ttu-id="9ea8b-217">Geometry. buffer (Double)</span><span class="sxs-lookup"><span data-stu-id="9ea8b-217">Geometry.Buffer(double)</span></span> | <span data-ttu-id="9ea8b-218">✔</span><span class="sxs-lookup"><span data-stu-id="9ea8b-218">✔</span></span> | <span data-ttu-id="9ea8b-219">✔</span><span class="sxs-lookup"><span data-stu-id="9ea8b-219">✔</span></span> | <span data-ttu-id="9ea8b-220">✔</span><span class="sxs-lookup"><span data-stu-id="9ea8b-220">✔</span></span> | <span data-ttu-id="9ea8b-221">✔</span><span class="sxs-lookup"><span data-stu-id="9ea8b-221">✔</span></span>
<span data-ttu-id="9ea8b-222">Geometry. buffer (Double, int)</span><span class="sxs-lookup"><span data-stu-id="9ea8b-222">Geometry.Buffer(double, int)</span></span> | | | <span data-ttu-id="9ea8b-223">✔</span><span class="sxs-lookup"><span data-stu-id="9ea8b-223">✔</span></span>
<span data-ttu-id="9ea8b-224">Geometry. baricentro</span><span class="sxs-lookup"><span data-stu-id="9ea8b-224">Geometry.Centroid</span></span> | <span data-ttu-id="9ea8b-225">✔</span><span class="sxs-lookup"><span data-stu-id="9ea8b-225">✔</span></span> | | <span data-ttu-id="9ea8b-226">✔</span><span class="sxs-lookup"><span data-stu-id="9ea8b-226">✔</span></span> | <span data-ttu-id="9ea8b-227">✔</span><span class="sxs-lookup"><span data-stu-id="9ea8b-227">✔</span></span>
<span data-ttu-id="9ea8b-228">Geometry. Contains (Geometry)</span><span class="sxs-lookup"><span data-stu-id="9ea8b-228">Geometry.Contains(Geometry)</span></span> | <span data-ttu-id="9ea8b-229">✔</span><span class="sxs-lookup"><span data-stu-id="9ea8b-229">✔</span></span> | <span data-ttu-id="9ea8b-230">✔</span><span class="sxs-lookup"><span data-stu-id="9ea8b-230">✔</span></span> | <span data-ttu-id="9ea8b-231">✔</span><span class="sxs-lookup"><span data-stu-id="9ea8b-231">✔</span></span> | <span data-ttu-id="9ea8b-232">✔</span><span class="sxs-lookup"><span data-stu-id="9ea8b-232">✔</span></span>
<span data-ttu-id="9ea8b-233">Geometry. ConvexHull ()</span><span class="sxs-lookup"><span data-stu-id="9ea8b-233">Geometry.ConvexHull()</span></span> | <span data-ttu-id="9ea8b-234">✔</span><span class="sxs-lookup"><span data-stu-id="9ea8b-234">✔</span></span> | <span data-ttu-id="9ea8b-235">✔</span><span class="sxs-lookup"><span data-stu-id="9ea8b-235">✔</span></span> | <span data-ttu-id="9ea8b-236">✔</span><span class="sxs-lookup"><span data-stu-id="9ea8b-236">✔</span></span> | <span data-ttu-id="9ea8b-237">✔</span><span class="sxs-lookup"><span data-stu-id="9ea8b-237">✔</span></span>
<span data-ttu-id="9ea8b-238">Geometry. CoveredBy (Geometry)</span><span class="sxs-lookup"><span data-stu-id="9ea8b-238">Geometry.CoveredBy(Geometry)</span></span> | | | <span data-ttu-id="9ea8b-239">✔</span><span class="sxs-lookup"><span data-stu-id="9ea8b-239">✔</span></span> | <span data-ttu-id="9ea8b-240">✔</span><span class="sxs-lookup"><span data-stu-id="9ea8b-240">✔</span></span>
<span data-ttu-id="9ea8b-241">Geometry. Covers (geometria)</span><span class="sxs-lookup"><span data-stu-id="9ea8b-241">Geometry.Covers(Geometry)</span></span> | | | <span data-ttu-id="9ea8b-242">✔</span><span class="sxs-lookup"><span data-stu-id="9ea8b-242">✔</span></span> | <span data-ttu-id="9ea8b-243">✔</span><span class="sxs-lookup"><span data-stu-id="9ea8b-243">✔</span></span>
<span data-ttu-id="9ea8b-244">Geometry. Crosses (geometria)</span><span class="sxs-lookup"><span data-stu-id="9ea8b-244">Geometry.Crosses(Geometry)</span></span> | <span data-ttu-id="9ea8b-245">✔</span><span class="sxs-lookup"><span data-stu-id="9ea8b-245">✔</span></span> | | <span data-ttu-id="9ea8b-246">✔</span><span class="sxs-lookup"><span data-stu-id="9ea8b-246">✔</span></span> | <span data-ttu-id="9ea8b-247">✔</span><span class="sxs-lookup"><span data-stu-id="9ea8b-247">✔</span></span>
<span data-ttu-id="9ea8b-248">Geometry. Difference (Geometry)</span><span class="sxs-lookup"><span data-stu-id="9ea8b-248">Geometry.Difference(Geometry)</span></span> | <span data-ttu-id="9ea8b-249">✔</span><span class="sxs-lookup"><span data-stu-id="9ea8b-249">✔</span></span> | <span data-ttu-id="9ea8b-250">✔</span><span class="sxs-lookup"><span data-stu-id="9ea8b-250">✔</span></span> | <span data-ttu-id="9ea8b-251">✔</span><span class="sxs-lookup"><span data-stu-id="9ea8b-251">✔</span></span> | <span data-ttu-id="9ea8b-252">✔</span><span class="sxs-lookup"><span data-stu-id="9ea8b-252">✔</span></span>
<span data-ttu-id="9ea8b-253">Geometry. Dimension</span><span class="sxs-lookup"><span data-stu-id="9ea8b-253">Geometry.Dimension</span></span> | <span data-ttu-id="9ea8b-254">✔</span><span class="sxs-lookup"><span data-stu-id="9ea8b-254">✔</span></span> | <span data-ttu-id="9ea8b-255">✔</span><span class="sxs-lookup"><span data-stu-id="9ea8b-255">✔</span></span> | <span data-ttu-id="9ea8b-256">✔</span><span class="sxs-lookup"><span data-stu-id="9ea8b-256">✔</span></span> | <span data-ttu-id="9ea8b-257">✔</span><span class="sxs-lookup"><span data-stu-id="9ea8b-257">✔</span></span>
<span data-ttu-id="9ea8b-258">Geometry. unjoint (Geometry)</span><span class="sxs-lookup"><span data-stu-id="9ea8b-258">Geometry.Disjoint(Geometry)</span></span> | <span data-ttu-id="9ea8b-259">✔</span><span class="sxs-lookup"><span data-stu-id="9ea8b-259">✔</span></span> | <span data-ttu-id="9ea8b-260">✔</span><span class="sxs-lookup"><span data-stu-id="9ea8b-260">✔</span></span> | <span data-ttu-id="9ea8b-261">✔</span><span class="sxs-lookup"><span data-stu-id="9ea8b-261">✔</span></span> | <span data-ttu-id="9ea8b-262">✔</span><span class="sxs-lookup"><span data-stu-id="9ea8b-262">✔</span></span>
<span data-ttu-id="9ea8b-263">Geometry. distance (Geometry)</span><span class="sxs-lookup"><span data-stu-id="9ea8b-263">Geometry.Distance(Geometry)</span></span> | <span data-ttu-id="9ea8b-264">✔</span><span class="sxs-lookup"><span data-stu-id="9ea8b-264">✔</span></span> | <span data-ttu-id="9ea8b-265">✔</span><span class="sxs-lookup"><span data-stu-id="9ea8b-265">✔</span></span> | <span data-ttu-id="9ea8b-266">✔</span><span class="sxs-lookup"><span data-stu-id="9ea8b-266">✔</span></span> | <span data-ttu-id="9ea8b-267">✔</span><span class="sxs-lookup"><span data-stu-id="9ea8b-267">✔</span></span>
<span data-ttu-id="9ea8b-268">Geometry. Envelope</span><span class="sxs-lookup"><span data-stu-id="9ea8b-268">Geometry.Envelope</span></span> | <span data-ttu-id="9ea8b-269">✔</span><span class="sxs-lookup"><span data-stu-id="9ea8b-269">✔</span></span> | | <span data-ttu-id="9ea8b-270">✔</span><span class="sxs-lookup"><span data-stu-id="9ea8b-270">✔</span></span> | <span data-ttu-id="9ea8b-271">✔</span><span class="sxs-lookup"><span data-stu-id="9ea8b-271">✔</span></span>
<span data-ttu-id="9ea8b-272">Geometry. EqualsExact (Geometry)</span><span class="sxs-lookup"><span data-stu-id="9ea8b-272">Geometry.EqualsExact(Geometry)</span></span> | | | | <span data-ttu-id="9ea8b-273">✔</span><span class="sxs-lookup"><span data-stu-id="9ea8b-273">✔</span></span>
<span data-ttu-id="9ea8b-274">Geometry. EqualsTopologically (Geometry)</span><span class="sxs-lookup"><span data-stu-id="9ea8b-274">Geometry.EqualsTopologically(Geometry)</span></span> | <span data-ttu-id="9ea8b-275">✔</span><span class="sxs-lookup"><span data-stu-id="9ea8b-275">✔</span></span> | <span data-ttu-id="9ea8b-276">✔</span><span class="sxs-lookup"><span data-stu-id="9ea8b-276">✔</span></span> | <span data-ttu-id="9ea8b-277">✔</span><span class="sxs-lookup"><span data-stu-id="9ea8b-277">✔</span></span> | <span data-ttu-id="9ea8b-278">✔</span><span class="sxs-lookup"><span data-stu-id="9ea8b-278">✔</span></span>
<span data-ttu-id="9ea8b-279">Geometry. GeometryType</span><span class="sxs-lookup"><span data-stu-id="9ea8b-279">Geometry.GeometryType</span></span> | <span data-ttu-id="9ea8b-280">✔</span><span class="sxs-lookup"><span data-stu-id="9ea8b-280">✔</span></span> | <span data-ttu-id="9ea8b-281">✔</span><span class="sxs-lookup"><span data-stu-id="9ea8b-281">✔</span></span> | <span data-ttu-id="9ea8b-282">✔</span><span class="sxs-lookup"><span data-stu-id="9ea8b-282">✔</span></span> | <span data-ttu-id="9ea8b-283">✔</span><span class="sxs-lookup"><span data-stu-id="9ea8b-283">✔</span></span>
<span data-ttu-id="9ea8b-284">Geometry. getgeometryn (int)</span><span class="sxs-lookup"><span data-stu-id="9ea8b-284">Geometry.GetGeometryN(int)</span></span> | <span data-ttu-id="9ea8b-285">✔</span><span class="sxs-lookup"><span data-stu-id="9ea8b-285">✔</span></span> | | <span data-ttu-id="9ea8b-286">✔</span><span class="sxs-lookup"><span data-stu-id="9ea8b-286">✔</span></span> | <span data-ttu-id="9ea8b-287">✔</span><span class="sxs-lookup"><span data-stu-id="9ea8b-287">✔</span></span>
<span data-ttu-id="9ea8b-288">Geometry. InteriorPoint</span><span class="sxs-lookup"><span data-stu-id="9ea8b-288">Geometry.InteriorPoint</span></span> | <span data-ttu-id="9ea8b-289">✔</span><span class="sxs-lookup"><span data-stu-id="9ea8b-289">✔</span></span> | | <span data-ttu-id="9ea8b-290">✔</span><span class="sxs-lookup"><span data-stu-id="9ea8b-290">✔</span></span>
<span data-ttu-id="9ea8b-291">Geometry. intersezione (Geometry)</span><span class="sxs-lookup"><span data-stu-id="9ea8b-291">Geometry.Intersection(Geometry)</span></span> | <span data-ttu-id="9ea8b-292">✔</span><span class="sxs-lookup"><span data-stu-id="9ea8b-292">✔</span></span> | <span data-ttu-id="9ea8b-293">✔</span><span class="sxs-lookup"><span data-stu-id="9ea8b-293">✔</span></span> | <span data-ttu-id="9ea8b-294">✔</span><span class="sxs-lookup"><span data-stu-id="9ea8b-294">✔</span></span> | <span data-ttu-id="9ea8b-295">✔</span><span class="sxs-lookup"><span data-stu-id="9ea8b-295">✔</span></span>
<span data-ttu-id="9ea8b-296">Geometry. Intersects (Geometry)</span><span class="sxs-lookup"><span data-stu-id="9ea8b-296">Geometry.Intersects(Geometry)</span></span> | <span data-ttu-id="9ea8b-297">✔</span><span class="sxs-lookup"><span data-stu-id="9ea8b-297">✔</span></span> | <span data-ttu-id="9ea8b-298">✔</span><span class="sxs-lookup"><span data-stu-id="9ea8b-298">✔</span></span> | <span data-ttu-id="9ea8b-299">✔</span><span class="sxs-lookup"><span data-stu-id="9ea8b-299">✔</span></span> | <span data-ttu-id="9ea8b-300">✔</span><span class="sxs-lookup"><span data-stu-id="9ea8b-300">✔</span></span>
<span data-ttu-id="9ea8b-301">Geometry. IsEmpty</span><span class="sxs-lookup"><span data-stu-id="9ea8b-301">Geometry.IsEmpty</span></span> | <span data-ttu-id="9ea8b-302">✔</span><span class="sxs-lookup"><span data-stu-id="9ea8b-302">✔</span></span> | <span data-ttu-id="9ea8b-303">✔</span><span class="sxs-lookup"><span data-stu-id="9ea8b-303">✔</span></span> | <span data-ttu-id="9ea8b-304">✔</span><span class="sxs-lookup"><span data-stu-id="9ea8b-304">✔</span></span> | <span data-ttu-id="9ea8b-305">✔</span><span class="sxs-lookup"><span data-stu-id="9ea8b-305">✔</span></span>
<span data-ttu-id="9ea8b-306">Geometry. Simple</span><span class="sxs-lookup"><span data-stu-id="9ea8b-306">Geometry.IsSimple</span></span> | <span data-ttu-id="9ea8b-307">✔</span><span class="sxs-lookup"><span data-stu-id="9ea8b-307">✔</span></span> | | <span data-ttu-id="9ea8b-308">✔</span><span class="sxs-lookup"><span data-stu-id="9ea8b-308">✔</span></span> | <span data-ttu-id="9ea8b-309">✔</span><span class="sxs-lookup"><span data-stu-id="9ea8b-309">✔</span></span>
<span data-ttu-id="9ea8b-310">Geometry. IsValid</span><span class="sxs-lookup"><span data-stu-id="9ea8b-310">Geometry.IsValid</span></span> | <span data-ttu-id="9ea8b-311">✔</span><span class="sxs-lookup"><span data-stu-id="9ea8b-311">✔</span></span> | <span data-ttu-id="9ea8b-312">✔</span><span class="sxs-lookup"><span data-stu-id="9ea8b-312">✔</span></span> | <span data-ttu-id="9ea8b-313">✔</span><span class="sxs-lookup"><span data-stu-id="9ea8b-313">✔</span></span> | <span data-ttu-id="9ea8b-314">✔</span><span class="sxs-lookup"><span data-stu-id="9ea8b-314">✔</span></span>
<span data-ttu-id="9ea8b-315">Geometry. IsWithinDistance (Geometry, Double)</span><span class="sxs-lookup"><span data-stu-id="9ea8b-315">Geometry.IsWithinDistance(Geometry, double)</span></span> | <span data-ttu-id="9ea8b-316">✔</span><span class="sxs-lookup"><span data-stu-id="9ea8b-316">✔</span></span> | | <span data-ttu-id="9ea8b-317">✔</span><span class="sxs-lookup"><span data-stu-id="9ea8b-317">✔</span></span>
<span data-ttu-id="9ea8b-318">Geometry. length</span><span class="sxs-lookup"><span data-stu-id="9ea8b-318">Geometry.Length</span></span> | <span data-ttu-id="9ea8b-319">✔</span><span class="sxs-lookup"><span data-stu-id="9ea8b-319">✔</span></span> | <span data-ttu-id="9ea8b-320">✔</span><span class="sxs-lookup"><span data-stu-id="9ea8b-320">✔</span></span> | <span data-ttu-id="9ea8b-321">✔</span><span class="sxs-lookup"><span data-stu-id="9ea8b-321">✔</span></span> | <span data-ttu-id="9ea8b-322">✔</span><span class="sxs-lookup"><span data-stu-id="9ea8b-322">✔</span></span>
<span data-ttu-id="9ea8b-323">Geometry. NumGeometries</span><span class="sxs-lookup"><span data-stu-id="9ea8b-323">Geometry.NumGeometries</span></span> | <span data-ttu-id="9ea8b-324">✔</span><span class="sxs-lookup"><span data-stu-id="9ea8b-324">✔</span></span> | <span data-ttu-id="9ea8b-325">✔</span><span class="sxs-lookup"><span data-stu-id="9ea8b-325">✔</span></span> | <span data-ttu-id="9ea8b-326">✔</span><span class="sxs-lookup"><span data-stu-id="9ea8b-326">✔</span></span> | <span data-ttu-id="9ea8b-327">✔</span><span class="sxs-lookup"><span data-stu-id="9ea8b-327">✔</span></span>
<span data-ttu-id="9ea8b-328">Geometry. NumPoints</span><span class="sxs-lookup"><span data-stu-id="9ea8b-328">Geometry.NumPoints</span></span> | <span data-ttu-id="9ea8b-329">✔</span><span class="sxs-lookup"><span data-stu-id="9ea8b-329">✔</span></span> | <span data-ttu-id="9ea8b-330">✔</span><span class="sxs-lookup"><span data-stu-id="9ea8b-330">✔</span></span> | <span data-ttu-id="9ea8b-331">✔</span><span class="sxs-lookup"><span data-stu-id="9ea8b-331">✔</span></span> | <span data-ttu-id="9ea8b-332">✔</span><span class="sxs-lookup"><span data-stu-id="9ea8b-332">✔</span></span>
<span data-ttu-id="9ea8b-333">Geometry. OgcGeometryType</span><span class="sxs-lookup"><span data-stu-id="9ea8b-333">Geometry.OgcGeometryType</span></span> | <span data-ttu-id="9ea8b-334">✔</span><span class="sxs-lookup"><span data-stu-id="9ea8b-334">✔</span></span> | <span data-ttu-id="9ea8b-335">✔</span><span class="sxs-lookup"><span data-stu-id="9ea8b-335">✔</span></span> | <span data-ttu-id="9ea8b-336">✔</span><span class="sxs-lookup"><span data-stu-id="9ea8b-336">✔</span></span>
<span data-ttu-id="9ea8b-337">Geometry. sovrapposizioni (Geometry)</span><span class="sxs-lookup"><span data-stu-id="9ea8b-337">Geometry.Overlaps(Geometry)</span></span> | <span data-ttu-id="9ea8b-338">✔</span><span class="sxs-lookup"><span data-stu-id="9ea8b-338">✔</span></span> | <span data-ttu-id="9ea8b-339">✔</span><span class="sxs-lookup"><span data-stu-id="9ea8b-339">✔</span></span> | <span data-ttu-id="9ea8b-340">✔</span><span class="sxs-lookup"><span data-stu-id="9ea8b-340">✔</span></span> | <span data-ttu-id="9ea8b-341">✔</span><span class="sxs-lookup"><span data-stu-id="9ea8b-341">✔</span></span>
<span data-ttu-id="9ea8b-342">Geometry. PointOnSurface</span><span class="sxs-lookup"><span data-stu-id="9ea8b-342">Geometry.PointOnSurface</span></span> | <span data-ttu-id="9ea8b-343">✔</span><span class="sxs-lookup"><span data-stu-id="9ea8b-343">✔</span></span> | | <span data-ttu-id="9ea8b-344">✔</span><span class="sxs-lookup"><span data-stu-id="9ea8b-344">✔</span></span> | <span data-ttu-id="9ea8b-345">✔</span><span class="sxs-lookup"><span data-stu-id="9ea8b-345">✔</span></span>
<span data-ttu-id="9ea8b-346">Geometry. correlate (Geometry, String)</span><span class="sxs-lookup"><span data-stu-id="9ea8b-346">Geometry.Relate(Geometry, string)</span></span> | <span data-ttu-id="9ea8b-347">✔</span><span class="sxs-lookup"><span data-stu-id="9ea8b-347">✔</span></span> | | <span data-ttu-id="9ea8b-348">✔</span><span class="sxs-lookup"><span data-stu-id="9ea8b-348">✔</span></span> | <span data-ttu-id="9ea8b-349">✔</span><span class="sxs-lookup"><span data-stu-id="9ea8b-349">✔</span></span>
<span data-ttu-id="9ea8b-350">Geometry. Reverse ()</span><span class="sxs-lookup"><span data-stu-id="9ea8b-350">Geometry.Reverse()</span></span> | | | <span data-ttu-id="9ea8b-351">✔</span><span class="sxs-lookup"><span data-stu-id="9ea8b-351">✔</span></span> | <span data-ttu-id="9ea8b-352">✔</span><span class="sxs-lookup"><span data-stu-id="9ea8b-352">✔</span></span>
<span data-ttu-id="9ea8b-353">Geometry. SRID</span><span class="sxs-lookup"><span data-stu-id="9ea8b-353">Geometry.SRID</span></span> | <span data-ttu-id="9ea8b-354">✔</span><span class="sxs-lookup"><span data-stu-id="9ea8b-354">✔</span></span> | <span data-ttu-id="9ea8b-355">✔</span><span class="sxs-lookup"><span data-stu-id="9ea8b-355">✔</span></span> | <span data-ttu-id="9ea8b-356">✔</span><span class="sxs-lookup"><span data-stu-id="9ea8b-356">✔</span></span> | <span data-ttu-id="9ea8b-357">✔</span><span class="sxs-lookup"><span data-stu-id="9ea8b-357">✔</span></span>
<span data-ttu-id="9ea8b-358">Geometry. SymmetricDifference (Geometry)</span><span class="sxs-lookup"><span data-stu-id="9ea8b-358">Geometry.SymmetricDifference(Geometry)</span></span> | <span data-ttu-id="9ea8b-359">✔</span><span class="sxs-lookup"><span data-stu-id="9ea8b-359">✔</span></span> | <span data-ttu-id="9ea8b-360">✔</span><span class="sxs-lookup"><span data-stu-id="9ea8b-360">✔</span></span> | <span data-ttu-id="9ea8b-361">✔</span><span class="sxs-lookup"><span data-stu-id="9ea8b-361">✔</span></span> | <span data-ttu-id="9ea8b-362">✔</span><span class="sxs-lookup"><span data-stu-id="9ea8b-362">✔</span></span>
<span data-ttu-id="9ea8b-363">Geometry. ToBinary ()</span><span class="sxs-lookup"><span data-stu-id="9ea8b-363">Geometry.ToBinary()</span></span> | <span data-ttu-id="9ea8b-364">✔</span><span class="sxs-lookup"><span data-stu-id="9ea8b-364">✔</span></span> | <span data-ttu-id="9ea8b-365">✔</span><span class="sxs-lookup"><span data-stu-id="9ea8b-365">✔</span></span> | <span data-ttu-id="9ea8b-366">✔</span><span class="sxs-lookup"><span data-stu-id="9ea8b-366">✔</span></span> | <span data-ttu-id="9ea8b-367">✔</span><span class="sxs-lookup"><span data-stu-id="9ea8b-367">✔</span></span>
<span data-ttu-id="9ea8b-368">Geometry. ToText ()</span><span class="sxs-lookup"><span data-stu-id="9ea8b-368">Geometry.ToText()</span></span> | <span data-ttu-id="9ea8b-369">✔</span><span class="sxs-lookup"><span data-stu-id="9ea8b-369">✔</span></span> | <span data-ttu-id="9ea8b-370">✔</span><span class="sxs-lookup"><span data-stu-id="9ea8b-370">✔</span></span> | <span data-ttu-id="9ea8b-371">✔</span><span class="sxs-lookup"><span data-stu-id="9ea8b-371">✔</span></span> | <span data-ttu-id="9ea8b-372">✔</span><span class="sxs-lookup"><span data-stu-id="9ea8b-372">✔</span></span>
<span data-ttu-id="9ea8b-373">Geometry. touchs (Geometry)</span><span class="sxs-lookup"><span data-stu-id="9ea8b-373">Geometry.Touches(Geometry)</span></span> | <span data-ttu-id="9ea8b-374">✔</span><span class="sxs-lookup"><span data-stu-id="9ea8b-374">✔</span></span> | | <span data-ttu-id="9ea8b-375">✔</span><span class="sxs-lookup"><span data-stu-id="9ea8b-375">✔</span></span> | <span data-ttu-id="9ea8b-376">✔</span><span class="sxs-lookup"><span data-stu-id="9ea8b-376">✔</span></span>
<span data-ttu-id="9ea8b-377">Geometry. Union ()</span><span class="sxs-lookup"><span data-stu-id="9ea8b-377">Geometry.Union()</span></span> | | | <span data-ttu-id="9ea8b-378">✔</span><span class="sxs-lookup"><span data-stu-id="9ea8b-378">✔</span></span>
<span data-ttu-id="9ea8b-379">Geometry. Union (Geometry)</span><span class="sxs-lookup"><span data-stu-id="9ea8b-379">Geometry.Union(Geometry)</span></span> | <span data-ttu-id="9ea8b-380">✔</span><span class="sxs-lookup"><span data-stu-id="9ea8b-380">✔</span></span> | <span data-ttu-id="9ea8b-381">✔</span><span class="sxs-lookup"><span data-stu-id="9ea8b-381">✔</span></span> | <span data-ttu-id="9ea8b-382">✔</span><span class="sxs-lookup"><span data-stu-id="9ea8b-382">✔</span></span> | <span data-ttu-id="9ea8b-383">✔</span><span class="sxs-lookup"><span data-stu-id="9ea8b-383">✔</span></span>
<span data-ttu-id="9ea8b-384">Geometry. Within (Geometry)</span><span class="sxs-lookup"><span data-stu-id="9ea8b-384">Geometry.Within(Geometry)</span></span> | <span data-ttu-id="9ea8b-385">✔</span><span class="sxs-lookup"><span data-stu-id="9ea8b-385">✔</span></span> | <span data-ttu-id="9ea8b-386">✔</span><span class="sxs-lookup"><span data-stu-id="9ea8b-386">✔</span></span> | <span data-ttu-id="9ea8b-387">✔</span><span class="sxs-lookup"><span data-stu-id="9ea8b-387">✔</span></span> | <span data-ttu-id="9ea8b-388">✔</span><span class="sxs-lookup"><span data-stu-id="9ea8b-388">✔</span></span>
<span data-ttu-id="9ea8b-389">GeometryCollection. Count</span><span class="sxs-lookup"><span data-stu-id="9ea8b-389">GeometryCollection.Count</span></span> | <span data-ttu-id="9ea8b-390">✔</span><span class="sxs-lookup"><span data-stu-id="9ea8b-390">✔</span></span> | <span data-ttu-id="9ea8b-391">✔</span><span class="sxs-lookup"><span data-stu-id="9ea8b-391">✔</span></span> | <span data-ttu-id="9ea8b-392">✔</span><span class="sxs-lookup"><span data-stu-id="9ea8b-392">✔</span></span> | <span data-ttu-id="9ea8b-393">✔</span><span class="sxs-lookup"><span data-stu-id="9ea8b-393">✔</span></span>
<span data-ttu-id="9ea8b-394">GeometryCollection [int]</span><span class="sxs-lookup"><span data-stu-id="9ea8b-394">GeometryCollection[int]</span></span> | <span data-ttu-id="9ea8b-395">✔</span><span class="sxs-lookup"><span data-stu-id="9ea8b-395">✔</span></span> | <span data-ttu-id="9ea8b-396">✔</span><span class="sxs-lookup"><span data-stu-id="9ea8b-396">✔</span></span> | <span data-ttu-id="9ea8b-397">✔</span><span class="sxs-lookup"><span data-stu-id="9ea8b-397">✔</span></span> | <span data-ttu-id="9ea8b-398">✔</span><span class="sxs-lookup"><span data-stu-id="9ea8b-398">✔</span></span>
<span data-ttu-id="9ea8b-399">LineString. Count</span><span class="sxs-lookup"><span data-stu-id="9ea8b-399">LineString.Count</span></span> | <span data-ttu-id="9ea8b-400">✔</span><span class="sxs-lookup"><span data-stu-id="9ea8b-400">✔</span></span> | <span data-ttu-id="9ea8b-401">✔</span><span class="sxs-lookup"><span data-stu-id="9ea8b-401">✔</span></span> | <span data-ttu-id="9ea8b-402">✔</span><span class="sxs-lookup"><span data-stu-id="9ea8b-402">✔</span></span> | <span data-ttu-id="9ea8b-403">✔</span><span class="sxs-lookup"><span data-stu-id="9ea8b-403">✔</span></span>
<span data-ttu-id="9ea8b-404">LineString. EndPoint</span><span class="sxs-lookup"><span data-stu-id="9ea8b-404">LineString.EndPoint</span></span> | <span data-ttu-id="9ea8b-405">✔</span><span class="sxs-lookup"><span data-stu-id="9ea8b-405">✔</span></span> | <span data-ttu-id="9ea8b-406">✔</span><span class="sxs-lookup"><span data-stu-id="9ea8b-406">✔</span></span> | <span data-ttu-id="9ea8b-407">✔</span><span class="sxs-lookup"><span data-stu-id="9ea8b-407">✔</span></span> | <span data-ttu-id="9ea8b-408">✔</span><span class="sxs-lookup"><span data-stu-id="9ea8b-408">✔</span></span>
<span data-ttu-id="9ea8b-409">LineString. getpointn (int)</span><span class="sxs-lookup"><span data-stu-id="9ea8b-409">LineString.GetPointN(int)</span></span> | <span data-ttu-id="9ea8b-410">✔</span><span class="sxs-lookup"><span data-stu-id="9ea8b-410">✔</span></span> | <span data-ttu-id="9ea8b-411">✔</span><span class="sxs-lookup"><span data-stu-id="9ea8b-411">✔</span></span> | <span data-ttu-id="9ea8b-412">✔</span><span class="sxs-lookup"><span data-stu-id="9ea8b-412">✔</span></span> | <span data-ttu-id="9ea8b-413">✔</span><span class="sxs-lookup"><span data-stu-id="9ea8b-413">✔</span></span>
<span data-ttu-id="9ea8b-414">LineString. di chiusura</span><span class="sxs-lookup"><span data-stu-id="9ea8b-414">LineString.IsClosed</span></span> | <span data-ttu-id="9ea8b-415">✔</span><span class="sxs-lookup"><span data-stu-id="9ea8b-415">✔</span></span> | <span data-ttu-id="9ea8b-416">✔</span><span class="sxs-lookup"><span data-stu-id="9ea8b-416">✔</span></span> | <span data-ttu-id="9ea8b-417">✔</span><span class="sxs-lookup"><span data-stu-id="9ea8b-417">✔</span></span> | <span data-ttu-id="9ea8b-418">✔</span><span class="sxs-lookup"><span data-stu-id="9ea8b-418">✔</span></span>
<span data-ttu-id="9ea8b-419">LineString. Ring</span><span class="sxs-lookup"><span data-stu-id="9ea8b-419">LineString.IsRing</span></span> | <span data-ttu-id="9ea8b-420">✔</span><span class="sxs-lookup"><span data-stu-id="9ea8b-420">✔</span></span> | | <span data-ttu-id="9ea8b-421">✔</span><span class="sxs-lookup"><span data-stu-id="9ea8b-421">✔</span></span> | <span data-ttu-id="9ea8b-422">✔</span><span class="sxs-lookup"><span data-stu-id="9ea8b-422">✔</span></span>
<span data-ttu-id="9ea8b-423">LineString. StartPoint</span><span class="sxs-lookup"><span data-stu-id="9ea8b-423">LineString.StartPoint</span></span> | <span data-ttu-id="9ea8b-424">✔</span><span class="sxs-lookup"><span data-stu-id="9ea8b-424">✔</span></span> | <span data-ttu-id="9ea8b-425">✔</span><span class="sxs-lookup"><span data-stu-id="9ea8b-425">✔</span></span> | <span data-ttu-id="9ea8b-426">✔</span><span class="sxs-lookup"><span data-stu-id="9ea8b-426">✔</span></span> | <span data-ttu-id="9ea8b-427">✔</span><span class="sxs-lookup"><span data-stu-id="9ea8b-427">✔</span></span>
<span data-ttu-id="9ea8b-428">MultiLineString. noclosed</span><span class="sxs-lookup"><span data-stu-id="9ea8b-428">MultiLineString.IsClosed</span></span> | <span data-ttu-id="9ea8b-429">✔</span><span class="sxs-lookup"><span data-stu-id="9ea8b-429">✔</span></span> | <span data-ttu-id="9ea8b-430">✔</span><span class="sxs-lookup"><span data-stu-id="9ea8b-430">✔</span></span> | <span data-ttu-id="9ea8b-431">✔</span><span class="sxs-lookup"><span data-stu-id="9ea8b-431">✔</span></span> | <span data-ttu-id="9ea8b-432">✔</span><span class="sxs-lookup"><span data-stu-id="9ea8b-432">✔</span></span>
<span data-ttu-id="9ea8b-433">Punto. M</span><span class="sxs-lookup"><span data-stu-id="9ea8b-433">Point.M</span></span> | <span data-ttu-id="9ea8b-434">✔</span><span class="sxs-lookup"><span data-stu-id="9ea8b-434">✔</span></span> | <span data-ttu-id="9ea8b-435">✔</span><span class="sxs-lookup"><span data-stu-id="9ea8b-435">✔</span></span> | <span data-ttu-id="9ea8b-436">✔</span><span class="sxs-lookup"><span data-stu-id="9ea8b-436">✔</span></span> | <span data-ttu-id="9ea8b-437">✔</span><span class="sxs-lookup"><span data-stu-id="9ea8b-437">✔</span></span>
<span data-ttu-id="9ea8b-438">Punto. X</span><span class="sxs-lookup"><span data-stu-id="9ea8b-438">Point.X</span></span> | <span data-ttu-id="9ea8b-439">✔</span><span class="sxs-lookup"><span data-stu-id="9ea8b-439">✔</span></span> | <span data-ttu-id="9ea8b-440">✔</span><span class="sxs-lookup"><span data-stu-id="9ea8b-440">✔</span></span> | <span data-ttu-id="9ea8b-441">✔</span><span class="sxs-lookup"><span data-stu-id="9ea8b-441">✔</span></span> | <span data-ttu-id="9ea8b-442">✔</span><span class="sxs-lookup"><span data-stu-id="9ea8b-442">✔</span></span>
<span data-ttu-id="9ea8b-443">Punto. Y</span><span class="sxs-lookup"><span data-stu-id="9ea8b-443">Point.Y</span></span> | <span data-ttu-id="9ea8b-444">✔</span><span class="sxs-lookup"><span data-stu-id="9ea8b-444">✔</span></span> | <span data-ttu-id="9ea8b-445">✔</span><span class="sxs-lookup"><span data-stu-id="9ea8b-445">✔</span></span> | <span data-ttu-id="9ea8b-446">✔</span><span class="sxs-lookup"><span data-stu-id="9ea8b-446">✔</span></span> | <span data-ttu-id="9ea8b-447">✔</span><span class="sxs-lookup"><span data-stu-id="9ea8b-447">✔</span></span>
<span data-ttu-id="9ea8b-448">Point. Z</span><span class="sxs-lookup"><span data-stu-id="9ea8b-448">Point.Z</span></span> | <span data-ttu-id="9ea8b-449">✔</span><span class="sxs-lookup"><span data-stu-id="9ea8b-449">✔</span></span> | <span data-ttu-id="9ea8b-450">✔</span><span class="sxs-lookup"><span data-stu-id="9ea8b-450">✔</span></span> | <span data-ttu-id="9ea8b-451">✔</span><span class="sxs-lookup"><span data-stu-id="9ea8b-451">✔</span></span> | <span data-ttu-id="9ea8b-452">✔</span><span class="sxs-lookup"><span data-stu-id="9ea8b-452">✔</span></span>
<span data-ttu-id="9ea8b-453">Poligono. ExteriorRing</span><span class="sxs-lookup"><span data-stu-id="9ea8b-453">Polygon.ExteriorRing</span></span> | <span data-ttu-id="9ea8b-454">✔</span><span class="sxs-lookup"><span data-stu-id="9ea8b-454">✔</span></span> | <span data-ttu-id="9ea8b-455">✔</span><span class="sxs-lookup"><span data-stu-id="9ea8b-455">✔</span></span> | <span data-ttu-id="9ea8b-456">✔</span><span class="sxs-lookup"><span data-stu-id="9ea8b-456">✔</span></span> | <span data-ttu-id="9ea8b-457">✔</span><span class="sxs-lookup"><span data-stu-id="9ea8b-457">✔</span></span>
<span data-ttu-id="9ea8b-458">Poligono. GetInteriorRingN (int)</span><span class="sxs-lookup"><span data-stu-id="9ea8b-458">Polygon.GetInteriorRingN(int)</span></span> | <span data-ttu-id="9ea8b-459">✔</span><span class="sxs-lookup"><span data-stu-id="9ea8b-459">✔</span></span> | <span data-ttu-id="9ea8b-460">✔</span><span class="sxs-lookup"><span data-stu-id="9ea8b-460">✔</span></span> | <span data-ttu-id="9ea8b-461">✔</span><span class="sxs-lookup"><span data-stu-id="9ea8b-461">✔</span></span> | <span data-ttu-id="9ea8b-462">✔</span><span class="sxs-lookup"><span data-stu-id="9ea8b-462">✔</span></span>
<span data-ttu-id="9ea8b-463">Poligono. NumInteriorRings</span><span class="sxs-lookup"><span data-stu-id="9ea8b-463">Polygon.NumInteriorRings</span></span> | <span data-ttu-id="9ea8b-464">✔</span><span class="sxs-lookup"><span data-stu-id="9ea8b-464">✔</span></span> | <span data-ttu-id="9ea8b-465">✔</span><span class="sxs-lookup"><span data-stu-id="9ea8b-465">✔</span></span> | <span data-ttu-id="9ea8b-466">✔</span><span class="sxs-lookup"><span data-stu-id="9ea8b-466">✔</span></span> | <span data-ttu-id="9ea8b-467">✔</span><span class="sxs-lookup"><span data-stu-id="9ea8b-467">✔</span></span>

## <a name="additional-resources"></a><span data-ttu-id="9ea8b-468">Risorse aggiuntive</span><span class="sxs-lookup"><span data-stu-id="9ea8b-468">Additional resources</span></span>

* [<span data-ttu-id="9ea8b-469">Dati spaziali in SQL Server</span><span class="sxs-lookup"><span data-stu-id="9ea8b-469">Spatial Data in SQL Server</span></span>](https://docs.microsoft.com/sql/relational-databases/spatial/spatial-data-sql-server)
* [<span data-ttu-id="9ea8b-470">Home page di SpatiaLite</span><span class="sxs-lookup"><span data-stu-id="9ea8b-470">SpatiaLite Homepage</span></span>](https://www.gaia-gis.it/fossil/libspatialite)
* [<span data-ttu-id="9ea8b-471">Documentazione spaziale di npgsql</span><span class="sxs-lookup"><span data-stu-id="9ea8b-471">Npgsql Spatial Documentation</span></span>](http://www.npgsql.org/efcore/mapping/nts.html)
* [<span data-ttu-id="9ea8b-472">Documentazione di PostGIS</span><span class="sxs-lookup"><span data-stu-id="9ea8b-472">PostGIS Documentation</span></span>](http://postgis.net/documentation/)
