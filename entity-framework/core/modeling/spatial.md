---
title: Données spatiales-EF Core
author: bricelam
ms.author: bricelam
ms.date: 11/01/2018
ms.assetid: 2BDE29FC-4161-41A0-841E-69F51CCD9341
uid: core/modeling/spatial
ms.openlocfilehash: 335d4f3a601624f7c994b7dcacefe4ef6798beb3
ms.sourcegitcommit: 18ab4c349473d94b15b4ca977df12147db07b77f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/06/2019
ms.locfileid: "73655610"
---
# <a name="spatial-data"></a><span data-ttu-id="d54db-102">Données spatiales</span><span class="sxs-lookup"><span data-stu-id="d54db-102">Spatial Data</span></span>

> [!NOTE]
> <span data-ttu-id="d54db-103">Cette fonctionnalité a été ajoutée dans EF Core 2,2.</span><span class="sxs-lookup"><span data-stu-id="d54db-103">This feature was added in EF Core 2.2.</span></span>

<span data-ttu-id="d54db-104">Les données spatiales représentent l’emplacement physique et la forme des objets.</span><span class="sxs-lookup"><span data-stu-id="d54db-104">Spatial data represents the physical location and the shape of objects.</span></span> <span data-ttu-id="d54db-105">De nombreuses bases de données prennent en charge ce type de données afin qu’elles puissent être indexées et interrogées avec d’autres données.</span><span class="sxs-lookup"><span data-stu-id="d54db-105">Many databases provide support for this type of data so it can be indexed and queried alongside other data.</span></span> <span data-ttu-id="d54db-106">Les scénarios courants incluent l’interrogation d’objets situés à une distance donnée à partir d’un emplacement ou la sélection de l’objet dont la bordure contient un emplacement donné.</span><span class="sxs-lookup"><span data-stu-id="d54db-106">Common scenarios include querying for objects within a given distance from a location, or selecting the object whose border contains a given location.</span></span> <span data-ttu-id="d54db-107">EF Core prend en charge le mappage aux types de données spatiales à l’aide de la bibliothèque spatiale [NetTopologySuite](https://github.com/NetTopologySuite/NetTopologySuite) .</span><span class="sxs-lookup"><span data-stu-id="d54db-107">EF Core supports mapping to spatial data types using the [NetTopologySuite](https://github.com/NetTopologySuite/NetTopologySuite) spatial library.</span></span>

## <a name="installing"></a><span data-ttu-id="d54db-108">Installation de</span><span class="sxs-lookup"><span data-stu-id="d54db-108">Installing</span></span>

<span data-ttu-id="d54db-109">Pour pouvoir utiliser les données spatiales avec EF Core, vous devez installer le package NuGet de prise en charge approprié.</span><span class="sxs-lookup"><span data-stu-id="d54db-109">In order to use spatial data with EF Core, you need to install the appropriate supporting NuGet package.</span></span> <span data-ttu-id="d54db-110">Le package que vous devez installer dépend du fournisseur que vous utilisez.</span><span class="sxs-lookup"><span data-stu-id="d54db-110">Which package you need to install depends on the provider you're using.</span></span>

<span data-ttu-id="d54db-111">Fournisseur EF Core</span><span class="sxs-lookup"><span data-stu-id="d54db-111">EF Core Provider</span></span>                        | <span data-ttu-id="d54db-112">Package NuGet spatial</span><span class="sxs-lookup"><span data-stu-id="d54db-112">Spatial NuGet Package</span></span>
--------------------------------------- | ---------------------
<span data-ttu-id="d54db-113">Microsoft.EntityFrameworkCore.SqlServer</span><span class="sxs-lookup"><span data-stu-id="d54db-113">Microsoft.EntityFrameworkCore.SqlServer</span></span> | [<span data-ttu-id="d54db-114">Microsoft. EntityFrameworkCore. SqlServer. NetTopologySuite</span><span class="sxs-lookup"><span data-stu-id="d54db-114">Microsoft.EntityFrameworkCore.SqlServer.NetTopologySuite</span></span>](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.SqlServer.NetTopologySuite)
<span data-ttu-id="d54db-115">Microsoft.EntityFrameworkCore.Sqlite</span><span class="sxs-lookup"><span data-stu-id="d54db-115">Microsoft.EntityFrameworkCore.Sqlite</span></span>    | [<span data-ttu-id="d54db-116">Microsoft. EntityFrameworkCore. sqlite. NetTopologySuite</span><span class="sxs-lookup"><span data-stu-id="d54db-116">Microsoft.EntityFrameworkCore.Sqlite.NetTopologySuite</span></span>](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Sqlite.NetTopologySuite)
<span data-ttu-id="d54db-117">Microsoft.EntityFrameworkCore.InMemory</span><span class="sxs-lookup"><span data-stu-id="d54db-117">Microsoft.EntityFrameworkCore.InMemory</span></span>  | [<span data-ttu-id="d54db-118">NetTopologySuite</span><span class="sxs-lookup"><span data-stu-id="d54db-118">NetTopologySuite</span></span>](https://www.nuget.org/packages/NetTopologySuite)
<span data-ttu-id="d54db-119">Npgsql.EntityFrameworkCore.PostgreSQL</span><span class="sxs-lookup"><span data-stu-id="d54db-119">Npgsql.EntityFrameworkCore.PostgreSQL</span></span>   | [<span data-ttu-id="d54db-120">Npgsql. EntityFrameworkCore. PostgreSQL. NetTopologySuite</span><span class="sxs-lookup"><span data-stu-id="d54db-120">Npgsql.EntityFrameworkCore.PostgreSQL.NetTopologySuite</span></span>](https://www.nuget.org/packages/Npgsql.EntityFrameworkCore.PostgreSQL.NetTopologySuite)

## <a name="reverse-engineering"></a><span data-ttu-id="d54db-121">Rétroconception</span><span class="sxs-lookup"><span data-stu-id="d54db-121">Reverse engineering</span></span>

<span data-ttu-id="d54db-122">Les packages NuGet spatiaux activent également les modèles d' [ingénierie à rebours](../managing-schemas/scaffolding.md) avec des propriétés spatiales, mais vous devez installer le package ***avant*** d’exécuter `Scaffold-DbContext` ou `dotnet ef dbcontext scaffold`.</span><span class="sxs-lookup"><span data-stu-id="d54db-122">The spatial NuGet packages also enable [reverse engineering](../managing-schemas/scaffolding.md) models with spatial properties, but you need to install the package ***before*** running `Scaffold-DbContext` or `dotnet ef dbcontext scaffold`.</span></span> <span data-ttu-id="d54db-123">Si vous ne le souhaitez pas, vous recevrez des avertissements sur les mappages de type pour les colonnes et les colonnes seront ignorées.</span><span class="sxs-lookup"><span data-stu-id="d54db-123">If you don't, you'll receive warnings about not finding type mappings for the columns and the columns will be skipped.</span></span>

## <a name="nettopologysuite-nts"></a><span data-ttu-id="d54db-124">NetTopologySuite (NTS)</span><span class="sxs-lookup"><span data-stu-id="d54db-124">NetTopologySuite (NTS)</span></span>

<span data-ttu-id="d54db-125">NetTopologySuite est une bibliothèque spatiale pour .NET.</span><span class="sxs-lookup"><span data-stu-id="d54db-125">NetTopologySuite is a spatial library for .NET.</span></span> <span data-ttu-id="d54db-126">EF Core permet le mappage aux types de données spatiales dans la base de données à l’aide des types NTS dans votre modèle.</span><span class="sxs-lookup"><span data-stu-id="d54db-126">EF Core enables mapping to spatial data types in the database by using NTS types in your model.</span></span>

<span data-ttu-id="d54db-127">Pour activer le mappage aux types spatiaux via NTS, appelez la méthode UseNetTopologySuite sur le générateur d’options DbContext du fournisseur.</span><span class="sxs-lookup"><span data-stu-id="d54db-127">To enable mapping to spatial types via NTS, call the UseNetTopologySuite method on the provider's DbContext options builder.</span></span> <span data-ttu-id="d54db-128">Par exemple, avec SQL Server vous l’appellerez comme ceci.</span><span class="sxs-lookup"><span data-stu-id="d54db-128">For example, with SQL Server you'd call it like this.</span></span>

``` csharp
optionsBuilder.UseSqlServer(
    @"Data Source=(localdb)\MSSQLLocalDB;Initial Catalog=WideWorldImporters",
    x => x.UseNetTopologySuite());
```

<span data-ttu-id="d54db-129">Il existe plusieurs types de données spatiales.</span><span class="sxs-lookup"><span data-stu-id="d54db-129">There are several spatial data types.</span></span> <span data-ttu-id="d54db-130">Le type que vous utilisez dépend des types de formes que vous souhaitez autoriser.</span><span class="sxs-lookup"><span data-stu-id="d54db-130">Which type you use depends on the types of shapes you want to allow.</span></span> <span data-ttu-id="d54db-131">Voici la hiérarchie des types NTS que vous pouvez utiliser pour les propriétés de votre modèle.</span><span class="sxs-lookup"><span data-stu-id="d54db-131">Here is the hierarchy of NTS types that you can use for properties in your model.</span></span> <span data-ttu-id="d54db-132">Ils sont situés dans l’espace de noms `NetTopologySuite.Geometries`.</span><span class="sxs-lookup"><span data-stu-id="d54db-132">They're located within the `NetTopologySuite.Geometries` namespace.</span></span>

* <span data-ttu-id="d54db-133">Geometr</span><span class="sxs-lookup"><span data-stu-id="d54db-133">Geometry</span></span>
  * <span data-ttu-id="d54db-134">Point</span><span class="sxs-lookup"><span data-stu-id="d54db-134">Point</span></span>
  * <span data-ttu-id="d54db-135">LineString</span><span class="sxs-lookup"><span data-stu-id="d54db-135">LineString</span></span>
  * <span data-ttu-id="d54db-136">Polygone</span><span class="sxs-lookup"><span data-stu-id="d54db-136">Polygon</span></span>
  * <span data-ttu-id="d54db-137">GeometryCollection</span><span class="sxs-lookup"><span data-stu-id="d54db-137">GeometryCollection</span></span>
    * <span data-ttu-id="d54db-138">MultiPoint</span><span class="sxs-lookup"><span data-stu-id="d54db-138">MultiPoint</span></span>
    * <span data-ttu-id="d54db-139">MultiLineString</span><span class="sxs-lookup"><span data-stu-id="d54db-139">MultiLineString</span></span>
    * <span data-ttu-id="d54db-140">MultiPolygon</span><span class="sxs-lookup"><span data-stu-id="d54db-140">MultiPolygon</span></span>

> [!WARNING]
> <span data-ttu-id="d54db-141">CircularString, CompoundCurve et CurePolygon ne sont pas pris en charge par NTS.</span><span class="sxs-lookup"><span data-stu-id="d54db-141">CircularString, CompoundCurve, and CurePolygon aren't supported by NTS.</span></span>

<span data-ttu-id="d54db-142">L’utilisation du type Geometry de base permet à n’importe quel type de forme d’être spécifié par la propriété.</span><span class="sxs-lookup"><span data-stu-id="d54db-142">Using the base Geometry type allows any type of shape to be specified by the property.</span></span>

<span data-ttu-id="d54db-143">Les classes d’entité suivantes peuvent être utilisées pour mapper à des tables dans l' [exemple de base de données grand World](https://go.microsoft.com/fwlink/?LinkID=800630)Importers.</span><span class="sxs-lookup"><span data-stu-id="d54db-143">The following entity classes could be used to map to tables in the [Wide World Importers sample database](https://go.microsoft.com/fwlink/?LinkID=800630).</span></span>

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

### <a name="creating-values"></a><span data-ttu-id="d54db-144">Création de valeurs</span><span class="sxs-lookup"><span data-stu-id="d54db-144">Creating values</span></span>

<span data-ttu-id="d54db-145">Vous pouvez utiliser des constructeurs pour créer des objets Geometry ; Toutefois, NTS recommande d’utiliser à la place une fabrique de géométrie.</span><span class="sxs-lookup"><span data-stu-id="d54db-145">You can use constructors to create geometry objects; however, NTS recommends using a geometry factory instead.</span></span> <span data-ttu-id="d54db-146">Cela vous permet de spécifier un SRID par défaut (le système de référence spatiale utilisé par les coordonnées) et vous donne le contrôle sur des éléments plus avancés tels que le modèle de précision (utilisé pendant les calculs) et la séquence de coordonnées (détermine les ordonnées-dimensions mesures et disponibles.</span><span class="sxs-lookup"><span data-stu-id="d54db-146">This lets you specify a default SRID (the spatial reference system used by the coordinates) and gives you control over more advanced things like the precision model (used during calculations) and the coordinate sequence (determines which ordinates--dimensions and measures--are available).</span></span>

``` csharp
var geometryFactory = NtsGeometryServices.Instance.CreateGeometryFactory(srid: 4326);
var currentLocation = geometryFactory.CreatePoint(-122.121512, 47.6739882);
```

> [!NOTE]
> <span data-ttu-id="d54db-147">4326 fait référence à WGS 84, une norme utilisée dans le GPS et d’autres systèmes géographiques.</span><span class="sxs-lookup"><span data-stu-id="d54db-147">4326 refers to WGS 84, a standard used in GPS and other geographic systems.</span></span>

### <a name="longitude-and-latitude"></a><span data-ttu-id="d54db-148">Longitude et Latitude</span><span class="sxs-lookup"><span data-stu-id="d54db-148">Longitude and Latitude</span></span>

<span data-ttu-id="d54db-149">Les coordonnées en NTS sont exprimées en valeurs X et Y.</span><span class="sxs-lookup"><span data-stu-id="d54db-149">Coordinates in NTS are in terms of X and Y values.</span></span> <span data-ttu-id="d54db-150">Pour représenter la longitude et la latitude, utilisez X pour la longitude et Y pour la latitude.</span><span class="sxs-lookup"><span data-stu-id="d54db-150">To represent longitude and latitude, use X for longitude and Y for latitude.</span></span> <span data-ttu-id="d54db-151">Notez que **c’est à partir du format** `latitude, longitude` dans lequel vous voyez généralement ces valeurs.</span><span class="sxs-lookup"><span data-stu-id="d54db-151">Note that this is **backwards** from the `latitude, longitude` format in which you typically see these values.</span></span>

### <a name="srid-ignored-during-client-operations"></a><span data-ttu-id="d54db-152">SRID ignoré pendant les opérations du client</span><span class="sxs-lookup"><span data-stu-id="d54db-152">SRID Ignored during client operations</span></span>

<span data-ttu-id="d54db-153">NTS ignore les valeurs SRID lors des opérations.</span><span class="sxs-lookup"><span data-stu-id="d54db-153">NTS ignores SRID values during operations.</span></span> <span data-ttu-id="d54db-154">Il part du principe qu’il s’agit d’un système de coordonnées planaire.</span><span class="sxs-lookup"><span data-stu-id="d54db-154">It assumes a planar coordinate system.</span></span> <span data-ttu-id="d54db-155">Cela signifie que si vous spécifiez des coordonnées en termes de longitude et de latitude, certaines valeurs évaluées par le client, telles que la distance, la longueur et la zone, seront en degrés, et non en mètres.</span><span class="sxs-lookup"><span data-stu-id="d54db-155">This means that if you specify coordinates in terms of longitude and latitude, some client-evaluated values like distance, length, and area will be in degrees, not meters.</span></span> <span data-ttu-id="d54db-156">Pour les valeurs plus significatives, vous devez d’abord projeter les coordonnées dans un autre système de coordonnées à l’aide d’une bibliothèque comme [ProjNet4GeoAPI](https://github.com/NetTopologySuite/ProjNet4GeoAPI) avant de calculer ces valeurs.</span><span class="sxs-lookup"><span data-stu-id="d54db-156">For more meaningful values, you first need to project the coordinates to another coordinate system using a library like [ProjNet4GeoAPI](https://github.com/NetTopologySuite/ProjNet4GeoAPI) before calculating these values.</span></span>

<span data-ttu-id="d54db-157">Si une opération est évaluée par le serveur par EF Core via SQL, l’unité du résultat est déterminée par la base de données.</span><span class="sxs-lookup"><span data-stu-id="d54db-157">If an operation is server-evaluated by EF Core via SQL, the result's unit will be determined by the database.</span></span>

<span data-ttu-id="d54db-158">Voici un exemple d’utilisation de ProjNet4GeoAPI pour calculer la distance entre deux villes.</span><span class="sxs-lookup"><span data-stu-id="d54db-158">Here is an example of using ProjNet4GeoAPI to calculate the distance between two cities.</span></span>

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

## <a name="querying-data"></a><span data-ttu-id="d54db-159">Interrogation des données</span><span class="sxs-lookup"><span data-stu-id="d54db-159">Querying Data</span></span>

<span data-ttu-id="d54db-160">Dans LINQ, les méthodes et propriétés NTS disponibles en tant que fonctions de base de données sont traduites en SQL.</span><span class="sxs-lookup"><span data-stu-id="d54db-160">In LINQ, the NTS methods and properties available as database functions will be translated to SQL.</span></span> <span data-ttu-id="d54db-161">Par exemple, les méthodes distance et Contains sont traduites dans les requêtes suivantes.</span><span class="sxs-lookup"><span data-stu-id="d54db-161">For example, the Distance and Contains methods are translated in the following queries.</span></span> <span data-ttu-id="d54db-162">Le tableau à la fin de cet article indique quels membres sont pris en charge par différents fournisseurs de EF Core.</span><span class="sxs-lookup"><span data-stu-id="d54db-162">The table at the end of this article shows which members are supported by various EF Core providers.</span></span>

``` csharp
var nearestCity = db.Cities
    .OrderBy(c => c.Location.Distance(currentLocation))
    .FirstOrDefault();

var currentCountry = db.Countries
    .FirstOrDefault(c => c.Border.Contains(currentLocation));
```

## <a name="sql-server"></a><span data-ttu-id="d54db-163">SQL Server</span><span class="sxs-lookup"><span data-stu-id="d54db-163">SQL Server</span></span>

<span data-ttu-id="d54db-164">Si vous utilisez SQL Server, vous devez connaître certaines choses supplémentaires.</span><span class="sxs-lookup"><span data-stu-id="d54db-164">If you're using SQL Server, there are some additional things you should be aware of.</span></span>

### <a name="geography-or-geometry"></a><span data-ttu-id="d54db-165">Geography ou Geometry</span><span class="sxs-lookup"><span data-stu-id="d54db-165">Geography or geometry</span></span>

<span data-ttu-id="d54db-166">Par défaut, les propriétés spatiales sont mappées à des colonnes `geography` dans SQL Server.</span><span class="sxs-lookup"><span data-stu-id="d54db-166">By default, spatial properties are mapped to `geography` columns in SQL Server.</span></span> <span data-ttu-id="d54db-167">Pour utiliser `geometry`, [configurez le type de colonne](xref:core/modeling/relational/data-types) dans votre modèle.</span><span class="sxs-lookup"><span data-stu-id="d54db-167">To use `geometry`, [configure the column type](xref:core/modeling/relational/data-types) in your model.</span></span>

### <a name="geography-polygon-rings"></a><span data-ttu-id="d54db-168">Anneaux de polygone Geography</span><span class="sxs-lookup"><span data-stu-id="d54db-168">Geography polygon rings</span></span>

<span data-ttu-id="d54db-169">Lors de l’utilisation du type de colonne `geography`, SQL Server impose des exigences supplémentaires sur l’anneau extérieur (ou l’interpréteur de commandes) et les anneaux intérieurs (ou trous).</span><span class="sxs-lookup"><span data-stu-id="d54db-169">When using the `geography` column type, SQL Server imposes additional requirements on the exterior ring (or shell) and interior rings (or holes).</span></span> <span data-ttu-id="d54db-170">L’anneau extérieur doit être orienté vers le sens inverse des aiguilles d’une montre et des anneaux intérieurs.</span><span class="sxs-lookup"><span data-stu-id="d54db-170">The exterior ring must be oriented counterclockwise and the interior rings clockwise.</span></span> <span data-ttu-id="d54db-171">NTS valide cette valeur avant d’envoyer des valeurs à la base de données.</span><span class="sxs-lookup"><span data-stu-id="d54db-171">NTS validates this before sending values to the database.</span></span>

### <a name="fullglobe"></a><span data-ttu-id="d54db-172">FullGlobe</span><span class="sxs-lookup"><span data-stu-id="d54db-172">FullGlobe</span></span>

<span data-ttu-id="d54db-173">SQL Server a un type Geometry non standard pour représenter le globe complet lors de l’utilisation du type de colonne `geography`.</span><span class="sxs-lookup"><span data-stu-id="d54db-173">SQL Server has a non-standard geometry type to represent the full globe when using the `geography` column type.</span></span> <span data-ttu-id="d54db-174">Il offre également un moyen de représenter les polygones en fonction du monde entier (sans anneau extérieur).</span><span class="sxs-lookup"><span data-stu-id="d54db-174">It also has a way to represent polygons based on the full globe (without an exterior ring).</span></span> <span data-ttu-id="d54db-175">Aucun de ces deux n’est pris en charge par NTS.</span><span class="sxs-lookup"><span data-stu-id="d54db-175">Neither of these are supported by NTS.</span></span>

> [!WARNING]
> <span data-ttu-id="d54db-176">Les FullGlobe et les polygones basés sur ce dernier ne sont pas pris en charge par NTS.</span><span class="sxs-lookup"><span data-stu-id="d54db-176">FullGlobe and polygons based on it aren't supported by NTS.</span></span>

## <a name="sqlite"></a><span data-ttu-id="d54db-177">SQLite</span><span class="sxs-lookup"><span data-stu-id="d54db-177">SQLite</span></span>

<span data-ttu-id="d54db-178">Voici quelques informations supplémentaires pour celles qui utilisent SQLite.</span><span class="sxs-lookup"><span data-stu-id="d54db-178">Here is some additional information for those using SQLite.</span></span>

### <a name="installing-spatialite"></a><span data-ttu-id="d54db-179">Installation de SpatiaLite</span><span class="sxs-lookup"><span data-stu-id="d54db-179">Installing SpatiaLite</span></span>

<span data-ttu-id="d54db-180">Sur Windows, la bibliothèque mod_spatialite native est distribuée en tant que dépendance de package NuGet.</span><span class="sxs-lookup"><span data-stu-id="d54db-180">On Windows, the native mod_spatialite library is distributed as a NuGet package dependency.</span></span> <span data-ttu-id="d54db-181">D’autres plateformes doivent l’installer séparément.</span><span class="sxs-lookup"><span data-stu-id="d54db-181">Other platforms need to install it separately.</span></span> <span data-ttu-id="d54db-182">Cette opération s’effectue généralement à l’aide d’un gestionnaire de package logiciel.</span><span class="sxs-lookup"><span data-stu-id="d54db-182">This is typically done using a software package manager.</span></span> <span data-ttu-id="d54db-183">Par exemple, vous pouvez utiliser la fonction APT sur Ubuntu et homebrew sur MacOS.</span><span class="sxs-lookup"><span data-stu-id="d54db-183">For example, you can use APT on Ubuntu and Homebrew on MacOS.</span></span>

``` sh
# Ubuntu
apt-get install libsqlite3-mod-spatialite

# macOS
brew install libspatialite
```

### <a name="configuring-srid"></a><span data-ttu-id="d54db-184">Configuration de SRID</span><span class="sxs-lookup"><span data-stu-id="d54db-184">Configuring SRID</span></span>

<span data-ttu-id="d54db-185">Dans SpatiaLite, les colonnes doivent spécifier un SRID par colonne.</span><span class="sxs-lookup"><span data-stu-id="d54db-185">In SpatiaLite, columns need to specify an SRID per column.</span></span> <span data-ttu-id="d54db-186">La valeur par défaut de SRID est `0`.</span><span class="sxs-lookup"><span data-stu-id="d54db-186">The default SRID is `0`.</span></span> <span data-ttu-id="d54db-187">Spécifiez un autre SRID à l’aide de la méthode ForSqliteHasSrid.</span><span class="sxs-lookup"><span data-stu-id="d54db-187">Specify a different SRID using the ForSqliteHasSrid method.</span></span>

``` csharp
modelBuilder.Entity<City>().Property(c => c.Location)
    .ForSqliteHasSrid(4326);
```

### <a name="dimension"></a><span data-ttu-id="d54db-188">Dimension</span><span class="sxs-lookup"><span data-stu-id="d54db-188">Dimension</span></span>

<span data-ttu-id="d54db-189">Comme pour SRID, la dimension d’une colonne (ou ordonnée) est également spécifiée dans le cadre de la colonne.</span><span class="sxs-lookup"><span data-stu-id="d54db-189">Similar to SRID, a column's dimension (or ordinates) is also specified as part of the column.</span></span> <span data-ttu-id="d54db-190">Les ordonnées par défaut sont X et Y. Activez des ordonnées supplémentaires (Z et M) à l’aide de la méthode ForSqliteHasDimension.</span><span class="sxs-lookup"><span data-stu-id="d54db-190">The default ordinates are X and Y. Enable additional ordinates (Z and M) using the ForSqliteHasDimension method.</span></span>

``` csharp
modelBuilder.Entity<City>().Property(c => c.Location)
    .ForSqliteHasDimension(Ordinates.XYZ);
```

## <a name="translated-operations"></a><span data-ttu-id="d54db-191">Opérations traduites</span><span class="sxs-lookup"><span data-stu-id="d54db-191">Translated Operations</span></span>

<span data-ttu-id="d54db-192">Ce tableau montre les membres NTS qui sont convertis en SQL par chaque fournisseur de EF Core.</span><span class="sxs-lookup"><span data-stu-id="d54db-192">This table shows which NTS members are translated into SQL by each EF Core provider.</span></span>

<span data-ttu-id="d54db-193">NetTopologySuite</span><span class="sxs-lookup"><span data-stu-id="d54db-193">NetTopologySuite</span></span> | <span data-ttu-id="d54db-194">SQL Server (géométrie)</span><span class="sxs-lookup"><span data-stu-id="d54db-194">SQL Server (geometry)</span></span> | <span data-ttu-id="d54db-195">SQL Server (geography)</span><span class="sxs-lookup"><span data-stu-id="d54db-195">SQL Server (geography)</span></span> | <span data-ttu-id="d54db-196">SQLite</span><span class="sxs-lookup"><span data-stu-id="d54db-196">SQLite</span></span> | <span data-ttu-id="d54db-197">Npgsql</span><span class="sxs-lookup"><span data-stu-id="d54db-197">Npgsql</span></span>
--- |:---:|:---:|:---:|:---:
<span data-ttu-id="d54db-198">Geometry. Area</span><span class="sxs-lookup"><span data-stu-id="d54db-198">Geometry.Area</span></span> | <span data-ttu-id="d54db-199">✔</span><span class="sxs-lookup"><span data-stu-id="d54db-199">✔</span></span> | <span data-ttu-id="d54db-200">✔</span><span class="sxs-lookup"><span data-stu-id="d54db-200">✔</span></span> | <span data-ttu-id="d54db-201">✔</span><span class="sxs-lookup"><span data-stu-id="d54db-201">✔</span></span> | <span data-ttu-id="d54db-202">✔</span><span class="sxs-lookup"><span data-stu-id="d54db-202">✔</span></span>
<span data-ttu-id="d54db-203">Geometry. AsBinary ()</span><span class="sxs-lookup"><span data-stu-id="d54db-203">Geometry.AsBinary()</span></span> | <span data-ttu-id="d54db-204">✔</span><span class="sxs-lookup"><span data-stu-id="d54db-204">✔</span></span> | <span data-ttu-id="d54db-205">✔</span><span class="sxs-lookup"><span data-stu-id="d54db-205">✔</span></span> | <span data-ttu-id="d54db-206">✔</span><span class="sxs-lookup"><span data-stu-id="d54db-206">✔</span></span> | <span data-ttu-id="d54db-207">✔</span><span class="sxs-lookup"><span data-stu-id="d54db-207">✔</span></span>
<span data-ttu-id="d54db-208">Geometry. AsText ()</span><span class="sxs-lookup"><span data-stu-id="d54db-208">Geometry.AsText()</span></span> | <span data-ttu-id="d54db-209">✔</span><span class="sxs-lookup"><span data-stu-id="d54db-209">✔</span></span> | <span data-ttu-id="d54db-210">✔</span><span class="sxs-lookup"><span data-stu-id="d54db-210">✔</span></span> | <span data-ttu-id="d54db-211">✔</span><span class="sxs-lookup"><span data-stu-id="d54db-211">✔</span></span> | <span data-ttu-id="d54db-212">✔</span><span class="sxs-lookup"><span data-stu-id="d54db-212">✔</span></span>
<span data-ttu-id="d54db-213">Geometry. Boundary</span><span class="sxs-lookup"><span data-stu-id="d54db-213">Geometry.Boundary</span></span> | <span data-ttu-id="d54db-214">✔</span><span class="sxs-lookup"><span data-stu-id="d54db-214">✔</span></span> | | <span data-ttu-id="d54db-215">✔</span><span class="sxs-lookup"><span data-stu-id="d54db-215">✔</span></span> | <span data-ttu-id="d54db-216">✔</span><span class="sxs-lookup"><span data-stu-id="d54db-216">✔</span></span>
<span data-ttu-id="d54db-217">Geometry. buffer (double)</span><span class="sxs-lookup"><span data-stu-id="d54db-217">Geometry.Buffer(double)</span></span> | <span data-ttu-id="d54db-218">✔</span><span class="sxs-lookup"><span data-stu-id="d54db-218">✔</span></span> | <span data-ttu-id="d54db-219">✔</span><span class="sxs-lookup"><span data-stu-id="d54db-219">✔</span></span> | <span data-ttu-id="d54db-220">✔</span><span class="sxs-lookup"><span data-stu-id="d54db-220">✔</span></span> | <span data-ttu-id="d54db-221">✔</span><span class="sxs-lookup"><span data-stu-id="d54db-221">✔</span></span>
<span data-ttu-id="d54db-222">Geometry. buffer (double, int)</span><span class="sxs-lookup"><span data-stu-id="d54db-222">Geometry.Buffer(double, int)</span></span> | | | <span data-ttu-id="d54db-223">✔</span><span class="sxs-lookup"><span data-stu-id="d54db-223">✔</span></span> | <span data-ttu-id="d54db-224">✔</span><span class="sxs-lookup"><span data-stu-id="d54db-224">✔</span></span>
<span data-ttu-id="d54db-225">Geometry. Centre de gravité</span><span class="sxs-lookup"><span data-stu-id="d54db-225">Geometry.Centroid</span></span> | <span data-ttu-id="d54db-226">✔</span><span class="sxs-lookup"><span data-stu-id="d54db-226">✔</span></span> | | <span data-ttu-id="d54db-227">✔</span><span class="sxs-lookup"><span data-stu-id="d54db-227">✔</span></span> | <span data-ttu-id="d54db-228">✔</span><span class="sxs-lookup"><span data-stu-id="d54db-228">✔</span></span>
<span data-ttu-id="d54db-229">Geometry. Contains (Geometry)</span><span class="sxs-lookup"><span data-stu-id="d54db-229">Geometry.Contains(Geometry)</span></span> | <span data-ttu-id="d54db-230">✔</span><span class="sxs-lookup"><span data-stu-id="d54db-230">✔</span></span> | <span data-ttu-id="d54db-231">✔</span><span class="sxs-lookup"><span data-stu-id="d54db-231">✔</span></span> | <span data-ttu-id="d54db-232">✔</span><span class="sxs-lookup"><span data-stu-id="d54db-232">✔</span></span> | <span data-ttu-id="d54db-233">✔</span><span class="sxs-lookup"><span data-stu-id="d54db-233">✔</span></span>
<span data-ttu-id="d54db-234">Geometry. ConvexHull ()</span><span class="sxs-lookup"><span data-stu-id="d54db-234">Geometry.ConvexHull()</span></span> | <span data-ttu-id="d54db-235">✔</span><span class="sxs-lookup"><span data-stu-id="d54db-235">✔</span></span> | <span data-ttu-id="d54db-236">✔</span><span class="sxs-lookup"><span data-stu-id="d54db-236">✔</span></span> | <span data-ttu-id="d54db-237">✔</span><span class="sxs-lookup"><span data-stu-id="d54db-237">✔</span></span> | <span data-ttu-id="d54db-238">✔</span><span class="sxs-lookup"><span data-stu-id="d54db-238">✔</span></span>
<span data-ttu-id="d54db-239">Geometry. CoveredBy (Geometry)</span><span class="sxs-lookup"><span data-stu-id="d54db-239">Geometry.CoveredBy(Geometry)</span></span> | | | <span data-ttu-id="d54db-240">✔</span><span class="sxs-lookup"><span data-stu-id="d54db-240">✔</span></span> | <span data-ttu-id="d54db-241">✔</span><span class="sxs-lookup"><span data-stu-id="d54db-241">✔</span></span>
<span data-ttu-id="d54db-242">Geometry. couvertures (Geometry)</span><span class="sxs-lookup"><span data-stu-id="d54db-242">Geometry.Covers(Geometry)</span></span> | | | <span data-ttu-id="d54db-243">✔</span><span class="sxs-lookup"><span data-stu-id="d54db-243">✔</span></span> | <span data-ttu-id="d54db-244">✔</span><span class="sxs-lookup"><span data-stu-id="d54db-244">✔</span></span>
<span data-ttu-id="d54db-245">Geometry. crosses (Geometry)</span><span class="sxs-lookup"><span data-stu-id="d54db-245">Geometry.Crosses(Geometry)</span></span> | <span data-ttu-id="d54db-246">✔</span><span class="sxs-lookup"><span data-stu-id="d54db-246">✔</span></span> | | <span data-ttu-id="d54db-247">✔</span><span class="sxs-lookup"><span data-stu-id="d54db-247">✔</span></span> | <span data-ttu-id="d54db-248">✔</span><span class="sxs-lookup"><span data-stu-id="d54db-248">✔</span></span>
<span data-ttu-id="d54db-249">Geometry. difference (Geometry)</span><span class="sxs-lookup"><span data-stu-id="d54db-249">Geometry.Difference(Geometry)</span></span> | <span data-ttu-id="d54db-250">✔</span><span class="sxs-lookup"><span data-stu-id="d54db-250">✔</span></span> | <span data-ttu-id="d54db-251">✔</span><span class="sxs-lookup"><span data-stu-id="d54db-251">✔</span></span> | <span data-ttu-id="d54db-252">✔</span><span class="sxs-lookup"><span data-stu-id="d54db-252">✔</span></span> | <span data-ttu-id="d54db-253">✔</span><span class="sxs-lookup"><span data-stu-id="d54db-253">✔</span></span>
<span data-ttu-id="d54db-254">Geometry. dimension</span><span class="sxs-lookup"><span data-stu-id="d54db-254">Geometry.Dimension</span></span> | <span data-ttu-id="d54db-255">✔</span><span class="sxs-lookup"><span data-stu-id="d54db-255">✔</span></span> | <span data-ttu-id="d54db-256">✔</span><span class="sxs-lookup"><span data-stu-id="d54db-256">✔</span></span> | <span data-ttu-id="d54db-257">✔</span><span class="sxs-lookup"><span data-stu-id="d54db-257">✔</span></span> | <span data-ttu-id="d54db-258">✔</span><span class="sxs-lookup"><span data-stu-id="d54db-258">✔</span></span>
<span data-ttu-id="d54db-259">Geometry. disjointe (Geometry)</span><span class="sxs-lookup"><span data-stu-id="d54db-259">Geometry.Disjoint(Geometry)</span></span> | <span data-ttu-id="d54db-260">✔</span><span class="sxs-lookup"><span data-stu-id="d54db-260">✔</span></span> | <span data-ttu-id="d54db-261">✔</span><span class="sxs-lookup"><span data-stu-id="d54db-261">✔</span></span> | <span data-ttu-id="d54db-262">✔</span><span class="sxs-lookup"><span data-stu-id="d54db-262">✔</span></span> | <span data-ttu-id="d54db-263">✔</span><span class="sxs-lookup"><span data-stu-id="d54db-263">✔</span></span>
<span data-ttu-id="d54db-264">Geometry. distance (Geometry)</span><span class="sxs-lookup"><span data-stu-id="d54db-264">Geometry.Distance(Geometry)</span></span> | <span data-ttu-id="d54db-265">✔</span><span class="sxs-lookup"><span data-stu-id="d54db-265">✔</span></span> | <span data-ttu-id="d54db-266">✔</span><span class="sxs-lookup"><span data-stu-id="d54db-266">✔</span></span> | <span data-ttu-id="d54db-267">✔</span><span class="sxs-lookup"><span data-stu-id="d54db-267">✔</span></span> | <span data-ttu-id="d54db-268">✔</span><span class="sxs-lookup"><span data-stu-id="d54db-268">✔</span></span>
<span data-ttu-id="d54db-269">Geometry. Envelope</span><span class="sxs-lookup"><span data-stu-id="d54db-269">Geometry.Envelope</span></span> | <span data-ttu-id="d54db-270">✔</span><span class="sxs-lookup"><span data-stu-id="d54db-270">✔</span></span> | | <span data-ttu-id="d54db-271">✔</span><span class="sxs-lookup"><span data-stu-id="d54db-271">✔</span></span> | <span data-ttu-id="d54db-272">✔</span><span class="sxs-lookup"><span data-stu-id="d54db-272">✔</span></span>
<span data-ttu-id="d54db-273">Geometry. EqualsExact (Geometry)</span><span class="sxs-lookup"><span data-stu-id="d54db-273">Geometry.EqualsExact(Geometry)</span></span> | | | | <span data-ttu-id="d54db-274">✔</span><span class="sxs-lookup"><span data-stu-id="d54db-274">✔</span></span>
<span data-ttu-id="d54db-275">Geometry. EqualsTopologically (Geometry)</span><span class="sxs-lookup"><span data-stu-id="d54db-275">Geometry.EqualsTopologically(Geometry)</span></span> | <span data-ttu-id="d54db-276">✔</span><span class="sxs-lookup"><span data-stu-id="d54db-276">✔</span></span> | <span data-ttu-id="d54db-277">✔</span><span class="sxs-lookup"><span data-stu-id="d54db-277">✔</span></span> | <span data-ttu-id="d54db-278">✔</span><span class="sxs-lookup"><span data-stu-id="d54db-278">✔</span></span> | <span data-ttu-id="d54db-279">✔</span><span class="sxs-lookup"><span data-stu-id="d54db-279">✔</span></span>
<span data-ttu-id="d54db-280">Geometry. GeometryType</span><span class="sxs-lookup"><span data-stu-id="d54db-280">Geometry.GeometryType</span></span> | <span data-ttu-id="d54db-281">✔</span><span class="sxs-lookup"><span data-stu-id="d54db-281">✔</span></span> | <span data-ttu-id="d54db-282">✔</span><span class="sxs-lookup"><span data-stu-id="d54db-282">✔</span></span> | <span data-ttu-id="d54db-283">✔</span><span class="sxs-lookup"><span data-stu-id="d54db-283">✔</span></span> | <span data-ttu-id="d54db-284">✔</span><span class="sxs-lookup"><span data-stu-id="d54db-284">✔</span></span>
<span data-ttu-id="d54db-285">Geometry. GetGeometryN (int)</span><span class="sxs-lookup"><span data-stu-id="d54db-285">Geometry.GetGeometryN(int)</span></span> | <span data-ttu-id="d54db-286">✔</span><span class="sxs-lookup"><span data-stu-id="d54db-286">✔</span></span> | | <span data-ttu-id="d54db-287">✔</span><span class="sxs-lookup"><span data-stu-id="d54db-287">✔</span></span> | <span data-ttu-id="d54db-288">✔</span><span class="sxs-lookup"><span data-stu-id="d54db-288">✔</span></span>
<span data-ttu-id="d54db-289">Geometry. InteriorPoint</span><span class="sxs-lookup"><span data-stu-id="d54db-289">Geometry.InteriorPoint</span></span> | <span data-ttu-id="d54db-290">✔</span><span class="sxs-lookup"><span data-stu-id="d54db-290">✔</span></span> | | <span data-ttu-id="d54db-291">✔</span><span class="sxs-lookup"><span data-stu-id="d54db-291">✔</span></span> | <span data-ttu-id="d54db-292">✔</span><span class="sxs-lookup"><span data-stu-id="d54db-292">✔</span></span>
<span data-ttu-id="d54db-293">Geometry. intersection (Geometry)</span><span class="sxs-lookup"><span data-stu-id="d54db-293">Geometry.Intersection(Geometry)</span></span> | <span data-ttu-id="d54db-294">✔</span><span class="sxs-lookup"><span data-stu-id="d54db-294">✔</span></span> | <span data-ttu-id="d54db-295">✔</span><span class="sxs-lookup"><span data-stu-id="d54db-295">✔</span></span> | <span data-ttu-id="d54db-296">✔</span><span class="sxs-lookup"><span data-stu-id="d54db-296">✔</span></span> | <span data-ttu-id="d54db-297">✔</span><span class="sxs-lookup"><span data-stu-id="d54db-297">✔</span></span>
<span data-ttu-id="d54db-298">Geometry. intersections (Geometry)</span><span class="sxs-lookup"><span data-stu-id="d54db-298">Geometry.Intersects(Geometry)</span></span> | <span data-ttu-id="d54db-299">✔</span><span class="sxs-lookup"><span data-stu-id="d54db-299">✔</span></span> | <span data-ttu-id="d54db-300">✔</span><span class="sxs-lookup"><span data-stu-id="d54db-300">✔</span></span> | <span data-ttu-id="d54db-301">✔</span><span class="sxs-lookup"><span data-stu-id="d54db-301">✔</span></span> | <span data-ttu-id="d54db-302">✔</span><span class="sxs-lookup"><span data-stu-id="d54db-302">✔</span></span>
<span data-ttu-id="d54db-303">Geometry. IsEmpty</span><span class="sxs-lookup"><span data-stu-id="d54db-303">Geometry.IsEmpty</span></span> | <span data-ttu-id="d54db-304">✔</span><span class="sxs-lookup"><span data-stu-id="d54db-304">✔</span></span> | <span data-ttu-id="d54db-305">✔</span><span class="sxs-lookup"><span data-stu-id="d54db-305">✔</span></span> | <span data-ttu-id="d54db-306">✔</span><span class="sxs-lookup"><span data-stu-id="d54db-306">✔</span></span> | <span data-ttu-id="d54db-307">✔</span><span class="sxs-lookup"><span data-stu-id="d54db-307">✔</span></span>
<span data-ttu-id="d54db-308">Geometry. IsSimple</span><span class="sxs-lookup"><span data-stu-id="d54db-308">Geometry.IsSimple</span></span> | <span data-ttu-id="d54db-309">✔</span><span class="sxs-lookup"><span data-stu-id="d54db-309">✔</span></span> | | <span data-ttu-id="d54db-310">✔</span><span class="sxs-lookup"><span data-stu-id="d54db-310">✔</span></span> | <span data-ttu-id="d54db-311">✔</span><span class="sxs-lookup"><span data-stu-id="d54db-311">✔</span></span>
<span data-ttu-id="d54db-312">Geometry. IsValid</span><span class="sxs-lookup"><span data-stu-id="d54db-312">Geometry.IsValid</span></span> | <span data-ttu-id="d54db-313">✔</span><span class="sxs-lookup"><span data-stu-id="d54db-313">✔</span></span> | <span data-ttu-id="d54db-314">✔</span><span class="sxs-lookup"><span data-stu-id="d54db-314">✔</span></span> | <span data-ttu-id="d54db-315">✔</span><span class="sxs-lookup"><span data-stu-id="d54db-315">✔</span></span> | <span data-ttu-id="d54db-316">✔</span><span class="sxs-lookup"><span data-stu-id="d54db-316">✔</span></span>
<span data-ttu-id="d54db-317">Geometry. IsWithinDistance (Geometry, double)</span><span class="sxs-lookup"><span data-stu-id="d54db-317">Geometry.IsWithinDistance(Geometry, double)</span></span> | <span data-ttu-id="d54db-318">✔</span><span class="sxs-lookup"><span data-stu-id="d54db-318">✔</span></span> | | <span data-ttu-id="d54db-319">✔</span><span class="sxs-lookup"><span data-stu-id="d54db-319">✔</span></span> | <span data-ttu-id="d54db-320">✔</span><span class="sxs-lookup"><span data-stu-id="d54db-320">✔</span></span>
<span data-ttu-id="d54db-321">Geometry. Length</span><span class="sxs-lookup"><span data-stu-id="d54db-321">Geometry.Length</span></span> | <span data-ttu-id="d54db-322">✔</span><span class="sxs-lookup"><span data-stu-id="d54db-322">✔</span></span> | <span data-ttu-id="d54db-323">✔</span><span class="sxs-lookup"><span data-stu-id="d54db-323">✔</span></span> | <span data-ttu-id="d54db-324">✔</span><span class="sxs-lookup"><span data-stu-id="d54db-324">✔</span></span> | <span data-ttu-id="d54db-325">✔</span><span class="sxs-lookup"><span data-stu-id="d54db-325">✔</span></span>
<span data-ttu-id="d54db-326">Geometry. NumGeometries</span><span class="sxs-lookup"><span data-stu-id="d54db-326">Geometry.NumGeometries</span></span> | <span data-ttu-id="d54db-327">✔</span><span class="sxs-lookup"><span data-stu-id="d54db-327">✔</span></span> | <span data-ttu-id="d54db-328">✔</span><span class="sxs-lookup"><span data-stu-id="d54db-328">✔</span></span> | <span data-ttu-id="d54db-329">✔</span><span class="sxs-lookup"><span data-stu-id="d54db-329">✔</span></span> | <span data-ttu-id="d54db-330">✔</span><span class="sxs-lookup"><span data-stu-id="d54db-330">✔</span></span>
<span data-ttu-id="d54db-331">Geometry. NumPoints</span><span class="sxs-lookup"><span data-stu-id="d54db-331">Geometry.NumPoints</span></span> | <span data-ttu-id="d54db-332">✔</span><span class="sxs-lookup"><span data-stu-id="d54db-332">✔</span></span> | <span data-ttu-id="d54db-333">✔</span><span class="sxs-lookup"><span data-stu-id="d54db-333">✔</span></span> | <span data-ttu-id="d54db-334">✔</span><span class="sxs-lookup"><span data-stu-id="d54db-334">✔</span></span> | <span data-ttu-id="d54db-335">✔</span><span class="sxs-lookup"><span data-stu-id="d54db-335">✔</span></span>
<span data-ttu-id="d54db-336">Geometry. OgcGeometryType</span><span class="sxs-lookup"><span data-stu-id="d54db-336">Geometry.OgcGeometryType</span></span> | <span data-ttu-id="d54db-337">✔</span><span class="sxs-lookup"><span data-stu-id="d54db-337">✔</span></span> | <span data-ttu-id="d54db-338">✔</span><span class="sxs-lookup"><span data-stu-id="d54db-338">✔</span></span> | <span data-ttu-id="d54db-339">✔</span><span class="sxs-lookup"><span data-stu-id="d54db-339">✔</span></span> | <span data-ttu-id="d54db-340">✔</span><span class="sxs-lookup"><span data-stu-id="d54db-340">✔</span></span>
<span data-ttu-id="d54db-341">Geometry. chevauchements (Geometry)</span><span class="sxs-lookup"><span data-stu-id="d54db-341">Geometry.Overlaps(Geometry)</span></span> | <span data-ttu-id="d54db-342">✔</span><span class="sxs-lookup"><span data-stu-id="d54db-342">✔</span></span> | <span data-ttu-id="d54db-343">✔</span><span class="sxs-lookup"><span data-stu-id="d54db-343">✔</span></span> | <span data-ttu-id="d54db-344">✔</span><span class="sxs-lookup"><span data-stu-id="d54db-344">✔</span></span> | <span data-ttu-id="d54db-345">✔</span><span class="sxs-lookup"><span data-stu-id="d54db-345">✔</span></span>
<span data-ttu-id="d54db-346">Geometry. PointOnSurface</span><span class="sxs-lookup"><span data-stu-id="d54db-346">Geometry.PointOnSurface</span></span> | <span data-ttu-id="d54db-347">✔</span><span class="sxs-lookup"><span data-stu-id="d54db-347">✔</span></span> | | <span data-ttu-id="d54db-348">✔</span><span class="sxs-lookup"><span data-stu-id="d54db-348">✔</span></span> | <span data-ttu-id="d54db-349">✔</span><span class="sxs-lookup"><span data-stu-id="d54db-349">✔</span></span>
<span data-ttu-id="d54db-350">Geometry. relate (Geometry, String)</span><span class="sxs-lookup"><span data-stu-id="d54db-350">Geometry.Relate(Geometry, string)</span></span> | <span data-ttu-id="d54db-351">✔</span><span class="sxs-lookup"><span data-stu-id="d54db-351">✔</span></span> | | <span data-ttu-id="d54db-352">✔</span><span class="sxs-lookup"><span data-stu-id="d54db-352">✔</span></span> | <span data-ttu-id="d54db-353">✔</span><span class="sxs-lookup"><span data-stu-id="d54db-353">✔</span></span>
<span data-ttu-id="d54db-354">Geometry. Reverse ()</span><span class="sxs-lookup"><span data-stu-id="d54db-354">Geometry.Reverse()</span></span> | | | <span data-ttu-id="d54db-355">✔</span><span class="sxs-lookup"><span data-stu-id="d54db-355">✔</span></span> | <span data-ttu-id="d54db-356">✔</span><span class="sxs-lookup"><span data-stu-id="d54db-356">✔</span></span>
<span data-ttu-id="d54db-357">Geometry. SRID</span><span class="sxs-lookup"><span data-stu-id="d54db-357">Geometry.SRID</span></span> | <span data-ttu-id="d54db-358">✔</span><span class="sxs-lookup"><span data-stu-id="d54db-358">✔</span></span> | <span data-ttu-id="d54db-359">✔</span><span class="sxs-lookup"><span data-stu-id="d54db-359">✔</span></span> | <span data-ttu-id="d54db-360">✔</span><span class="sxs-lookup"><span data-stu-id="d54db-360">✔</span></span> | <span data-ttu-id="d54db-361">✔</span><span class="sxs-lookup"><span data-stu-id="d54db-361">✔</span></span>
<span data-ttu-id="d54db-362">Geometry. SymmetricDifference (Geometry)</span><span class="sxs-lookup"><span data-stu-id="d54db-362">Geometry.SymmetricDifference(Geometry)</span></span> | <span data-ttu-id="d54db-363">✔</span><span class="sxs-lookup"><span data-stu-id="d54db-363">✔</span></span> | <span data-ttu-id="d54db-364">✔</span><span class="sxs-lookup"><span data-stu-id="d54db-364">✔</span></span> | <span data-ttu-id="d54db-365">✔</span><span class="sxs-lookup"><span data-stu-id="d54db-365">✔</span></span> | <span data-ttu-id="d54db-366">✔</span><span class="sxs-lookup"><span data-stu-id="d54db-366">✔</span></span>
<span data-ttu-id="d54db-367">Geometry. ToBinary ()</span><span class="sxs-lookup"><span data-stu-id="d54db-367">Geometry.ToBinary()</span></span> | <span data-ttu-id="d54db-368">✔</span><span class="sxs-lookup"><span data-stu-id="d54db-368">✔</span></span> | <span data-ttu-id="d54db-369">✔</span><span class="sxs-lookup"><span data-stu-id="d54db-369">✔</span></span> | <span data-ttu-id="d54db-370">✔</span><span class="sxs-lookup"><span data-stu-id="d54db-370">✔</span></span> | <span data-ttu-id="d54db-371">✔</span><span class="sxs-lookup"><span data-stu-id="d54db-371">✔</span></span>
<span data-ttu-id="d54db-372">Geometry. ToText ()</span><span class="sxs-lookup"><span data-stu-id="d54db-372">Geometry.ToText()</span></span> | <span data-ttu-id="d54db-373">✔</span><span class="sxs-lookup"><span data-stu-id="d54db-373">✔</span></span> | <span data-ttu-id="d54db-374">✔</span><span class="sxs-lookup"><span data-stu-id="d54db-374">✔</span></span> | <span data-ttu-id="d54db-375">✔</span><span class="sxs-lookup"><span data-stu-id="d54db-375">✔</span></span> | <span data-ttu-id="d54db-376">✔</span><span class="sxs-lookup"><span data-stu-id="d54db-376">✔</span></span>
<span data-ttu-id="d54db-377">Geometry. touche (Geometry)</span><span class="sxs-lookup"><span data-stu-id="d54db-377">Geometry.Touches(Geometry)</span></span> | <span data-ttu-id="d54db-378">✔</span><span class="sxs-lookup"><span data-stu-id="d54db-378">✔</span></span> | | <span data-ttu-id="d54db-379">✔</span><span class="sxs-lookup"><span data-stu-id="d54db-379">✔</span></span> | <span data-ttu-id="d54db-380">✔</span><span class="sxs-lookup"><span data-stu-id="d54db-380">✔</span></span>
<span data-ttu-id="d54db-381">Geometry. Union ()</span><span class="sxs-lookup"><span data-stu-id="d54db-381">Geometry.Union()</span></span> | | | <span data-ttu-id="d54db-382">✔</span><span class="sxs-lookup"><span data-stu-id="d54db-382">✔</span></span> | <span data-ttu-id="d54db-383">✔</span><span class="sxs-lookup"><span data-stu-id="d54db-383">✔</span></span>
<span data-ttu-id="d54db-384">Geometry. Union (Geometry)</span><span class="sxs-lookup"><span data-stu-id="d54db-384">Geometry.Union(Geometry)</span></span> | <span data-ttu-id="d54db-385">✔</span><span class="sxs-lookup"><span data-stu-id="d54db-385">✔</span></span> | <span data-ttu-id="d54db-386">✔</span><span class="sxs-lookup"><span data-stu-id="d54db-386">✔</span></span> | <span data-ttu-id="d54db-387">✔</span><span class="sxs-lookup"><span data-stu-id="d54db-387">✔</span></span> | <span data-ttu-id="d54db-388">✔</span><span class="sxs-lookup"><span data-stu-id="d54db-388">✔</span></span>
<span data-ttu-id="d54db-389">Geometry. within (Geometry)</span><span class="sxs-lookup"><span data-stu-id="d54db-389">Geometry.Within(Geometry)</span></span> | <span data-ttu-id="d54db-390">✔</span><span class="sxs-lookup"><span data-stu-id="d54db-390">✔</span></span> | <span data-ttu-id="d54db-391">✔</span><span class="sxs-lookup"><span data-stu-id="d54db-391">✔</span></span> | <span data-ttu-id="d54db-392">✔</span><span class="sxs-lookup"><span data-stu-id="d54db-392">✔</span></span> | <span data-ttu-id="d54db-393">✔</span><span class="sxs-lookup"><span data-stu-id="d54db-393">✔</span></span>
<span data-ttu-id="d54db-394">GeometryCollection. Count</span><span class="sxs-lookup"><span data-stu-id="d54db-394">GeometryCollection.Count</span></span> | <span data-ttu-id="d54db-395">✔</span><span class="sxs-lookup"><span data-stu-id="d54db-395">✔</span></span> | <span data-ttu-id="d54db-396">✔</span><span class="sxs-lookup"><span data-stu-id="d54db-396">✔</span></span> | <span data-ttu-id="d54db-397">✔</span><span class="sxs-lookup"><span data-stu-id="d54db-397">✔</span></span> | <span data-ttu-id="d54db-398">✔</span><span class="sxs-lookup"><span data-stu-id="d54db-398">✔</span></span>
<span data-ttu-id="d54db-399">GeometryCollection [int]</span><span class="sxs-lookup"><span data-stu-id="d54db-399">GeometryCollection[int]</span></span> | <span data-ttu-id="d54db-400">✔</span><span class="sxs-lookup"><span data-stu-id="d54db-400">✔</span></span> | <span data-ttu-id="d54db-401">✔</span><span class="sxs-lookup"><span data-stu-id="d54db-401">✔</span></span> | <span data-ttu-id="d54db-402">✔</span><span class="sxs-lookup"><span data-stu-id="d54db-402">✔</span></span> | <span data-ttu-id="d54db-403">✔</span><span class="sxs-lookup"><span data-stu-id="d54db-403">✔</span></span>
<span data-ttu-id="d54db-404">LineString. Count</span><span class="sxs-lookup"><span data-stu-id="d54db-404">LineString.Count</span></span> | <span data-ttu-id="d54db-405">✔</span><span class="sxs-lookup"><span data-stu-id="d54db-405">✔</span></span> | <span data-ttu-id="d54db-406">✔</span><span class="sxs-lookup"><span data-stu-id="d54db-406">✔</span></span> | <span data-ttu-id="d54db-407">✔</span><span class="sxs-lookup"><span data-stu-id="d54db-407">✔</span></span> | <span data-ttu-id="d54db-408">✔</span><span class="sxs-lookup"><span data-stu-id="d54db-408">✔</span></span>
<span data-ttu-id="d54db-409">LineString. point de terminaison</span><span class="sxs-lookup"><span data-stu-id="d54db-409">LineString.EndPoint</span></span> | <span data-ttu-id="d54db-410">✔</span><span class="sxs-lookup"><span data-stu-id="d54db-410">✔</span></span> | <span data-ttu-id="d54db-411">✔</span><span class="sxs-lookup"><span data-stu-id="d54db-411">✔</span></span> | <span data-ttu-id="d54db-412">✔</span><span class="sxs-lookup"><span data-stu-id="d54db-412">✔</span></span> | <span data-ttu-id="d54db-413">✔</span><span class="sxs-lookup"><span data-stu-id="d54db-413">✔</span></span>
<span data-ttu-id="d54db-414">LineString. GetPointN (int)</span><span class="sxs-lookup"><span data-stu-id="d54db-414">LineString.GetPointN(int)</span></span> | <span data-ttu-id="d54db-415">✔</span><span class="sxs-lookup"><span data-stu-id="d54db-415">✔</span></span> | <span data-ttu-id="d54db-416">✔</span><span class="sxs-lookup"><span data-stu-id="d54db-416">✔</span></span> | <span data-ttu-id="d54db-417">✔</span><span class="sxs-lookup"><span data-stu-id="d54db-417">✔</span></span> | <span data-ttu-id="d54db-418">✔</span><span class="sxs-lookup"><span data-stu-id="d54db-418">✔</span></span>
<span data-ttu-id="d54db-419">LineString. IsClosed</span><span class="sxs-lookup"><span data-stu-id="d54db-419">LineString.IsClosed</span></span> | <span data-ttu-id="d54db-420">✔</span><span class="sxs-lookup"><span data-stu-id="d54db-420">✔</span></span> | <span data-ttu-id="d54db-421">✔</span><span class="sxs-lookup"><span data-stu-id="d54db-421">✔</span></span> | <span data-ttu-id="d54db-422">✔</span><span class="sxs-lookup"><span data-stu-id="d54db-422">✔</span></span> | <span data-ttu-id="d54db-423">✔</span><span class="sxs-lookup"><span data-stu-id="d54db-423">✔</span></span>
<span data-ttu-id="d54db-424">LineString. IsRing</span><span class="sxs-lookup"><span data-stu-id="d54db-424">LineString.IsRing</span></span> | <span data-ttu-id="d54db-425">✔</span><span class="sxs-lookup"><span data-stu-id="d54db-425">✔</span></span> | | <span data-ttu-id="d54db-426">✔</span><span class="sxs-lookup"><span data-stu-id="d54db-426">✔</span></span> | <span data-ttu-id="d54db-427">✔</span><span class="sxs-lookup"><span data-stu-id="d54db-427">✔</span></span>
<span data-ttu-id="d54db-428">LineString. StartPoint</span><span class="sxs-lookup"><span data-stu-id="d54db-428">LineString.StartPoint</span></span> | <span data-ttu-id="d54db-429">✔</span><span class="sxs-lookup"><span data-stu-id="d54db-429">✔</span></span> | <span data-ttu-id="d54db-430">✔</span><span class="sxs-lookup"><span data-stu-id="d54db-430">✔</span></span> | <span data-ttu-id="d54db-431">✔</span><span class="sxs-lookup"><span data-stu-id="d54db-431">✔</span></span> | <span data-ttu-id="d54db-432">✔</span><span class="sxs-lookup"><span data-stu-id="d54db-432">✔</span></span>
<span data-ttu-id="d54db-433">MultiLineString. IsClosed</span><span class="sxs-lookup"><span data-stu-id="d54db-433">MultiLineString.IsClosed</span></span> | <span data-ttu-id="d54db-434">✔</span><span class="sxs-lookup"><span data-stu-id="d54db-434">✔</span></span> | <span data-ttu-id="d54db-435">✔</span><span class="sxs-lookup"><span data-stu-id="d54db-435">✔</span></span> | <span data-ttu-id="d54db-436">✔</span><span class="sxs-lookup"><span data-stu-id="d54db-436">✔</span></span> | <span data-ttu-id="d54db-437">✔</span><span class="sxs-lookup"><span data-stu-id="d54db-437">✔</span></span>
<span data-ttu-id="d54db-438">Point. M</span><span class="sxs-lookup"><span data-stu-id="d54db-438">Point.M</span></span> | <span data-ttu-id="d54db-439">✔</span><span class="sxs-lookup"><span data-stu-id="d54db-439">✔</span></span> | <span data-ttu-id="d54db-440">✔</span><span class="sxs-lookup"><span data-stu-id="d54db-440">✔</span></span> | <span data-ttu-id="d54db-441">✔</span><span class="sxs-lookup"><span data-stu-id="d54db-441">✔</span></span> | <span data-ttu-id="d54db-442">✔</span><span class="sxs-lookup"><span data-stu-id="d54db-442">✔</span></span>
<span data-ttu-id="d54db-443">Point. X</span><span class="sxs-lookup"><span data-stu-id="d54db-443">Point.X</span></span> | <span data-ttu-id="d54db-444">✔</span><span class="sxs-lookup"><span data-stu-id="d54db-444">✔</span></span> | <span data-ttu-id="d54db-445">✔</span><span class="sxs-lookup"><span data-stu-id="d54db-445">✔</span></span> | <span data-ttu-id="d54db-446">✔</span><span class="sxs-lookup"><span data-stu-id="d54db-446">✔</span></span> | <span data-ttu-id="d54db-447">✔</span><span class="sxs-lookup"><span data-stu-id="d54db-447">✔</span></span>
<span data-ttu-id="d54db-448">Point. Y</span><span class="sxs-lookup"><span data-stu-id="d54db-448">Point.Y</span></span> | <span data-ttu-id="d54db-449">✔</span><span class="sxs-lookup"><span data-stu-id="d54db-449">✔</span></span> | <span data-ttu-id="d54db-450">✔</span><span class="sxs-lookup"><span data-stu-id="d54db-450">✔</span></span> | <span data-ttu-id="d54db-451">✔</span><span class="sxs-lookup"><span data-stu-id="d54db-451">✔</span></span> | <span data-ttu-id="d54db-452">✔</span><span class="sxs-lookup"><span data-stu-id="d54db-452">✔</span></span>
<span data-ttu-id="d54db-453">Point. Z</span><span class="sxs-lookup"><span data-stu-id="d54db-453">Point.Z</span></span> | <span data-ttu-id="d54db-454">✔</span><span class="sxs-lookup"><span data-stu-id="d54db-454">✔</span></span> | <span data-ttu-id="d54db-455">✔</span><span class="sxs-lookup"><span data-stu-id="d54db-455">✔</span></span> | <span data-ttu-id="d54db-456">✔</span><span class="sxs-lookup"><span data-stu-id="d54db-456">✔</span></span> | <span data-ttu-id="d54db-457">✔</span><span class="sxs-lookup"><span data-stu-id="d54db-457">✔</span></span>
<span data-ttu-id="d54db-458">Polygon. ExteriorRing</span><span class="sxs-lookup"><span data-stu-id="d54db-458">Polygon.ExteriorRing</span></span> | <span data-ttu-id="d54db-459">✔</span><span class="sxs-lookup"><span data-stu-id="d54db-459">✔</span></span> | <span data-ttu-id="d54db-460">✔</span><span class="sxs-lookup"><span data-stu-id="d54db-460">✔</span></span> | <span data-ttu-id="d54db-461">✔</span><span class="sxs-lookup"><span data-stu-id="d54db-461">✔</span></span> | <span data-ttu-id="d54db-462">✔</span><span class="sxs-lookup"><span data-stu-id="d54db-462">✔</span></span>
<span data-ttu-id="d54db-463">Polygon. GetInteriorRingN (int)</span><span class="sxs-lookup"><span data-stu-id="d54db-463">Polygon.GetInteriorRingN(int)</span></span> | <span data-ttu-id="d54db-464">✔</span><span class="sxs-lookup"><span data-stu-id="d54db-464">✔</span></span> | <span data-ttu-id="d54db-465">✔</span><span class="sxs-lookup"><span data-stu-id="d54db-465">✔</span></span> | <span data-ttu-id="d54db-466">✔</span><span class="sxs-lookup"><span data-stu-id="d54db-466">✔</span></span> | <span data-ttu-id="d54db-467">✔</span><span class="sxs-lookup"><span data-stu-id="d54db-467">✔</span></span>
<span data-ttu-id="d54db-468">Polygon. NumInteriorRings</span><span class="sxs-lookup"><span data-stu-id="d54db-468">Polygon.NumInteriorRings</span></span> | <span data-ttu-id="d54db-469">✔</span><span class="sxs-lookup"><span data-stu-id="d54db-469">✔</span></span> | <span data-ttu-id="d54db-470">✔</span><span class="sxs-lookup"><span data-stu-id="d54db-470">✔</span></span> | <span data-ttu-id="d54db-471">✔</span><span class="sxs-lookup"><span data-stu-id="d54db-471">✔</span></span> | <span data-ttu-id="d54db-472">✔</span><span class="sxs-lookup"><span data-stu-id="d54db-472">✔</span></span>

## <a name="additional-resources"></a><span data-ttu-id="d54db-473">Ressources supplémentaires</span><span class="sxs-lookup"><span data-stu-id="d54db-473">Additional resources</span></span>

* [<span data-ttu-id="d54db-474">Données spatiales dans SQL Server</span><span class="sxs-lookup"><span data-stu-id="d54db-474">Spatial Data in SQL Server</span></span>](https://docs.microsoft.com/sql/relational-databases/spatial/spatial-data-sql-server)
* [<span data-ttu-id="d54db-475">Page d’accueil SpatiaLite</span><span class="sxs-lookup"><span data-stu-id="d54db-475">SpatiaLite Homepage</span></span>](https://www.gaia-gis.it/fossil/libspatialite)
* [<span data-ttu-id="d54db-476">Documentation spatiale npgsql</span><span class="sxs-lookup"><span data-stu-id="d54db-476">Npgsql Spatial Documentation</span></span>](https://www.npgsql.org/efcore/mapping/nts.html)
* [<span data-ttu-id="d54db-477">Documentation PostGIS</span><span class="sxs-lookup"><span data-stu-id="d54db-477">PostGIS Documentation</span></span>](https://postgis.net/documentation/)
