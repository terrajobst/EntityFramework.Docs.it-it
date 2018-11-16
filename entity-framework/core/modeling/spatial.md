---
title: Dati spaziali - EF Core
author: bricelam
ms.author: bricelam
ms.date: 11/01/2018
ms.assetid: 2BDE29FC-4161-41A0-841E-69F51CCD9341
uid: core/modeling/spatial
ms.openlocfilehash: 49c18758af2f2383ea082ead2f6df4c022152b36
ms.sourcegitcommit: b3c2b34d5f006ee3b41d6668f16fe7dcad1b4317
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 11/15/2018
ms.locfileid: "51688787"
---
# <a name="spatial-data"></a>Dati spaziali

> [!NOTE]
> Questa funzionalità è una novità di EF Core 2.2.

I dati spaziali rappresentano la posizione fisica e la forma degli oggetti. Molti database forniscono supporto per questo tipo di dati in modo che può essere indicizzato e sottoposti a query insieme agli altri dati. Gli scenari comuni includono l'esecuzione di query per gli oggetti all'interno di una determinata distanza da un percorso o selezionare l'oggetto il cui bordo contiene una posizione specificata. EF Core supporta il mapping ai tipi di dati spaziali utilizzando il [NetTopologySuite](https://github.com/NetTopologySuite/NetTopologySuite) libreria spaziale.

## <a name="installing"></a>Installazione di

Per usare i dati spaziali con EF Core, è necessario installare il pacchetto NuGet di supporto appropriato. Il pacchetto che è necessario installare dipende dal provider in uso.

Provider di EF Core                        | Pacchetto NuGet spaziali
--------------------------------------- | ---------------------
Entityframeworkcore | [Microsoft.EntityFrameworkCore.SqlServer.NetTopologySuite](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.SqlServer.NetTopologySuite)
Microsoft    | [Microsoft.EntityFrameworkCore.Sqlite.NetTopologySuite](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Sqlite.NetTopologySuite)
Microsoft.EntityFrameworkCore.InMemory  | [NetTopologySuite](https://www.nuget.org/packages/NetTopologySuite)
Npgsql   | [Npgsql.EntityFrameworkCore.PostgreSQL.NetTopologySuite](https://www.nuget.org/packages/Npgsql.EntityFrameworkCore.PostgreSQL.NetTopologySuite)

## <a name="reverse-engineering"></a>Decompilazione

NuGet spaziali pacchetti anche abilitare [decompilazione](../managing-schemas/scaffolding.md) modelli con le proprietà spaziali, ma è necessario installare il pacchetto ***prima*** esecuzione `Scaffold-DbContext` o `dotnet ef dbcontext scaffold`. In caso contrario, si riceveranno gli avvisi relativi non trova i mapping dei tipi per le colonne e le colonne verranno ignorate.

## <a name="nettopologysuite-nts"></a>NetTopologySuite (NTS)

NetTopologySuite è una libreria spaziale per .NET. EF Core Abilita mapping ai dati spaziali dei tipi nel database utilizzando tipi NTS nel modello.

Per abilitare il mapping a tipi di dati spaziali tramite NTS, chiamare il metodo UseNetTopologySuite sul generatore di opzioni del provider DbContext. Ad esempio, con SQL Server è necessario chiamarlo simile al seguente.

``` csharp
optionsBuilder.UseSqlServer(
    @"Data Source=(localdb)\MSSQLLocalDB;Initial Catalog=WideWorldImporters",
    x => x.UseNetTopologySuite());
```

Esistono diversi tipi di dati spaziali. Il tipo da utilizzare dipende dai tipi di forme che si desidera consentire. Di seguito è riportata la gerarchia dei tipi NTS che è possibile usare per le proprietà nel modello. Si trovano all'interno di `NetTopologySuite.Geometries` dello spazio dei nomi. Interfacce corrispondenti nel pacchetto GeoAPI (`GeoAPI.Geometries` dello spazio dei nomi) utilizzabili.

* Geometry
  * Punto
  * LineString
  * Poligono
  * GeometryCollection
    * MultiPoint
    * MultiLineString
    * MultiPolygon

> [!WARNING]
> CircularString e CompoundCurve CurePolygon non sono supportati da NTS.

Utilizzando il tipo di geometria di base consente qualsiasi tipo di forma da essere specificato dalla proprietà.

Le seguenti classi di entità può essere utilizzate per eseguire il mapping alle tabelle nel [database di esempio Wide World Importers](http://go.microsoft.com/fwlink/?LinkID=800630).

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

### <a name="creating-values"></a>Creazione di valori

È possibile usare i costruttori per creare oggetti geometry. Tuttavia, NTS consiglia di usare invece una factory di geometria. Ciò consente di specificare un identificatore predefinito SRID (il sistema di riferimento spaziale usato dalle coordinate) e consente di controllare aspetti più avanzati, ad esempio il modello di precisione (usato durante i calcoli) e la sequenza di coordinate (determina quali coordinate: le dimensioni e sono disponibili misure).

``` csharp
var geometryFactory = NtsGeometryServices.Instance.CreateGeometryFactory(srid: 4326);
var currentLocation = geometryFactory.CreatePoint(-122.121512, 47.6739882);
```

> [!NOTE]
> 4326 fa riferimento a WGS 84, uno standard usato in GPS e altri sistemi geografici.

### <a name="longitude-and-latitude"></a>Indicata con longitudine e latitudine

Coordinate NTS sono in termini di valori X e Y. Per rappresentare la longitudine e latitudine, usare X per Y e la longitudine della latitudine. Si noti che si tratta **con le versioni precedenti** dal `latitude, longitude` formato in cui è in genere visualizzare questi valori.

### <a name="srid-ignored-during-client-operations"></a>Identificatore SRID ignorato durante le operazioni client

NTS ignora i valori di identificatore SRID durante le operazioni. Si presuppone che un sistema di coordinate planare. Ciò significa che se si specificano le coordinate in termini di longitudine e latitudine, alcuni valori valutati client, ad esempio distanza, la lunghezza e area sarà espresso in gradi, non i contatori. Per i valori più significativi, è innanzitutto necessario proiettare le coordinate da un altro sistema di coordinate tramite una libreria come [ProjNet4GeoAPI](https://github.com/NetTopologySuite/ProjNet4GeoAPI) prima di calcolare tali valori.

Se un'operazione è valutato da Entity Framework Core con SQL server, unità del risultato verrà determinato dal database.

Ecco un esempio dell'uso ProjNet4GeoAPI per calcolare la distanza tra due città.

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

## <a name="querying-data"></a>Esecuzione di query su dati

In LINQ, i metodi NTS e le proprietà disponibili come funzioni di database verranno tradotto in SQL. Ad esempio, i metodi di distanza e Contains vengono convertiti nelle query seguenti. La tabella alla fine di questo articolo illustra quali membri sono supportati dai diversi provider di EF Core.

``` csharp
var nearestCity = db.Cities
    .OrderBy(c => c.Location.Distance(currentLocation))
    .FirstOrDefault();

var currentCountry = db.Countries
    .FirstOrDefault(c => c.Border.Contains(currentLocation));
```

## <a name="sql-server"></a>SQL Server

Se si usa SQL Server, esistono alcuni aspetti aggiuntivi di che è necessario essere consapevoli.

### <a name="geography-or-geometry"></a>Geografia o geometria

Per impostazione predefinita, le proprietà spaziali sono mappate a `geography` colonne in SQL Server. Per utilizzare `geometry`, [configurare il tipo di colonna](xref:core/modeling/relational/data-types) nel modello.

### <a name="geography-polygon-rings"></a>Anelli di poligono di geografia

Quando si usa il `geography` tipo di colonna, SQL Server impone requisiti aggiuntivi per l'anello esterno (o shell) e interni anelli (o aree libere). L'anello esterno deve essere orientata in senso antiorario e l'interno gli anelli in senso orario. NTS questa convalida prima di inviare i valori per il database.

### <a name="fullglobe"></a>FullGlobe

SQL Server dispone di un tipo di geometria non standard per rappresentare l'intero globo quando si usa il `geography` tipo della colonna. Include inoltre un modo per rappresentare i poligoni in base l'intero globo (senza un anello esterno). Nessuna di queste sono supportata da NTS.

> [!WARNING]
> FullGlobe e poligoni basati su di essa non sono supportati da NTS.

## <a name="sqlite"></a>SQLite

Ecco alcune informazioni aggiuntive per chi usa SQLite.

### <a name="installing-spatialite"></a>Installazione SpatiaLite

In Windows, la libreria nativa mod_spatialite viene distribuita come una dipendenza del pacchetto NuGet. Altre piattaforme è necessario installarlo separatamente. Questa operazione viene in genere eseguita usando un gestore di pacchetti software. Ad esempio, è possibile usare APT in Ubuntu e Homebrew in MacOS.

``` sh
# Ubuntu
apt-get install libsqlite3-mod-spatialite

# macOS
brew install libspatialite
```

### <a name="configuring-srid"></a>Configurazione di identificatore SRID

In SpatiaLite, le colonne necessarie specificare un identificatore SRID per ogni colonna. Il valore predefinito è SRID `0`. Specificare un diverso SRID utilizzando il metodo ForSqliteHasSrid.

``` csharp
modelBuilder.Entity<City>().Property(c => c.Location)
    .ForSqliteHasSrid(4326);
```

### <a name="dimension"></a>Dimensione

Simile a identificatore SRID, dimensione di una colonna (o nelle coordinate) viene specificate anche come parte della colonna. Le coordinate predefinito sono X e y. per abilitare aggiuntive nelle coordinate (Z e M) usando il metodo ForSqliteHasDimension.

``` csharp
modelBuilder.Entity<City>().Property(c => c.Location)
    .ForSqliteHasDimension(Ordinates.XYZ);
```

## <a name="translated-operations"></a>Operazioni tradotte

Questa tabella mostra i membri cui NTS vengono convertiti in SQL da ogni provider di EF Core.

NetTopologySuite | SQL Server (geometry) | SQL Server (geography) | SQLite | Npgsql
--- |:---:|:---:|:---:|:---:
Geometry.Area | ✔ | ✔ | ✔ | ✔
Geometry.AsBinary() | ✔ | ✔ | ✔ | ✔
Geometry.AsText() | ✔ | ✔ | ✔ | ✔
Geometry.Boundary | ✔ | | ✔ | ✔
Geometry.Buffer(double) | ✔ | ✔ | ✔ | ✔
Geometry.Buffer (double, int) | | | ✔
Geometry.Centroid | ✔ | | ✔ | ✔
Geometry.Contains(Geometry) | ✔ | ✔ | ✔ | ✔
Geometry.ConvexHull() | ✔ | ✔ | ✔ | ✔
Geometry.CoveredBy(Geometry) | | | ✔ | ✔
Geometry.Covers(Geometry) | | | ✔ | ✔
Geometry.Crosses(Geometry) | ✔ | | ✔ | ✔
Geometry.Difference(Geometry) | ✔ | ✔ | ✔ | ✔
Geometry.Dimension | ✔ | ✔ | ✔ | ✔
Geometry.Disjoint(Geometry) | ✔ | ✔ | ✔ | ✔
Geometry.Distance(Geometry) | ✔ | ✔ | ✔ | ✔
Geometry.Envelope | ✔ | | ✔ | ✔
Geometry.EqualsExact(Geometry) | | | | ✔
Geometry.EqualsTopologically(Geometry) | ✔ | ✔ | ✔ | ✔
Geometry.GeometryType | ✔ | ✔ | ✔ | ✔
Geometry.GetGeometryN(int) | ✔ | | ✔ | ✔
Geometry.InteriorPoint | ✔ | | ✔
Geometry.Intersection(Geometry) | ✔ | ✔ | ✔ | ✔
Geometry.Intersects(Geometry) | ✔ | ✔ | ✔ | ✔
Geometry.IsEmpty | ✔ | ✔ | ✔ | ✔
Geometry.IsSimple | ✔ | | ✔ | ✔
Geometry.IsValid | ✔ | ✔ | ✔ | ✔
Geometry.IsWithinDistance double (Geometry) | ✔ | | ✔
Geometry.Length | ✔ | ✔ | ✔ | ✔
Geometry.NumGeometries | ✔ | ✔ | ✔ | ✔
Geometry.NumPoints | ✔ | ✔ | ✔ | ✔
Geometry.OgcGeometryType | ✔ | ✔ | ✔
Geometry.Overlaps(Geometry) | ✔ | ✔ | ✔ | ✔
Geometry.PointOnSurface | ✔ | | ✔ | ✔
Geometry.Relate (Geometry, stringa) | ✔ | | ✔ | ✔
Geometry.Reverse() | | | ✔ | ✔
Geometry.SRID | ✔ | ✔ | ✔ | ✔
Geometry.SymmetricDifference(Geometry) | ✔ | ✔ | ✔ | ✔
Geometry.ToBinary() | ✔ | ✔ | ✔ | ✔
Geometry.ToText() | ✔ | ✔ | ✔ | ✔
Geometry.Touches(Geometry) | ✔ | | ✔ | ✔
Geometry.Union() | | | ✔
Geometry.Union(Geometry) | ✔ | ✔ | ✔ | ✔
Geometry.Within(Geometry) | ✔ | ✔ | ✔ | ✔
GeometryCollection.Count | ✔ | ✔ | ✔ | ✔
GeometryCollection di tipo [int] | ✔ | ✔ | ✔ | ✔
LineString.Count | ✔ | ✔ | ✔ | ✔
LineString.EndPoint | ✔ | ✔ | ✔ | ✔
LineString.GetPointN(int) | ✔ | ✔ | ✔ | ✔
LineString.IsClosed | ✔ | ✔ | ✔ | ✔
LineString.IsRing | ✔ | | ✔ | ✔
LineString.StartPoint | ✔ | ✔ | ✔ | ✔
MultiLineString.IsClosed | ✔ | ✔ | ✔ | ✔
Point.M | ✔ | ✔ | ✔ | ✔
Point | ✔ | ✔ | ✔ | ✔
Point | ✔ | ✔ | ✔ | ✔
Point.Z | ✔ | ✔ | ✔ | ✔
Polygon.ExteriorRing | ✔ | ✔ | ✔ | ✔
Polygon.GetInteriorRingN(int) | ✔ | ✔ | ✔ | ✔
Polygon.NumInteriorRings | ✔ | ✔ | ✔ | ✔

## <a name="additional-resources"></a>Risorse aggiuntive

* [Dati spaziali in SQL Server](https://docs.microsoft.com/sql/relational-databases/spatial/spatial-data-sql-server)
* [Home page SpatiaLite](https://www.gaia-gis.it/fossil/libspatialite)
* [Documentazione di PostGIS](http://postgis.net/documentation/)
