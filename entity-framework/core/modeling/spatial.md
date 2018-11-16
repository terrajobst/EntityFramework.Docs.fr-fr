---
title: Données spatiales - EF Core
author: bricelam
ms.author: bricelam
ms.date: 11/01/2018
ms.assetid: 2BDE29FC-4161-41A0-841E-69F51CCD9341
uid: core/modeling/spatial
ms.openlocfilehash: 49c18758af2f2383ea082ead2f6df4c022152b36
ms.sourcegitcommit: b3c2b34d5f006ee3b41d6668f16fe7dcad1b4317
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/15/2018
ms.locfileid: "51688785"
---
# <a name="spatial-data"></a><span data-ttu-id="14bf7-102">Données spatiales</span><span class="sxs-lookup"><span data-stu-id="14bf7-102">Spatial Data</span></span>

> [!NOTE]
> <span data-ttu-id="14bf7-103">Cette fonctionnalité est une nouveauté dans EF Core 2.2.</span><span class="sxs-lookup"><span data-stu-id="14bf7-103">This feature is new in EF Core 2.2.</span></span>

<span data-ttu-id="14bf7-104">Données spatiales représentent l’emplacement physique et la forme d’objets.</span><span class="sxs-lookup"><span data-stu-id="14bf7-104">Spatial data represents the physical location and the shape of objects.</span></span> <span data-ttu-id="14bf7-105">Plusieurs bases de données prennent en charge ce type de données afin de pouvoir être indexé et interrogée en même temps que d’autres données.</span><span class="sxs-lookup"><span data-stu-id="14bf7-105">Many databases provide support for this type of data so it can be indexed and queried alongside other data.</span></span> <span data-ttu-id="14bf7-106">Scénarios courants incluent l’interrogation d’objets dans un rayon donné à partir d’un emplacement, ou en sélectionnant l’objet dont la bordure contient un emplacement donné.</span><span class="sxs-lookup"><span data-stu-id="14bf7-106">Common scenarios include querying for objects within a given distance from a location, or selecting the object whose border contains a given location.</span></span> <span data-ttu-id="14bf7-107">EF Core prend en charge le mappage de types de données spatiales à l’aide de la [NetTopologySuite](https://github.com/NetTopologySuite/NetTopologySuite) bibliothèque spatiale.</span><span class="sxs-lookup"><span data-stu-id="14bf7-107">EF Core supports mapping to spatial data types using the [NetTopologySuite](https://github.com/NetTopologySuite/NetTopologySuite) spatial library.</span></span>

## <a name="installing"></a><span data-ttu-id="14bf7-108">Installation de</span><span class="sxs-lookup"><span data-stu-id="14bf7-108">Installing</span></span>

<span data-ttu-id="14bf7-109">Pour utiliser les données spatiales avec EF Core, vous devez installer le package NuGet de prise en charge approprié.</span><span class="sxs-lookup"><span data-stu-id="14bf7-109">In order to use spatial data with EF Core, you need to install the appropriate supporting NuGet package.</span></span> <span data-ttu-id="14bf7-110">Le package que vous devez installer varie selon le fournisseur que vous utilisez.</span><span class="sxs-lookup"><span data-stu-id="14bf7-110">Which package you need to install depends on the provider you're using.</span></span>

<span data-ttu-id="14bf7-111">Fournisseur EF Core</span><span class="sxs-lookup"><span data-stu-id="14bf7-111">EF Core Provider</span></span>                        | <span data-ttu-id="14bf7-112">Package NuGet spatial</span><span class="sxs-lookup"><span data-stu-id="14bf7-112">Spatial NuGet Package</span></span>
--------------------------------------- | ---------------------
<span data-ttu-id="14bf7-113">Microsoft.EntityFrameworkCore.SqlServer</span><span class="sxs-lookup"><span data-stu-id="14bf7-113">Microsoft.EntityFrameworkCore.SqlServer</span></span> | [<span data-ttu-id="14bf7-114">Microsoft.EntityFrameworkCore.SqlServer.NetTopologySuite</span><span class="sxs-lookup"><span data-stu-id="14bf7-114">Microsoft.EntityFrameworkCore.SqlServer.NetTopologySuite</span></span>](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.SqlServer.NetTopologySuite)
<span data-ttu-id="14bf7-115">Microsoft.EntityFrameworkCore.Sqlite</span><span class="sxs-lookup"><span data-stu-id="14bf7-115">Microsoft.EntityFrameworkCore.Sqlite</span></span>    | [<span data-ttu-id="14bf7-116">Microsoft.EntityFrameworkCore.Sqlite.NetTopologySuite</span><span class="sxs-lookup"><span data-stu-id="14bf7-116">Microsoft.EntityFrameworkCore.Sqlite.NetTopologySuite</span></span>](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Sqlite.NetTopologySuite)
<span data-ttu-id="14bf7-117">Microsoft.EntityFrameworkCore.InMemory</span><span class="sxs-lookup"><span data-stu-id="14bf7-117">Microsoft.EntityFrameworkCore.InMemory</span></span>  | [<span data-ttu-id="14bf7-118">NetTopologySuite</span><span class="sxs-lookup"><span data-stu-id="14bf7-118">NetTopologySuite</span></span>](https://www.nuget.org/packages/NetTopologySuite)
<span data-ttu-id="14bf7-119">Npgsql.EntityFrameworkCore.PostgreSQL</span><span class="sxs-lookup"><span data-stu-id="14bf7-119">Npgsql.EntityFrameworkCore.PostgreSQL</span></span>   | [<span data-ttu-id="14bf7-120">Npgsql.EntityFrameworkCore.PostgreSQL.NetTopologySuite</span><span class="sxs-lookup"><span data-stu-id="14bf7-120">Npgsql.EntityFrameworkCore.PostgreSQL.NetTopologySuite</span></span>](https://www.nuget.org/packages/Npgsql.EntityFrameworkCore.PostgreSQL.NetTopologySuite)

## <a name="reverse-engineering"></a><span data-ttu-id="14bf7-121">Ingénierie à rebours</span><span class="sxs-lookup"><span data-stu-id="14bf7-121">Reverse engineering</span></span>

<span data-ttu-id="14bf7-122">Les spatial packages NuGet également activer [ingénierie à rebours](../managing-schemas/scaffolding.md) modèles avec des propriétés spatiales, mais vous doivent installer le package ***avant*** en cours d’exécution `Scaffold-DbContext` ou `dotnet ef dbcontext scaffold`.</span><span class="sxs-lookup"><span data-stu-id="14bf7-122">The spatial NuGet packages also enable [reverse engineering](../managing-schemas/scaffolding.md) models with spatial properties, but you need to install the package ***before*** running `Scaffold-DbContext` or `dotnet ef dbcontext scaffold`.</span></span> <span data-ttu-id="14bf7-123">Si vous ne le faites, vous recevez des avertissements sur la recherche ne pas de mappages de type pour les colonnes et les colonnes sont ignorés.</span><span class="sxs-lookup"><span data-stu-id="14bf7-123">If you don't, you'll receive warnings about not finding type mappings for the columns and the columns will be skipped.</span></span>

## <a name="nettopologysuite-nts"></a><span data-ttu-id="14bf7-124">NetTopologySuite (NTS)</span><span class="sxs-lookup"><span data-stu-id="14bf7-124">NetTopologySuite (NTS)</span></span>

<span data-ttu-id="14bf7-125">NetTopologySuite est une bibliothèque spatiale pour .NET.</span><span class="sxs-lookup"><span data-stu-id="14bf7-125">NetTopologySuite is a spatial library for .NET.</span></span> <span data-ttu-id="14bf7-126">EF Core autorise les types de mappage des données spatiales dans la base de données à l’aide des types NTS dans votre modèle.</span><span class="sxs-lookup"><span data-stu-id="14bf7-126">EF Core enables mapping to spatial data types in the database by using NTS types in your model.</span></span>

<span data-ttu-id="14bf7-127">Pour activer le mappage de types spatiaux via NTS, appelez la méthode de UseNetTopologySuite sur Générateur d’options du fournisseur DbContext.</span><span class="sxs-lookup"><span data-stu-id="14bf7-127">To enable mapping to spatial types via NTS, call the UseNetTopologySuite method on the provider's DbContext options builder.</span></span> <span data-ttu-id="14bf7-128">Par exemple, avec SQL Server vous appelez il comme suit.</span><span class="sxs-lookup"><span data-stu-id="14bf7-128">For example, with SQL Server you'd call it like this.</span></span>

``` csharp
optionsBuilder.UseSqlServer(
    @"Data Source=(localdb)\MSSQLLocalDB;Initial Catalog=WideWorldImporters",
    x => x.UseNetTopologySuite());
```

<span data-ttu-id="14bf7-129">Il existe plusieurs types de données spatiales.</span><span class="sxs-lookup"><span data-stu-id="14bf7-129">There are several spatial data types.</span></span> <span data-ttu-id="14bf7-130">Le type que vous utilisez dépend des types de formes que vous souhaitez autoriser.</span><span class="sxs-lookup"><span data-stu-id="14bf7-130">Which type you use depends on the types of shapes you want to allow.</span></span> <span data-ttu-id="14bf7-131">Voici la hiérarchie des types NTS que vous pouvez utiliser pour les propriétés dans votre modèle.</span><span class="sxs-lookup"><span data-stu-id="14bf7-131">Here is the hierarchy of NTS types that you can use for properties in your model.</span></span> <span data-ttu-id="14bf7-132">Elles se trouvent dans le `NetTopologySuite.Geometries` espace de noms.</span><span class="sxs-lookup"><span data-stu-id="14bf7-132">They're located within the `NetTopologySuite.Geometries` namespace.</span></span> <span data-ttu-id="14bf7-133">Les interfaces correspondantes dans le package GeoAPI (`GeoAPI.Geometries` espace de noms) peut également être utilisé.</span><span class="sxs-lookup"><span data-stu-id="14bf7-133">Corresponding interfaces in the GeoAPI package (`GeoAPI.Geometries` namespace) can also be used.</span></span>

* <span data-ttu-id="14bf7-134">géométrie</span><span class="sxs-lookup"><span data-stu-id="14bf7-134">Geometry</span></span>
  * <span data-ttu-id="14bf7-135">Point</span><span class="sxs-lookup"><span data-stu-id="14bf7-135">Point</span></span>
  * <span data-ttu-id="14bf7-136">LineString</span><span class="sxs-lookup"><span data-stu-id="14bf7-136">LineString</span></span>
  * <span data-ttu-id="14bf7-137">Polygone</span><span class="sxs-lookup"><span data-stu-id="14bf7-137">Polygon</span></span>
  * <span data-ttu-id="14bf7-138">GeometryCollection</span><span class="sxs-lookup"><span data-stu-id="14bf7-138">GeometryCollection</span></span>
    * <span data-ttu-id="14bf7-139">MultiPoint</span><span class="sxs-lookup"><span data-stu-id="14bf7-139">MultiPoint</span></span>
    * <span data-ttu-id="14bf7-140">MultiLineString</span><span class="sxs-lookup"><span data-stu-id="14bf7-140">MultiLineString</span></span>
    * <span data-ttu-id="14bf7-141">MultiPolygon</span><span class="sxs-lookup"><span data-stu-id="14bf7-141">MultiPolygon</span></span>

> [!WARNING]
> <span data-ttu-id="14bf7-142">CircularString, CompoundCurve et CurePolygon ne sont pas pris en charge par NTS.</span><span class="sxs-lookup"><span data-stu-id="14bf7-142">CircularString, CompoundCurve, and CurePolygon aren't supported by NTS.</span></span>

<span data-ttu-id="14bf7-143">Le type de géométrie de base grâce à n’importe quel type de forme à être spécifié par la propriété.</span><span class="sxs-lookup"><span data-stu-id="14bf7-143">Using the base Geometry type allows any type of shape to be specified by the property.</span></span>

<span data-ttu-id="14bf7-144">Les classes d’entité suivante peut servir à mapper aux tables dans le [base de données Wide World Importers exemple](http://go.microsoft.com/fwlink/?LinkID=800630).</span><span class="sxs-lookup"><span data-stu-id="14bf7-144">The following entity classes could be used to map to tables in the [Wide World Importers sample database](http://go.microsoft.com/fwlink/?LinkID=800630).</span></span>

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

### <a name="creating-values"></a><span data-ttu-id="14bf7-145">Création de valeurs</span><span class="sxs-lookup"><span data-stu-id="14bf7-145">Creating values</span></span>

<span data-ttu-id="14bf7-146">Vous pouvez utiliser des constructeurs pour créer des objets de géométrie ; Toutefois, NTS recommande plutôt d’utiliser une fabrique de géométrie.</span><span class="sxs-lookup"><span data-stu-id="14bf7-146">You can use constructors to create geometry objects; however, NTS recommends using a geometry factory instead.</span></span> <span data-ttu-id="14bf7-147">Cela vous permet de spécifier une valeur par défaut SRID (le système de référence spatiale utilisé par les coordonnées) et vous donne un contrôle sur des éléments plus avancés tels que le modèle de précision (utilisé lors des calculs) et la séquence de coordonnées (détermine les coordonnées--dimensions et mesures sont disponibles).</span><span class="sxs-lookup"><span data-stu-id="14bf7-147">This lets you specify a default SRID (the spatial reference system used by the coordinates) and gives you control over more advanced things like the precision model (used during calculations) and the coordinate sequence (determines which ordinates--dimensions and measures--are available).</span></span>

``` csharp
var geometryFactory = NtsGeometryServices.Instance.CreateGeometryFactory(srid: 4326);
var currentLocation = geometryFactory.CreatePoint(-122.121512, 47.6739882);
```

> [!NOTE]
> <span data-ttu-id="14bf7-148">4326 fait référence à WGS 84, une norme utilisée dans le GPS et d’autres systèmes géographiques.</span><span class="sxs-lookup"><span data-stu-id="14bf7-148">4326 refers to WGS 84, a standard used in GPS and other geographic systems.</span></span>

### <a name="longitude-and-latitude"></a><span data-ttu-id="14bf7-149">Longitude et Latitude</span><span class="sxs-lookup"><span data-stu-id="14bf7-149">Longitude and Latitude</span></span>

<span data-ttu-id="14bf7-150">Coordonnées dans NTS sont exprimées en termes de valeurs X et Y.</span><span class="sxs-lookup"><span data-stu-id="14bf7-150">Coordinates in NTS are in terms of X and Y values.</span></span> <span data-ttu-id="14bf7-151">Pour représenter la longitude et latitude, utilisez X longitude et Y de latitude.</span><span class="sxs-lookup"><span data-stu-id="14bf7-151">To represent longitude and latitude, use X for longitude and Y for latitude.</span></span> <span data-ttu-id="14bf7-152">Notez que cela est **descendante** à partir de la `latitude, longitude` format dans lequel vous voyez généralement ces valeurs.</span><span class="sxs-lookup"><span data-stu-id="14bf7-152">Note that this is **backwards** from the `latitude, longitude` format in which you typically see these values.</span></span>

### <a name="srid-ignored-during-client-operations"></a><span data-ttu-id="14bf7-153">SRID ignorés pendant les opérations du client</span><span class="sxs-lookup"><span data-stu-id="14bf7-153">SRID Ignored during client operations</span></span>

<span data-ttu-id="14bf7-154">NTS ignore les valeurs SRID lors des opérations.</span><span class="sxs-lookup"><span data-stu-id="14bf7-154">NTS ignores SRID values during operations.</span></span> <span data-ttu-id="14bf7-155">Il suppose un système de coordonnées planaire.</span><span class="sxs-lookup"><span data-stu-id="14bf7-155">It assumes a planar coordinate system.</span></span> <span data-ttu-id="14bf7-156">Cela signifie que si vous spécifiez des coordonnées en termes de longitude et latitude, certaines valeurs client évalué comme zone, la longueur et distance sera en degrés, pas de compteurs.</span><span class="sxs-lookup"><span data-stu-id="14bf7-156">This means that if you specify coordinates in terms of longitude and latitude, some client-evaluated values like distance, length, and area will be in degrees, not meters.</span></span> <span data-ttu-id="14bf7-157">Pour des valeurs plus significatives, vous devez tout d’abord projeter les coordonnées à un autre système de coordonnées à l’aide d’une bibliothèque comme [ProjNet4GeoAPI](https://github.com/NetTopologySuite/ProjNet4GeoAPI) avant de calculer ces valeurs.</span><span class="sxs-lookup"><span data-stu-id="14bf7-157">For more meaningful values, you first need to project the coordinates to another coordinate system using a library like [ProjNet4GeoAPI](https://github.com/NetTopologySuite/ProjNet4GeoAPI) before calculating these values.</span></span>

<span data-ttu-id="14bf7-158">Si une opération est évalué par EF Core par le biais de SQL server, les unités du résultat seront déterminée par la base de données.</span><span class="sxs-lookup"><span data-stu-id="14bf7-158">If an operation is server-evaluated by EF Core via SQL, the result's unit will be determined by the database.</span></span>

<span data-ttu-id="14bf7-159">Voici un exemple d’utilisation ProjNet4GeoAPI pour calculer la distance entre deux villes.</span><span class="sxs-lookup"><span data-stu-id="14bf7-159">Here is an example of using ProjNet4GeoAPI to calculate the distance between two cities.</span></span>

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

## <a name="querying-data"></a><span data-ttu-id="14bf7-160">Interrogation des données</span><span class="sxs-lookup"><span data-stu-id="14bf7-160">Querying Data</span></span>

<span data-ttu-id="14bf7-161">Dans LINQ, les méthodes NTS et propriétés disponibles en tant que fonctions de base de données seront traduites à SQL.</span><span class="sxs-lookup"><span data-stu-id="14bf7-161">In LINQ, the NTS methods and properties available as database functions will be translated to SQL.</span></span> <span data-ttu-id="14bf7-162">Par exemple, les méthodes de Distance et Contains sont traduits dans les requêtes suivantes.</span><span class="sxs-lookup"><span data-stu-id="14bf7-162">For example, the Distance and Contains methods are translated in the following queries.</span></span> <span data-ttu-id="14bf7-163">Le tableau à la fin de cet article indique quels membres sont pris en charge par divers fournisseurs d’EF Core.</span><span class="sxs-lookup"><span data-stu-id="14bf7-163">The table at the end of this article shows which members are supported by various EF Core providers.</span></span>

``` csharp
var nearestCity = db.Cities
    .OrderBy(c => c.Location.Distance(currentLocation))
    .FirstOrDefault();

var currentCountry = db.Countries
    .FirstOrDefault(c => c.Border.Contains(currentLocation));
```

## <a name="sql-server"></a><span data-ttu-id="14bf7-164">SQL Server</span><span class="sxs-lookup"><span data-stu-id="14bf7-164">SQL Server</span></span>

<span data-ttu-id="14bf7-165">Si vous utilisez SQL Server, il existe certaines choses supplémentaires, que vous devez connaître.</span><span class="sxs-lookup"><span data-stu-id="14bf7-165">If you're using SQL Server, there are some additional things you should be aware of.</span></span>

### <a name="geography-or-geometry"></a><span data-ttu-id="14bf7-166">Géographie ou géométrie</span><span class="sxs-lookup"><span data-stu-id="14bf7-166">Geography or geometry</span></span>

<span data-ttu-id="14bf7-167">Par défaut, les propriétés spatiales sont mappées sur `geography` colonnes dans SQL Server.</span><span class="sxs-lookup"><span data-stu-id="14bf7-167">By default, spatial properties are mapped to `geography` columns in SQL Server.</span></span> <span data-ttu-id="14bf7-168">Pour utiliser `geometry`, [configurer le type de colonne](xref:core/modeling/relational/data-types) dans votre modèle.</span><span class="sxs-lookup"><span data-stu-id="14bf7-168">To use `geometry`, [configure the column type](xref:core/modeling/relational/data-types) in your model.</span></span>

### <a name="geography-polygon-rings"></a><span data-ttu-id="14bf7-169">Anneaux de polygon Geography</span><span class="sxs-lookup"><span data-stu-id="14bf7-169">Geography polygon rings</span></span>

<span data-ttu-id="14bf7-170">Lorsque vous utilisez le `geography` type de colonne, SQL Server impose des exigences supplémentaires sur l’anneau extérieur (ou un interpréteur de commandes) et intérieurs anneaux (ni aucune faille).</span><span class="sxs-lookup"><span data-stu-id="14bf7-170">When using the `geography` column type, SQL Server imposes additional requirements on the exterior ring (or shell) and interior rings (or holes).</span></span> <span data-ttu-id="14bf7-171">L’anneau extérieur doit être orienté dans le sens anti-horaire et l’intérieur les anneaux dans le sens horaire.</span><span class="sxs-lookup"><span data-stu-id="14bf7-171">The exterior ring must be oriented counterclockwise and the interior rings clockwise.</span></span> <span data-ttu-id="14bf7-172">NTS cela valide avant d’envoyer des valeurs à la base de données.</span><span class="sxs-lookup"><span data-stu-id="14bf7-172">NTS validates this before sending values to the database.</span></span>

### <a name="fullglobe"></a><span data-ttu-id="14bf7-173">FullGlobe</span><span class="sxs-lookup"><span data-stu-id="14bf7-173">FullGlobe</span></span>

<span data-ttu-id="14bf7-174">SQL Server dispose d’un type de géométrie non standard pour représenter le globe complet lorsque vous utilisez le `geography` type de colonne.</span><span class="sxs-lookup"><span data-stu-id="14bf7-174">SQL Server has a non-standard geometry type to represent the full globe when using the `geography` column type.</span></span> <span data-ttu-id="14bf7-175">Il a également un moyen pour représenter des polygones selon le globe complet (sans un anneau extérieur).</span><span class="sxs-lookup"><span data-stu-id="14bf7-175">It also has a way to represent polygons based on the full globe (without an exterior ring).</span></span> <span data-ttu-id="14bf7-176">Aucune de ces sont pris en charge par NTS.</span><span class="sxs-lookup"><span data-stu-id="14bf7-176">Neither of these are supported by NTS.</span></span>

> [!WARNING]
> <span data-ttu-id="14bf7-177">Polygones basés sur celui-ci et FullGlobe ne sont pas pris en charge par NTS.</span><span class="sxs-lookup"><span data-stu-id="14bf7-177">FullGlobe and polygons based on it aren't supported by NTS.</span></span>

## <a name="sqlite"></a><span data-ttu-id="14bf7-178">SQLite</span><span class="sxs-lookup"><span data-stu-id="14bf7-178">SQLite</span></span>

<span data-ttu-id="14bf7-179">Voici quelques informations supplémentaires pour ceux qui utilisent SQLite.</span><span class="sxs-lookup"><span data-stu-id="14bf7-179">Here is some additional information for those using SQLite.</span></span>

### <a name="installing-spatialite"></a><span data-ttu-id="14bf7-180">L’installation SpatiaLite</span><span class="sxs-lookup"><span data-stu-id="14bf7-180">Installing SpatiaLite</span></span>

<span data-ttu-id="14bf7-181">La bibliothèque native mod_spatialite est distribuée sur Windows, comme une dépendance de package NuGet.</span><span class="sxs-lookup"><span data-stu-id="14bf7-181">On Windows, the native mod_spatialite library is distributed as a NuGet package dependency.</span></span> <span data-ttu-id="14bf7-182">Autres plateformes doivent l’installer séparément.</span><span class="sxs-lookup"><span data-stu-id="14bf7-182">Other platforms need to install it separately.</span></span> <span data-ttu-id="14bf7-183">Cela s’effectue généralement à l’aide d’un gestionnaire de package de logiciel.</span><span class="sxs-lookup"><span data-stu-id="14bf7-183">This is typically done using a software package manager.</span></span> <span data-ttu-id="14bf7-184">Par exemple, vous pouvez utiliser APT sur Ubuntu et Homebrew sur MacOS.</span><span class="sxs-lookup"><span data-stu-id="14bf7-184">For example, you can use APT on Ubuntu and Homebrew on MacOS.</span></span>

``` sh
# Ubuntu
apt-get install libsqlite3-mod-spatialite

# macOS
brew install libspatialite
```

### <a name="configuring-srid"></a><span data-ttu-id="14bf7-185">Configuration des SRID</span><span class="sxs-lookup"><span data-stu-id="14bf7-185">Configuring SRID</span></span>

<span data-ttu-id="14bf7-186">Dans SpatiaLite, les colonnes doivent spécifier un SRID par colonne.</span><span class="sxs-lookup"><span data-stu-id="14bf7-186">In SpatiaLite, columns need to specify an SRID per column.</span></span> <span data-ttu-id="14bf7-187">La valeur par défaut est de SRID `0`.</span><span class="sxs-lookup"><span data-stu-id="14bf7-187">The default SRID is `0`.</span></span> <span data-ttu-id="14bf7-188">Spécifiez un SRID différent à l’aide de la méthode ForSqliteHasSrid.</span><span class="sxs-lookup"><span data-stu-id="14bf7-188">Specify a different SRID using the ForSqliteHasSrid method.</span></span>

``` csharp
modelBuilder.Entity<City>().Property(c => c.Location)
    .ForSqliteHasSrid(4326);
```

### <a name="dimension"></a><span data-ttu-id="14bf7-189">Dimension</span><span class="sxs-lookup"><span data-stu-id="14bf7-189">Dimension</span></span>

<span data-ttu-id="14bf7-190">Comme pour les SRID, dimension d’une colonne (ou les coordonnées) sont également spécifiées dans le cadre de la colonne.</span><span class="sxs-lookup"><span data-stu-id="14bf7-190">Similar to SRID, a column's dimension (or ordinates) is also specified as part of the column.</span></span> <span data-ttu-id="14bf7-191">Les coordonnées de la valeur par défaut sont X et Y. activer les coordonnées supplémentaires (Z et M) à l’aide de la méthode ForSqliteHasDimension.</span><span class="sxs-lookup"><span data-stu-id="14bf7-191">The default ordinates are X and Y. Enable additional ordinates (Z and M) using the ForSqliteHasDimension method.</span></span>

``` csharp
modelBuilder.Entity<City>().Property(c => c.Location)
    .ForSqliteHasDimension(Ordinates.XYZ);
```

## <a name="translated-operations"></a><span data-ttu-id="14bf7-192">Opérations traduites</span><span class="sxs-lookup"><span data-stu-id="14bf7-192">Translated Operations</span></span>

<span data-ttu-id="14bf7-193">Ce tableau indique quels membres NTS sont traduites en SQL par chaque fournisseur EF Core.</span><span class="sxs-lookup"><span data-stu-id="14bf7-193">This table shows which NTS members are translated into SQL by each EF Core provider.</span></span>

<span data-ttu-id="14bf7-194">NetTopologySuite</span><span class="sxs-lookup"><span data-stu-id="14bf7-194">NetTopologySuite</span></span> | <span data-ttu-id="14bf7-195">SQL Server (geometry)</span><span class="sxs-lookup"><span data-stu-id="14bf7-195">SQL Server (geometry)</span></span> | <span data-ttu-id="14bf7-196">SQL Server (geography)</span><span class="sxs-lookup"><span data-stu-id="14bf7-196">SQL Server (geography)</span></span> | <span data-ttu-id="14bf7-197">SQLite</span><span class="sxs-lookup"><span data-stu-id="14bf7-197">SQLite</span></span> | <span data-ttu-id="14bf7-198">Npgsql</span><span class="sxs-lookup"><span data-stu-id="14bf7-198">Npgsql</span></span>
--- |:---:|:---:|:---:|:---:
<span data-ttu-id="14bf7-199">Geometry.Area</span><span class="sxs-lookup"><span data-stu-id="14bf7-199">Geometry.Area</span></span> | <span data-ttu-id="14bf7-200">✔</span><span class="sxs-lookup"><span data-stu-id="14bf7-200">✔</span></span> | <span data-ttu-id="14bf7-201">✔</span><span class="sxs-lookup"><span data-stu-id="14bf7-201">✔</span></span> | <span data-ttu-id="14bf7-202">✔</span><span class="sxs-lookup"><span data-stu-id="14bf7-202">✔</span></span> | <span data-ttu-id="14bf7-203">✔</span><span class="sxs-lookup"><span data-stu-id="14bf7-203">✔</span></span>
<span data-ttu-id="14bf7-204">Geometry.AsBinary()</span><span class="sxs-lookup"><span data-stu-id="14bf7-204">Geometry.AsBinary()</span></span> | <span data-ttu-id="14bf7-205">✔</span><span class="sxs-lookup"><span data-stu-id="14bf7-205">✔</span></span> | <span data-ttu-id="14bf7-206">✔</span><span class="sxs-lookup"><span data-stu-id="14bf7-206">✔</span></span> | <span data-ttu-id="14bf7-207">✔</span><span class="sxs-lookup"><span data-stu-id="14bf7-207">✔</span></span> | <span data-ttu-id="14bf7-208">✔</span><span class="sxs-lookup"><span data-stu-id="14bf7-208">✔</span></span>
<span data-ttu-id="14bf7-209">Geometry.AsText()</span><span class="sxs-lookup"><span data-stu-id="14bf7-209">Geometry.AsText()</span></span> | <span data-ttu-id="14bf7-210">✔</span><span class="sxs-lookup"><span data-stu-id="14bf7-210">✔</span></span> | <span data-ttu-id="14bf7-211">✔</span><span class="sxs-lookup"><span data-stu-id="14bf7-211">✔</span></span> | <span data-ttu-id="14bf7-212">✔</span><span class="sxs-lookup"><span data-stu-id="14bf7-212">✔</span></span> | <span data-ttu-id="14bf7-213">✔</span><span class="sxs-lookup"><span data-stu-id="14bf7-213">✔</span></span>
<span data-ttu-id="14bf7-214">Geometry.Boundary</span><span class="sxs-lookup"><span data-stu-id="14bf7-214">Geometry.Boundary</span></span> | <span data-ttu-id="14bf7-215">✔</span><span class="sxs-lookup"><span data-stu-id="14bf7-215">✔</span></span> | | <span data-ttu-id="14bf7-216">✔</span><span class="sxs-lookup"><span data-stu-id="14bf7-216">✔</span></span> | <span data-ttu-id="14bf7-217">✔</span><span class="sxs-lookup"><span data-stu-id="14bf7-217">✔</span></span>
<span data-ttu-id="14bf7-218">Geometry.Buffer(double)</span><span class="sxs-lookup"><span data-stu-id="14bf7-218">Geometry.Buffer(double)</span></span> | <span data-ttu-id="14bf7-219">✔</span><span class="sxs-lookup"><span data-stu-id="14bf7-219">✔</span></span> | <span data-ttu-id="14bf7-220">✔</span><span class="sxs-lookup"><span data-stu-id="14bf7-220">✔</span></span> | <span data-ttu-id="14bf7-221">✔</span><span class="sxs-lookup"><span data-stu-id="14bf7-221">✔</span></span> | <span data-ttu-id="14bf7-222">✔</span><span class="sxs-lookup"><span data-stu-id="14bf7-222">✔</span></span>
<span data-ttu-id="14bf7-223">Geometry.Buffer (double, int)</span><span class="sxs-lookup"><span data-stu-id="14bf7-223">Geometry.Buffer(double, int)</span></span> | | | <span data-ttu-id="14bf7-224">✔</span><span class="sxs-lookup"><span data-stu-id="14bf7-224">✔</span></span>
<span data-ttu-id="14bf7-225">Geometry.Centroid</span><span class="sxs-lookup"><span data-stu-id="14bf7-225">Geometry.Centroid</span></span> | <span data-ttu-id="14bf7-226">✔</span><span class="sxs-lookup"><span data-stu-id="14bf7-226">✔</span></span> | | <span data-ttu-id="14bf7-227">✔</span><span class="sxs-lookup"><span data-stu-id="14bf7-227">✔</span></span> | <span data-ttu-id="14bf7-228">✔</span><span class="sxs-lookup"><span data-stu-id="14bf7-228">✔</span></span>
<span data-ttu-id="14bf7-229">Geometry.Contains(Geometry)</span><span class="sxs-lookup"><span data-stu-id="14bf7-229">Geometry.Contains(Geometry)</span></span> | <span data-ttu-id="14bf7-230">✔</span><span class="sxs-lookup"><span data-stu-id="14bf7-230">✔</span></span> | <span data-ttu-id="14bf7-231">✔</span><span class="sxs-lookup"><span data-stu-id="14bf7-231">✔</span></span> | <span data-ttu-id="14bf7-232">✔</span><span class="sxs-lookup"><span data-stu-id="14bf7-232">✔</span></span> | <span data-ttu-id="14bf7-233">✔</span><span class="sxs-lookup"><span data-stu-id="14bf7-233">✔</span></span>
<span data-ttu-id="14bf7-234">Geometry.ConvexHull()</span><span class="sxs-lookup"><span data-stu-id="14bf7-234">Geometry.ConvexHull()</span></span> | <span data-ttu-id="14bf7-235">✔</span><span class="sxs-lookup"><span data-stu-id="14bf7-235">✔</span></span> | <span data-ttu-id="14bf7-236">✔</span><span class="sxs-lookup"><span data-stu-id="14bf7-236">✔</span></span> | <span data-ttu-id="14bf7-237">✔</span><span class="sxs-lookup"><span data-stu-id="14bf7-237">✔</span></span> | <span data-ttu-id="14bf7-238">✔</span><span class="sxs-lookup"><span data-stu-id="14bf7-238">✔</span></span>
<span data-ttu-id="14bf7-239">Geometry.CoveredBy(Geometry)</span><span class="sxs-lookup"><span data-stu-id="14bf7-239">Geometry.CoveredBy(Geometry)</span></span> | | | <span data-ttu-id="14bf7-240">✔</span><span class="sxs-lookup"><span data-stu-id="14bf7-240">✔</span></span> | <span data-ttu-id="14bf7-241">✔</span><span class="sxs-lookup"><span data-stu-id="14bf7-241">✔</span></span>
<span data-ttu-id="14bf7-242">Geometry.Covers(Geometry)</span><span class="sxs-lookup"><span data-stu-id="14bf7-242">Geometry.Covers(Geometry)</span></span> | | | <span data-ttu-id="14bf7-243">✔</span><span class="sxs-lookup"><span data-stu-id="14bf7-243">✔</span></span> | <span data-ttu-id="14bf7-244">✔</span><span class="sxs-lookup"><span data-stu-id="14bf7-244">✔</span></span>
<span data-ttu-id="14bf7-245">Geometry.Crosses(Geometry)</span><span class="sxs-lookup"><span data-stu-id="14bf7-245">Geometry.Crosses(Geometry)</span></span> | <span data-ttu-id="14bf7-246">✔</span><span class="sxs-lookup"><span data-stu-id="14bf7-246">✔</span></span> | | <span data-ttu-id="14bf7-247">✔</span><span class="sxs-lookup"><span data-stu-id="14bf7-247">✔</span></span> | <span data-ttu-id="14bf7-248">✔</span><span class="sxs-lookup"><span data-stu-id="14bf7-248">✔</span></span>
<span data-ttu-id="14bf7-249">Geometry.Difference(Geometry)</span><span class="sxs-lookup"><span data-stu-id="14bf7-249">Geometry.Difference(Geometry)</span></span> | <span data-ttu-id="14bf7-250">✔</span><span class="sxs-lookup"><span data-stu-id="14bf7-250">✔</span></span> | <span data-ttu-id="14bf7-251">✔</span><span class="sxs-lookup"><span data-stu-id="14bf7-251">✔</span></span> | <span data-ttu-id="14bf7-252">✔</span><span class="sxs-lookup"><span data-stu-id="14bf7-252">✔</span></span> | <span data-ttu-id="14bf7-253">✔</span><span class="sxs-lookup"><span data-stu-id="14bf7-253">✔</span></span>
<span data-ttu-id="14bf7-254">Geometry.Dimension</span><span class="sxs-lookup"><span data-stu-id="14bf7-254">Geometry.Dimension</span></span> | <span data-ttu-id="14bf7-255">✔</span><span class="sxs-lookup"><span data-stu-id="14bf7-255">✔</span></span> | <span data-ttu-id="14bf7-256">✔</span><span class="sxs-lookup"><span data-stu-id="14bf7-256">✔</span></span> | <span data-ttu-id="14bf7-257">✔</span><span class="sxs-lookup"><span data-stu-id="14bf7-257">✔</span></span> | <span data-ttu-id="14bf7-258">✔</span><span class="sxs-lookup"><span data-stu-id="14bf7-258">✔</span></span>
<span data-ttu-id="14bf7-259">Geometry.Disjoint(Geometry)</span><span class="sxs-lookup"><span data-stu-id="14bf7-259">Geometry.Disjoint(Geometry)</span></span> | <span data-ttu-id="14bf7-260">✔</span><span class="sxs-lookup"><span data-stu-id="14bf7-260">✔</span></span> | <span data-ttu-id="14bf7-261">✔</span><span class="sxs-lookup"><span data-stu-id="14bf7-261">✔</span></span> | <span data-ttu-id="14bf7-262">✔</span><span class="sxs-lookup"><span data-stu-id="14bf7-262">✔</span></span> | <span data-ttu-id="14bf7-263">✔</span><span class="sxs-lookup"><span data-stu-id="14bf7-263">✔</span></span>
<span data-ttu-id="14bf7-264">Geometry.Distance(Geometry)</span><span class="sxs-lookup"><span data-stu-id="14bf7-264">Geometry.Distance(Geometry)</span></span> | <span data-ttu-id="14bf7-265">✔</span><span class="sxs-lookup"><span data-stu-id="14bf7-265">✔</span></span> | <span data-ttu-id="14bf7-266">✔</span><span class="sxs-lookup"><span data-stu-id="14bf7-266">✔</span></span> | <span data-ttu-id="14bf7-267">✔</span><span class="sxs-lookup"><span data-stu-id="14bf7-267">✔</span></span> | <span data-ttu-id="14bf7-268">✔</span><span class="sxs-lookup"><span data-stu-id="14bf7-268">✔</span></span>
<span data-ttu-id="14bf7-269">Geometry.Envelope</span><span class="sxs-lookup"><span data-stu-id="14bf7-269">Geometry.Envelope</span></span> | <span data-ttu-id="14bf7-270">✔</span><span class="sxs-lookup"><span data-stu-id="14bf7-270">✔</span></span> | | <span data-ttu-id="14bf7-271">✔</span><span class="sxs-lookup"><span data-stu-id="14bf7-271">✔</span></span> | <span data-ttu-id="14bf7-272">✔</span><span class="sxs-lookup"><span data-stu-id="14bf7-272">✔</span></span>
<span data-ttu-id="14bf7-273">Geometry.EqualsExact(Geometry)</span><span class="sxs-lookup"><span data-stu-id="14bf7-273">Geometry.EqualsExact(Geometry)</span></span> | | | | <span data-ttu-id="14bf7-274">✔</span><span class="sxs-lookup"><span data-stu-id="14bf7-274">✔</span></span>
<span data-ttu-id="14bf7-275">Geometry.EqualsTopologically(Geometry)</span><span class="sxs-lookup"><span data-stu-id="14bf7-275">Geometry.EqualsTopologically(Geometry)</span></span> | <span data-ttu-id="14bf7-276">✔</span><span class="sxs-lookup"><span data-stu-id="14bf7-276">✔</span></span> | <span data-ttu-id="14bf7-277">✔</span><span class="sxs-lookup"><span data-stu-id="14bf7-277">✔</span></span> | <span data-ttu-id="14bf7-278">✔</span><span class="sxs-lookup"><span data-stu-id="14bf7-278">✔</span></span> | <span data-ttu-id="14bf7-279">✔</span><span class="sxs-lookup"><span data-stu-id="14bf7-279">✔</span></span>
<span data-ttu-id="14bf7-280">Geometry.GeometryType</span><span class="sxs-lookup"><span data-stu-id="14bf7-280">Geometry.GeometryType</span></span> | <span data-ttu-id="14bf7-281">✔</span><span class="sxs-lookup"><span data-stu-id="14bf7-281">✔</span></span> | <span data-ttu-id="14bf7-282">✔</span><span class="sxs-lookup"><span data-stu-id="14bf7-282">✔</span></span> | <span data-ttu-id="14bf7-283">✔</span><span class="sxs-lookup"><span data-stu-id="14bf7-283">✔</span></span> | <span data-ttu-id="14bf7-284">✔</span><span class="sxs-lookup"><span data-stu-id="14bf7-284">✔</span></span>
<span data-ttu-id="14bf7-285">Geometry.GetGeometryN(int)</span><span class="sxs-lookup"><span data-stu-id="14bf7-285">Geometry.GetGeometryN(int)</span></span> | <span data-ttu-id="14bf7-286">✔</span><span class="sxs-lookup"><span data-stu-id="14bf7-286">✔</span></span> | | <span data-ttu-id="14bf7-287">✔</span><span class="sxs-lookup"><span data-stu-id="14bf7-287">✔</span></span> | <span data-ttu-id="14bf7-288">✔</span><span class="sxs-lookup"><span data-stu-id="14bf7-288">✔</span></span>
<span data-ttu-id="14bf7-289">Geometry.InteriorPoint</span><span class="sxs-lookup"><span data-stu-id="14bf7-289">Geometry.InteriorPoint</span></span> | <span data-ttu-id="14bf7-290">✔</span><span class="sxs-lookup"><span data-stu-id="14bf7-290">✔</span></span> | | <span data-ttu-id="14bf7-291">✔</span><span class="sxs-lookup"><span data-stu-id="14bf7-291">✔</span></span>
<span data-ttu-id="14bf7-292">Geometry.Intersection(Geometry)</span><span class="sxs-lookup"><span data-stu-id="14bf7-292">Geometry.Intersection(Geometry)</span></span> | <span data-ttu-id="14bf7-293">✔</span><span class="sxs-lookup"><span data-stu-id="14bf7-293">✔</span></span> | <span data-ttu-id="14bf7-294">✔</span><span class="sxs-lookup"><span data-stu-id="14bf7-294">✔</span></span> | <span data-ttu-id="14bf7-295">✔</span><span class="sxs-lookup"><span data-stu-id="14bf7-295">✔</span></span> | <span data-ttu-id="14bf7-296">✔</span><span class="sxs-lookup"><span data-stu-id="14bf7-296">✔</span></span>
<span data-ttu-id="14bf7-297">Geometry.Intersects(Geometry)</span><span class="sxs-lookup"><span data-stu-id="14bf7-297">Geometry.Intersects(Geometry)</span></span> | <span data-ttu-id="14bf7-298">✔</span><span class="sxs-lookup"><span data-stu-id="14bf7-298">✔</span></span> | <span data-ttu-id="14bf7-299">✔</span><span class="sxs-lookup"><span data-stu-id="14bf7-299">✔</span></span> | <span data-ttu-id="14bf7-300">✔</span><span class="sxs-lookup"><span data-stu-id="14bf7-300">✔</span></span> | <span data-ttu-id="14bf7-301">✔</span><span class="sxs-lookup"><span data-stu-id="14bf7-301">✔</span></span>
<span data-ttu-id="14bf7-302">Geometry.IsEmpty</span><span class="sxs-lookup"><span data-stu-id="14bf7-302">Geometry.IsEmpty</span></span> | <span data-ttu-id="14bf7-303">✔</span><span class="sxs-lookup"><span data-stu-id="14bf7-303">✔</span></span> | <span data-ttu-id="14bf7-304">✔</span><span class="sxs-lookup"><span data-stu-id="14bf7-304">✔</span></span> | <span data-ttu-id="14bf7-305">✔</span><span class="sxs-lookup"><span data-stu-id="14bf7-305">✔</span></span> | <span data-ttu-id="14bf7-306">✔</span><span class="sxs-lookup"><span data-stu-id="14bf7-306">✔</span></span>
<span data-ttu-id="14bf7-307">Geometry.IsSimple</span><span class="sxs-lookup"><span data-stu-id="14bf7-307">Geometry.IsSimple</span></span> | <span data-ttu-id="14bf7-308">✔</span><span class="sxs-lookup"><span data-stu-id="14bf7-308">✔</span></span> | | <span data-ttu-id="14bf7-309">✔</span><span class="sxs-lookup"><span data-stu-id="14bf7-309">✔</span></span> | <span data-ttu-id="14bf7-310">✔</span><span class="sxs-lookup"><span data-stu-id="14bf7-310">✔</span></span>
<span data-ttu-id="14bf7-311">Geometry.IsValid</span><span class="sxs-lookup"><span data-stu-id="14bf7-311">Geometry.IsValid</span></span> | <span data-ttu-id="14bf7-312">✔</span><span class="sxs-lookup"><span data-stu-id="14bf7-312">✔</span></span> | <span data-ttu-id="14bf7-313">✔</span><span class="sxs-lookup"><span data-stu-id="14bf7-313">✔</span></span> | <span data-ttu-id="14bf7-314">✔</span><span class="sxs-lookup"><span data-stu-id="14bf7-314">✔</span></span> | <span data-ttu-id="14bf7-315">✔</span><span class="sxs-lookup"><span data-stu-id="14bf7-315">✔</span></span>
<span data-ttu-id="14bf7-316">Geometry.IsWithinDistance (Geometry, double)</span><span class="sxs-lookup"><span data-stu-id="14bf7-316">Geometry.IsWithinDistance(Geometry, double)</span></span> | <span data-ttu-id="14bf7-317">✔</span><span class="sxs-lookup"><span data-stu-id="14bf7-317">✔</span></span> | | <span data-ttu-id="14bf7-318">✔</span><span class="sxs-lookup"><span data-stu-id="14bf7-318">✔</span></span>
<span data-ttu-id="14bf7-319">Geometry.Length</span><span class="sxs-lookup"><span data-stu-id="14bf7-319">Geometry.Length</span></span> | <span data-ttu-id="14bf7-320">✔</span><span class="sxs-lookup"><span data-stu-id="14bf7-320">✔</span></span> | <span data-ttu-id="14bf7-321">✔</span><span class="sxs-lookup"><span data-stu-id="14bf7-321">✔</span></span> | <span data-ttu-id="14bf7-322">✔</span><span class="sxs-lookup"><span data-stu-id="14bf7-322">✔</span></span> | <span data-ttu-id="14bf7-323">✔</span><span class="sxs-lookup"><span data-stu-id="14bf7-323">✔</span></span>
<span data-ttu-id="14bf7-324">Geometry.NumGeometries</span><span class="sxs-lookup"><span data-stu-id="14bf7-324">Geometry.NumGeometries</span></span> | <span data-ttu-id="14bf7-325">✔</span><span class="sxs-lookup"><span data-stu-id="14bf7-325">✔</span></span> | <span data-ttu-id="14bf7-326">✔</span><span class="sxs-lookup"><span data-stu-id="14bf7-326">✔</span></span> | <span data-ttu-id="14bf7-327">✔</span><span class="sxs-lookup"><span data-stu-id="14bf7-327">✔</span></span> | <span data-ttu-id="14bf7-328">✔</span><span class="sxs-lookup"><span data-stu-id="14bf7-328">✔</span></span>
<span data-ttu-id="14bf7-329">Geometry.NumPoints</span><span class="sxs-lookup"><span data-stu-id="14bf7-329">Geometry.NumPoints</span></span> | <span data-ttu-id="14bf7-330">✔</span><span class="sxs-lookup"><span data-stu-id="14bf7-330">✔</span></span> | <span data-ttu-id="14bf7-331">✔</span><span class="sxs-lookup"><span data-stu-id="14bf7-331">✔</span></span> | <span data-ttu-id="14bf7-332">✔</span><span class="sxs-lookup"><span data-stu-id="14bf7-332">✔</span></span> | <span data-ttu-id="14bf7-333">✔</span><span class="sxs-lookup"><span data-stu-id="14bf7-333">✔</span></span>
<span data-ttu-id="14bf7-334">Geometry.OgcGeometryType</span><span class="sxs-lookup"><span data-stu-id="14bf7-334">Geometry.OgcGeometryType</span></span> | <span data-ttu-id="14bf7-335">✔</span><span class="sxs-lookup"><span data-stu-id="14bf7-335">✔</span></span> | <span data-ttu-id="14bf7-336">✔</span><span class="sxs-lookup"><span data-stu-id="14bf7-336">✔</span></span> | <span data-ttu-id="14bf7-337">✔</span><span class="sxs-lookup"><span data-stu-id="14bf7-337">✔</span></span>
<span data-ttu-id="14bf7-338">Geometry.Overlaps(Geometry)</span><span class="sxs-lookup"><span data-stu-id="14bf7-338">Geometry.Overlaps(Geometry)</span></span> | <span data-ttu-id="14bf7-339">✔</span><span class="sxs-lookup"><span data-stu-id="14bf7-339">✔</span></span> | <span data-ttu-id="14bf7-340">✔</span><span class="sxs-lookup"><span data-stu-id="14bf7-340">✔</span></span> | <span data-ttu-id="14bf7-341">✔</span><span class="sxs-lookup"><span data-stu-id="14bf7-341">✔</span></span> | <span data-ttu-id="14bf7-342">✔</span><span class="sxs-lookup"><span data-stu-id="14bf7-342">✔</span></span>
<span data-ttu-id="14bf7-343">Geometry.PointOnSurface</span><span class="sxs-lookup"><span data-stu-id="14bf7-343">Geometry.PointOnSurface</span></span> | <span data-ttu-id="14bf7-344">✔</span><span class="sxs-lookup"><span data-stu-id="14bf7-344">✔</span></span> | | <span data-ttu-id="14bf7-345">✔</span><span class="sxs-lookup"><span data-stu-id="14bf7-345">✔</span></span> | <span data-ttu-id="14bf7-346">✔</span><span class="sxs-lookup"><span data-stu-id="14bf7-346">✔</span></span>
<span data-ttu-id="14bf7-347">Geometry.Relate (Geometry, chaîne)</span><span class="sxs-lookup"><span data-stu-id="14bf7-347">Geometry.Relate(Geometry, string)</span></span> | <span data-ttu-id="14bf7-348">✔</span><span class="sxs-lookup"><span data-stu-id="14bf7-348">✔</span></span> | | <span data-ttu-id="14bf7-349">✔</span><span class="sxs-lookup"><span data-stu-id="14bf7-349">✔</span></span> | <span data-ttu-id="14bf7-350">✔</span><span class="sxs-lookup"><span data-stu-id="14bf7-350">✔</span></span>
<span data-ttu-id="14bf7-351">Geometry.Reverse()</span><span class="sxs-lookup"><span data-stu-id="14bf7-351">Geometry.Reverse()</span></span> | | | <span data-ttu-id="14bf7-352">✔</span><span class="sxs-lookup"><span data-stu-id="14bf7-352">✔</span></span> | <span data-ttu-id="14bf7-353">✔</span><span class="sxs-lookup"><span data-stu-id="14bf7-353">✔</span></span>
<span data-ttu-id="14bf7-354">Geometry.SRID</span><span class="sxs-lookup"><span data-stu-id="14bf7-354">Geometry.SRID</span></span> | <span data-ttu-id="14bf7-355">✔</span><span class="sxs-lookup"><span data-stu-id="14bf7-355">✔</span></span> | <span data-ttu-id="14bf7-356">✔</span><span class="sxs-lookup"><span data-stu-id="14bf7-356">✔</span></span> | <span data-ttu-id="14bf7-357">✔</span><span class="sxs-lookup"><span data-stu-id="14bf7-357">✔</span></span> | <span data-ttu-id="14bf7-358">✔</span><span class="sxs-lookup"><span data-stu-id="14bf7-358">✔</span></span>
<span data-ttu-id="14bf7-359">Geometry.SymmetricDifference(Geometry)</span><span class="sxs-lookup"><span data-stu-id="14bf7-359">Geometry.SymmetricDifference(Geometry)</span></span> | <span data-ttu-id="14bf7-360">✔</span><span class="sxs-lookup"><span data-stu-id="14bf7-360">✔</span></span> | <span data-ttu-id="14bf7-361">✔</span><span class="sxs-lookup"><span data-stu-id="14bf7-361">✔</span></span> | <span data-ttu-id="14bf7-362">✔</span><span class="sxs-lookup"><span data-stu-id="14bf7-362">✔</span></span> | <span data-ttu-id="14bf7-363">✔</span><span class="sxs-lookup"><span data-stu-id="14bf7-363">✔</span></span>
<span data-ttu-id="14bf7-364">Geometry.ToBinary()</span><span class="sxs-lookup"><span data-stu-id="14bf7-364">Geometry.ToBinary()</span></span> | <span data-ttu-id="14bf7-365">✔</span><span class="sxs-lookup"><span data-stu-id="14bf7-365">✔</span></span> | <span data-ttu-id="14bf7-366">✔</span><span class="sxs-lookup"><span data-stu-id="14bf7-366">✔</span></span> | <span data-ttu-id="14bf7-367">✔</span><span class="sxs-lookup"><span data-stu-id="14bf7-367">✔</span></span> | <span data-ttu-id="14bf7-368">✔</span><span class="sxs-lookup"><span data-stu-id="14bf7-368">✔</span></span>
<span data-ttu-id="14bf7-369">Geometry.ToText()</span><span class="sxs-lookup"><span data-stu-id="14bf7-369">Geometry.ToText()</span></span> | <span data-ttu-id="14bf7-370">✔</span><span class="sxs-lookup"><span data-stu-id="14bf7-370">✔</span></span> | <span data-ttu-id="14bf7-371">✔</span><span class="sxs-lookup"><span data-stu-id="14bf7-371">✔</span></span> | <span data-ttu-id="14bf7-372">✔</span><span class="sxs-lookup"><span data-stu-id="14bf7-372">✔</span></span> | <span data-ttu-id="14bf7-373">✔</span><span class="sxs-lookup"><span data-stu-id="14bf7-373">✔</span></span>
<span data-ttu-id="14bf7-374">Geometry.Touches(Geometry)</span><span class="sxs-lookup"><span data-stu-id="14bf7-374">Geometry.Touches(Geometry)</span></span> | <span data-ttu-id="14bf7-375">✔</span><span class="sxs-lookup"><span data-stu-id="14bf7-375">✔</span></span> | | <span data-ttu-id="14bf7-376">✔</span><span class="sxs-lookup"><span data-stu-id="14bf7-376">✔</span></span> | <span data-ttu-id="14bf7-377">✔</span><span class="sxs-lookup"><span data-stu-id="14bf7-377">✔</span></span>
<span data-ttu-id="14bf7-378">Geometry.Union()</span><span class="sxs-lookup"><span data-stu-id="14bf7-378">Geometry.Union()</span></span> | | | <span data-ttu-id="14bf7-379">✔</span><span class="sxs-lookup"><span data-stu-id="14bf7-379">✔</span></span>
<span data-ttu-id="14bf7-380">Geometry.Union(Geometry)</span><span class="sxs-lookup"><span data-stu-id="14bf7-380">Geometry.Union(Geometry)</span></span> | <span data-ttu-id="14bf7-381">✔</span><span class="sxs-lookup"><span data-stu-id="14bf7-381">✔</span></span> | <span data-ttu-id="14bf7-382">✔</span><span class="sxs-lookup"><span data-stu-id="14bf7-382">✔</span></span> | <span data-ttu-id="14bf7-383">✔</span><span class="sxs-lookup"><span data-stu-id="14bf7-383">✔</span></span> | <span data-ttu-id="14bf7-384">✔</span><span class="sxs-lookup"><span data-stu-id="14bf7-384">✔</span></span>
<span data-ttu-id="14bf7-385">Geometry.Within(Geometry)</span><span class="sxs-lookup"><span data-stu-id="14bf7-385">Geometry.Within(Geometry)</span></span> | <span data-ttu-id="14bf7-386">✔</span><span class="sxs-lookup"><span data-stu-id="14bf7-386">✔</span></span> | <span data-ttu-id="14bf7-387">✔</span><span class="sxs-lookup"><span data-stu-id="14bf7-387">✔</span></span> | <span data-ttu-id="14bf7-388">✔</span><span class="sxs-lookup"><span data-stu-id="14bf7-388">✔</span></span> | <span data-ttu-id="14bf7-389">✔</span><span class="sxs-lookup"><span data-stu-id="14bf7-389">✔</span></span>
<span data-ttu-id="14bf7-390">GeometryCollection.Count</span><span class="sxs-lookup"><span data-stu-id="14bf7-390">GeometryCollection.Count</span></span> | <span data-ttu-id="14bf7-391">✔</span><span class="sxs-lookup"><span data-stu-id="14bf7-391">✔</span></span> | <span data-ttu-id="14bf7-392">✔</span><span class="sxs-lookup"><span data-stu-id="14bf7-392">✔</span></span> | <span data-ttu-id="14bf7-393">✔</span><span class="sxs-lookup"><span data-stu-id="14bf7-393">✔</span></span> | <span data-ttu-id="14bf7-394">✔</span><span class="sxs-lookup"><span data-stu-id="14bf7-394">✔</span></span>
<span data-ttu-id="14bf7-395">GeometryCollection [int]</span><span class="sxs-lookup"><span data-stu-id="14bf7-395">GeometryCollection[int]</span></span> | <span data-ttu-id="14bf7-396">✔</span><span class="sxs-lookup"><span data-stu-id="14bf7-396">✔</span></span> | <span data-ttu-id="14bf7-397">✔</span><span class="sxs-lookup"><span data-stu-id="14bf7-397">✔</span></span> | <span data-ttu-id="14bf7-398">✔</span><span class="sxs-lookup"><span data-stu-id="14bf7-398">✔</span></span> | <span data-ttu-id="14bf7-399">✔</span><span class="sxs-lookup"><span data-stu-id="14bf7-399">✔</span></span>
<span data-ttu-id="14bf7-400">LineString.Count</span><span class="sxs-lookup"><span data-stu-id="14bf7-400">LineString.Count</span></span> | <span data-ttu-id="14bf7-401">✔</span><span class="sxs-lookup"><span data-stu-id="14bf7-401">✔</span></span> | <span data-ttu-id="14bf7-402">✔</span><span class="sxs-lookup"><span data-stu-id="14bf7-402">✔</span></span> | <span data-ttu-id="14bf7-403">✔</span><span class="sxs-lookup"><span data-stu-id="14bf7-403">✔</span></span> | <span data-ttu-id="14bf7-404">✔</span><span class="sxs-lookup"><span data-stu-id="14bf7-404">✔</span></span>
<span data-ttu-id="14bf7-405">LineString.EndPoint</span><span class="sxs-lookup"><span data-stu-id="14bf7-405">LineString.EndPoint</span></span> | <span data-ttu-id="14bf7-406">✔</span><span class="sxs-lookup"><span data-stu-id="14bf7-406">✔</span></span> | <span data-ttu-id="14bf7-407">✔</span><span class="sxs-lookup"><span data-stu-id="14bf7-407">✔</span></span> | <span data-ttu-id="14bf7-408">✔</span><span class="sxs-lookup"><span data-stu-id="14bf7-408">✔</span></span> | <span data-ttu-id="14bf7-409">✔</span><span class="sxs-lookup"><span data-stu-id="14bf7-409">✔</span></span>
<span data-ttu-id="14bf7-410">LineString.GetPointN(int)</span><span class="sxs-lookup"><span data-stu-id="14bf7-410">LineString.GetPointN(int)</span></span> | <span data-ttu-id="14bf7-411">✔</span><span class="sxs-lookup"><span data-stu-id="14bf7-411">✔</span></span> | <span data-ttu-id="14bf7-412">✔</span><span class="sxs-lookup"><span data-stu-id="14bf7-412">✔</span></span> | <span data-ttu-id="14bf7-413">✔</span><span class="sxs-lookup"><span data-stu-id="14bf7-413">✔</span></span> | <span data-ttu-id="14bf7-414">✔</span><span class="sxs-lookup"><span data-stu-id="14bf7-414">✔</span></span>
<span data-ttu-id="14bf7-415">LineString.IsClosed</span><span class="sxs-lookup"><span data-stu-id="14bf7-415">LineString.IsClosed</span></span> | <span data-ttu-id="14bf7-416">✔</span><span class="sxs-lookup"><span data-stu-id="14bf7-416">✔</span></span> | <span data-ttu-id="14bf7-417">✔</span><span class="sxs-lookup"><span data-stu-id="14bf7-417">✔</span></span> | <span data-ttu-id="14bf7-418">✔</span><span class="sxs-lookup"><span data-stu-id="14bf7-418">✔</span></span> | <span data-ttu-id="14bf7-419">✔</span><span class="sxs-lookup"><span data-stu-id="14bf7-419">✔</span></span>
<span data-ttu-id="14bf7-420">LineString.IsRing</span><span class="sxs-lookup"><span data-stu-id="14bf7-420">LineString.IsRing</span></span> | <span data-ttu-id="14bf7-421">✔</span><span class="sxs-lookup"><span data-stu-id="14bf7-421">✔</span></span> | | <span data-ttu-id="14bf7-422">✔</span><span class="sxs-lookup"><span data-stu-id="14bf7-422">✔</span></span> | <span data-ttu-id="14bf7-423">✔</span><span class="sxs-lookup"><span data-stu-id="14bf7-423">✔</span></span>
<span data-ttu-id="14bf7-424">LineString.StartPoint</span><span class="sxs-lookup"><span data-stu-id="14bf7-424">LineString.StartPoint</span></span> | <span data-ttu-id="14bf7-425">✔</span><span class="sxs-lookup"><span data-stu-id="14bf7-425">✔</span></span> | <span data-ttu-id="14bf7-426">✔</span><span class="sxs-lookup"><span data-stu-id="14bf7-426">✔</span></span> | <span data-ttu-id="14bf7-427">✔</span><span class="sxs-lookup"><span data-stu-id="14bf7-427">✔</span></span> | <span data-ttu-id="14bf7-428">✔</span><span class="sxs-lookup"><span data-stu-id="14bf7-428">✔</span></span>
<span data-ttu-id="14bf7-429">MultiLineString.IsClosed</span><span class="sxs-lookup"><span data-stu-id="14bf7-429">MultiLineString.IsClosed</span></span> | <span data-ttu-id="14bf7-430">✔</span><span class="sxs-lookup"><span data-stu-id="14bf7-430">✔</span></span> | <span data-ttu-id="14bf7-431">✔</span><span class="sxs-lookup"><span data-stu-id="14bf7-431">✔</span></span> | <span data-ttu-id="14bf7-432">✔</span><span class="sxs-lookup"><span data-stu-id="14bf7-432">✔</span></span> | <span data-ttu-id="14bf7-433">✔</span><span class="sxs-lookup"><span data-stu-id="14bf7-433">✔</span></span>
<span data-ttu-id="14bf7-434">Point.M</span><span class="sxs-lookup"><span data-stu-id="14bf7-434">Point.M</span></span> | <span data-ttu-id="14bf7-435">✔</span><span class="sxs-lookup"><span data-stu-id="14bf7-435">✔</span></span> | <span data-ttu-id="14bf7-436">✔</span><span class="sxs-lookup"><span data-stu-id="14bf7-436">✔</span></span> | <span data-ttu-id="14bf7-437">✔</span><span class="sxs-lookup"><span data-stu-id="14bf7-437">✔</span></span> | <span data-ttu-id="14bf7-438">✔</span><span class="sxs-lookup"><span data-stu-id="14bf7-438">✔</span></span>
<span data-ttu-id="14bf7-439">Point.X</span><span class="sxs-lookup"><span data-stu-id="14bf7-439">Point.X</span></span> | <span data-ttu-id="14bf7-440">✔</span><span class="sxs-lookup"><span data-stu-id="14bf7-440">✔</span></span> | <span data-ttu-id="14bf7-441">✔</span><span class="sxs-lookup"><span data-stu-id="14bf7-441">✔</span></span> | <span data-ttu-id="14bf7-442">✔</span><span class="sxs-lookup"><span data-stu-id="14bf7-442">✔</span></span> | <span data-ttu-id="14bf7-443">✔</span><span class="sxs-lookup"><span data-stu-id="14bf7-443">✔</span></span>
<span data-ttu-id="14bf7-444">Point.Y</span><span class="sxs-lookup"><span data-stu-id="14bf7-444">Point.Y</span></span> | <span data-ttu-id="14bf7-445">✔</span><span class="sxs-lookup"><span data-stu-id="14bf7-445">✔</span></span> | <span data-ttu-id="14bf7-446">✔</span><span class="sxs-lookup"><span data-stu-id="14bf7-446">✔</span></span> | <span data-ttu-id="14bf7-447">✔</span><span class="sxs-lookup"><span data-stu-id="14bf7-447">✔</span></span> | <span data-ttu-id="14bf7-448">✔</span><span class="sxs-lookup"><span data-stu-id="14bf7-448">✔</span></span>
<span data-ttu-id="14bf7-449">Point.Z</span><span class="sxs-lookup"><span data-stu-id="14bf7-449">Point.Z</span></span> | <span data-ttu-id="14bf7-450">✔</span><span class="sxs-lookup"><span data-stu-id="14bf7-450">✔</span></span> | <span data-ttu-id="14bf7-451">✔</span><span class="sxs-lookup"><span data-stu-id="14bf7-451">✔</span></span> | <span data-ttu-id="14bf7-452">✔</span><span class="sxs-lookup"><span data-stu-id="14bf7-452">✔</span></span> | <span data-ttu-id="14bf7-453">✔</span><span class="sxs-lookup"><span data-stu-id="14bf7-453">✔</span></span>
<span data-ttu-id="14bf7-454">Polygon.ExteriorRing</span><span class="sxs-lookup"><span data-stu-id="14bf7-454">Polygon.ExteriorRing</span></span> | <span data-ttu-id="14bf7-455">✔</span><span class="sxs-lookup"><span data-stu-id="14bf7-455">✔</span></span> | <span data-ttu-id="14bf7-456">✔</span><span class="sxs-lookup"><span data-stu-id="14bf7-456">✔</span></span> | <span data-ttu-id="14bf7-457">✔</span><span class="sxs-lookup"><span data-stu-id="14bf7-457">✔</span></span> | <span data-ttu-id="14bf7-458">✔</span><span class="sxs-lookup"><span data-stu-id="14bf7-458">✔</span></span>
<span data-ttu-id="14bf7-459">Polygon.GetInteriorRingN(int)</span><span class="sxs-lookup"><span data-stu-id="14bf7-459">Polygon.GetInteriorRingN(int)</span></span> | <span data-ttu-id="14bf7-460">✔</span><span class="sxs-lookup"><span data-stu-id="14bf7-460">✔</span></span> | <span data-ttu-id="14bf7-461">✔</span><span class="sxs-lookup"><span data-stu-id="14bf7-461">✔</span></span> | <span data-ttu-id="14bf7-462">✔</span><span class="sxs-lookup"><span data-stu-id="14bf7-462">✔</span></span> | <span data-ttu-id="14bf7-463">✔</span><span class="sxs-lookup"><span data-stu-id="14bf7-463">✔</span></span>
<span data-ttu-id="14bf7-464">Polygon.NumInteriorRings</span><span class="sxs-lookup"><span data-stu-id="14bf7-464">Polygon.NumInteriorRings</span></span> | <span data-ttu-id="14bf7-465">✔</span><span class="sxs-lookup"><span data-stu-id="14bf7-465">✔</span></span> | <span data-ttu-id="14bf7-466">✔</span><span class="sxs-lookup"><span data-stu-id="14bf7-466">✔</span></span> | <span data-ttu-id="14bf7-467">✔</span><span class="sxs-lookup"><span data-stu-id="14bf7-467">✔</span></span> | <span data-ttu-id="14bf7-468">✔</span><span class="sxs-lookup"><span data-stu-id="14bf7-468">✔</span></span>

## <a name="additional-resources"></a><span data-ttu-id="14bf7-469">Ressources supplémentaires</span><span class="sxs-lookup"><span data-stu-id="14bf7-469">Additional resources</span></span>

* [<span data-ttu-id="14bf7-470">Données spatiales dans SQL Server</span><span class="sxs-lookup"><span data-stu-id="14bf7-470">Spatial Data in SQL Server</span></span>](https://docs.microsoft.com/sql/relational-databases/spatial/spatial-data-sql-server)
* [<span data-ttu-id="14bf7-471">Page d’accueil SpatiaLite</span><span class="sxs-lookup"><span data-stu-id="14bf7-471">SpatiaLite Homepage</span></span>](https://www.gaia-gis.it/fossil/libspatialite)
* [<span data-ttu-id="14bf7-472">Documentation de PostGIS</span><span class="sxs-lookup"><span data-stu-id="14bf7-472">PostGIS Documentation</span></span>](http://postgis.net/documentation/)
