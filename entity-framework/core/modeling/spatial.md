---
title: Données spatiales - EF Core
author: bricelam
ms.author: bricelam
ms.date: 11/01/2018
ms.assetid: 2BDE29FC-4161-41A0-841E-69F51CCD9341
uid: core/modeling/spatial
ms.openlocfilehash: cf488c6b7d94ca19018efe1c23ff410fe7eb594b
ms.sourcegitcommit: 81c53ac43d8f15b900f117294ec71dc49fe028fa
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/16/2018
ms.locfileid: "51817908"
---
# <a name="spatial-data"></a>Données spatiales

> [!NOTE]
> Cette fonctionnalité est une nouveauté dans EF Core 2.2.

Données spatiales représentent l’emplacement physique et la forme d’objets. Plusieurs bases de données prennent en charge ce type de données afin de pouvoir être indexé et interrogée en même temps que d’autres données. Scénarios courants incluent l’interrogation d’objets dans un rayon donné à partir d’un emplacement, ou en sélectionnant l’objet dont la bordure contient un emplacement donné. EF Core prend en charge le mappage de types de données spatiales à l’aide de la [NetTopologySuite](https://github.com/NetTopologySuite/NetTopologySuite) bibliothèque spatiale.

## <a name="installing"></a>Installation de

Pour utiliser les données spatiales avec EF Core, vous devez installer le package NuGet de prise en charge approprié. Le package que vous devez installer varie selon le fournisseur que vous utilisez.

Fournisseur EF Core                        | Package NuGet spatial
--------------------------------------- | ---------------------
Microsoft.EntityFrameworkCore.SqlServer | [Microsoft.EntityFrameworkCore.SqlServer.NetTopologySuite](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.SqlServer.NetTopologySuite)
Microsoft.EntityFrameworkCore.Sqlite    | [Microsoft.EntityFrameworkCore.Sqlite.NetTopologySuite](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Sqlite.NetTopologySuite)
Microsoft.EntityFrameworkCore.InMemory  | [NetTopologySuite](https://www.nuget.org/packages/NetTopologySuite)
Npgsql.EntityFrameworkCore.PostgreSQL   | [Npgsql.EntityFrameworkCore.PostgreSQL.NetTopologySuite](https://www.nuget.org/packages/Npgsql.EntityFrameworkCore.PostgreSQL.NetTopologySuite)

## <a name="reverse-engineering"></a>Ingénierie à rebours

Les spatial packages NuGet également activer [ingénierie à rebours](../managing-schemas/scaffolding.md) modèles avec des propriétés spatiales, mais vous doivent installer le package ***avant*** en cours d’exécution `Scaffold-DbContext` ou `dotnet ef dbcontext scaffold`. Si vous ne le faites, vous recevez des avertissements sur la recherche ne pas de mappages de type pour les colonnes et les colonnes sont ignorés.

## <a name="nettopologysuite-nts"></a>NetTopologySuite (NTS)

NetTopologySuite est une bibliothèque spatiale pour .NET. EF Core autorise les types de mappage des données spatiales dans la base de données à l’aide des types NTS dans votre modèle.

Pour activer le mappage de types spatiaux via NTS, appelez la méthode de UseNetTopologySuite sur Générateur d’options du fournisseur DbContext. Par exemple, avec SQL Server vous appelez il comme suit.

``` csharp
optionsBuilder.UseSqlServer(
    @"Data Source=(localdb)\MSSQLLocalDB;Initial Catalog=WideWorldImporters",
    x => x.UseNetTopologySuite());
```

Il existe plusieurs types de données spatiales. Le type que vous utilisez dépend des types de formes que vous souhaitez autoriser. Voici la hiérarchie des types NTS que vous pouvez utiliser pour les propriétés dans votre modèle. Elles se trouvent dans le `NetTopologySuite.Geometries` espace de noms. Les interfaces correspondantes dans le package GeoAPI (`GeoAPI.Geometries` espace de noms) peut également être utilisé.

* géométrie
  * Point
  * LineString
  * Polygone
  * GeometryCollection
    * MultiPoint
    * MultiLineString
    * MultiPolygon

> [!WARNING]
> CircularString, CompoundCurve et CurePolygon ne sont pas pris en charge par NTS.

Le type de géométrie de base grâce à n’importe quel type de forme à être spécifié par la propriété.

Les classes d’entité suivante peut servir à mapper aux tables dans le [base de données Wide World Importers exemple](http://go.microsoft.com/fwlink/?LinkID=800630).

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

### <a name="creating-values"></a>Création de valeurs

Vous pouvez utiliser des constructeurs pour créer des objets de géométrie ; Toutefois, NTS recommande plutôt d’utiliser une fabrique de géométrie. Cela vous permet de spécifier une valeur par défaut SRID (le système de référence spatiale utilisé par les coordonnées) et vous donne un contrôle sur des éléments plus avancés tels que le modèle de précision (utilisé lors des calculs) et la séquence de coordonnées (détermine les coordonnées--dimensions et mesures sont disponibles).

``` csharp
var geometryFactory = NtsGeometryServices.Instance.CreateGeometryFactory(srid: 4326);
var currentLocation = geometryFactory.CreatePoint(-122.121512, 47.6739882);
```

> [!NOTE]
> 4326 fait référence à WGS 84, une norme utilisée dans le GPS et d’autres systèmes géographiques.

### <a name="longitude-and-latitude"></a>Longitude et Latitude

Coordonnées dans NTS sont exprimées en termes de valeurs X et Y. Pour représenter la longitude et latitude, utilisez X longitude et Y de latitude. Notez que cela est **descendante** à partir de la `latitude, longitude` format dans lequel vous voyez généralement ces valeurs.

### <a name="srid-ignored-during-client-operations"></a>SRID ignorés pendant les opérations du client

NTS ignore les valeurs SRID lors des opérations. Il suppose un système de coordonnées planaire. Cela signifie que si vous spécifiez des coordonnées en termes de longitude et latitude, certaines valeurs client évalué comme zone, la longueur et distance sera en degrés, pas de compteurs. Pour des valeurs plus significatives, vous devez tout d’abord projeter les coordonnées à un autre système de coordonnées à l’aide d’une bibliothèque comme [ProjNet4GeoAPI](https://github.com/NetTopologySuite/ProjNet4GeoAPI) avant de calculer ces valeurs.

Si une opération est évalué par EF Core par le biais de SQL server, les unités du résultat seront déterminée par la base de données.

Voici un exemple d’utilisation ProjNet4GeoAPI pour calculer la distance entre deux villes.

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

## <a name="querying-data"></a>Interrogation des données

Dans LINQ, les méthodes NTS et propriétés disponibles en tant que fonctions de base de données seront traduites à SQL. Par exemple, les méthodes de Distance et Contains sont traduits dans les requêtes suivantes. Le tableau à la fin de cet article indique quels membres sont pris en charge par divers fournisseurs d’EF Core.

``` csharp
var nearestCity = db.Cities
    .OrderBy(c => c.Location.Distance(currentLocation))
    .FirstOrDefault();

var currentCountry = db.Countries
    .FirstOrDefault(c => c.Border.Contains(currentLocation));
```

## <a name="sql-server"></a>SQL Server

Si vous utilisez SQL Server, il existe certaines choses supplémentaires, que vous devez connaître.

### <a name="geography-or-geometry"></a>Géographie ou géométrie

Par défaut, les propriétés spatiales sont mappées sur `geography` colonnes dans SQL Server. Pour utiliser `geometry`, [configurer le type de colonne](xref:core/modeling/relational/data-types) dans votre modèle.

### <a name="geography-polygon-rings"></a>Anneaux de polygon Geography

Lorsque vous utilisez le `geography` type de colonne, SQL Server impose des exigences supplémentaires sur l’anneau extérieur (ou un interpréteur de commandes) et intérieurs anneaux (ni aucune faille). L’anneau extérieur doit être orienté dans le sens anti-horaire et l’intérieur les anneaux dans le sens horaire. NTS cela valide avant d’envoyer des valeurs à la base de données.

### <a name="fullglobe"></a>FullGlobe

SQL Server dispose d’un type de géométrie non standard pour représenter le globe complet lorsque vous utilisez le `geography` type de colonne. Il a également un moyen pour représenter des polygones selon le globe complet (sans un anneau extérieur). Aucune de ces sont pris en charge par NTS.

> [!WARNING]
> Polygones basés sur celui-ci et FullGlobe ne sont pas pris en charge par NTS.

## <a name="sqlite"></a>SQLite

Voici quelques informations supplémentaires pour ceux qui utilisent SQLite.

### <a name="installing-spatialite"></a>L’installation SpatiaLite

La bibliothèque native mod_spatialite est distribuée sur Windows, comme une dépendance de package NuGet. Autres plateformes doivent l’installer séparément. Cela s’effectue généralement à l’aide d’un gestionnaire de package de logiciel. Par exemple, vous pouvez utiliser APT sur Ubuntu et Homebrew sur MacOS.

``` sh
# Ubuntu
apt-get install libsqlite3-mod-spatialite

# macOS
brew install libspatialite
```

### <a name="configuring-srid"></a>Configuration des SRID

Dans SpatiaLite, les colonnes doivent spécifier un SRID par colonne. La valeur par défaut est de SRID `0`. Spécifiez un SRID différent à l’aide de la méthode ForSqliteHasSrid.

``` csharp
modelBuilder.Entity<City>().Property(c => c.Location)
    .ForSqliteHasSrid(4326);
```

### <a name="dimension"></a>Dimension

Comme pour les SRID, dimension d’une colonne (ou les coordonnées) sont également spécifiées dans le cadre de la colonne. Les coordonnées de la valeur par défaut sont X et Y. activer les coordonnées supplémentaires (Z et M) à l’aide de la méthode ForSqliteHasDimension.

``` csharp
modelBuilder.Entity<City>().Property(c => c.Location)
    .ForSqliteHasDimension(Ordinates.XYZ);
```

## <a name="translated-operations"></a>Opérations traduites

Ce tableau indique quels membres NTS sont traduites en SQL par chaque fournisseur EF Core.

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
Geometry.IsWithinDistance (Geometry, double) | ✔ | | ✔
Geometry.Length | ✔ | ✔ | ✔ | ✔
Geometry.NumGeometries | ✔ | ✔ | ✔ | ✔
Geometry.NumPoints | ✔ | ✔ | ✔ | ✔
Geometry.OgcGeometryType | ✔ | ✔ | ✔
Geometry.Overlaps(Geometry) | ✔ | ✔ | ✔ | ✔
Geometry.PointOnSurface | ✔ | | ✔ | ✔
Geometry.Relate (Geometry, chaîne) | ✔ | | ✔ | ✔
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
GeometryCollection [int] | ✔ | ✔ | ✔ | ✔
LineString.Count | ✔ | ✔ | ✔ | ✔
LineString.EndPoint | ✔ | ✔ | ✔ | ✔
LineString.GetPointN(int) | ✔ | ✔ | ✔ | ✔
LineString.IsClosed | ✔ | ✔ | ✔ | ✔
LineString.IsRing | ✔ | | ✔ | ✔
LineString.StartPoint | ✔ | ✔ | ✔ | ✔
MultiLineString.IsClosed | ✔ | ✔ | ✔ | ✔
Point.M | ✔ | ✔ | ✔ | ✔
Point.X | ✔ | ✔ | ✔ | ✔
Point.Y | ✔ | ✔ | ✔ | ✔
Point.Z | ✔ | ✔ | ✔ | ✔
Polygon.ExteriorRing | ✔ | ✔ | ✔ | ✔
Polygon.GetInteriorRingN(int) | ✔ | ✔ | ✔ | ✔
Polygon.NumInteriorRings | ✔ | ✔ | ✔ | ✔

## <a name="additional-resources"></a>Ressources supplémentaires

* [Données spatiales dans SQL Server](https://docs.microsoft.com/sql/relational-databases/spatial/spatial-data-sql-server)
* [Page d’accueil SpatiaLite](https://www.gaia-gis.it/fossil/libspatialite)
* [Documentation de Npgsql Spatial](http://www.npgsql.org/efcore/mapping/nts.html)
* [Documentation de PostGIS](http://postgis.net/documentation/)
