---
title: Conversions de valeurs - EF Core
author: ajcvickers
ms.date: 02/19/2018
ms.assetid: 3154BF3C-1749-4C60-8D51-AE86773AA116
uid: core/modeling/value-conversions
ms.openlocfilehash: 2a1956221ecc920feba796e4d95cc97259e89c53
ms.sourcegitcommit: 0cef7d448e1e47bdb333002e2254ed42d57b45b6
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/29/2018
ms.locfileid: "43152508"
---
# <a name="value-conversions"></a><span data-ttu-id="c7f16-102">Conversions de valeurs</span><span class="sxs-lookup"><span data-stu-id="c7f16-102">Value Conversions</span></span>

> [!NOTE]  
> <span data-ttu-id="c7f16-103">Cette fonctionnalité est une nouveauté d’EF Core 2.1.</span><span class="sxs-lookup"><span data-stu-id="c7f16-103">This feature is new in EF Core 2.1.</span></span>

<span data-ttu-id="c7f16-104">Convertisseurs de valeur autorisent les valeurs de propriété à convertir lors de la lecture ou écriture à la base de données.</span><span class="sxs-lookup"><span data-stu-id="c7f16-104">Value converters allow property values to be converted when reading from or writing to the database.</span></span> <span data-ttu-id="c7f16-105">Cette conversion peut être d’une valeur à l’autre du même type (par exemple, les chaînes de chiffrement) ou à partir d’une valeur d’un type à une valeur d’un autre type (par exemple, conversion valeurs enum vers et à partir de chaînes dans la base de données.)</span><span class="sxs-lookup"><span data-stu-id="c7f16-105">This conversion can be from one value to another of the same type (for example, encrypting strings) or from a value of one type to a value of another type (for example, converting enum values to and from strings in the database.)</span></span>

## <a name="fundamentals"></a><span data-ttu-id="c7f16-106">Notions de base</span><span class="sxs-lookup"><span data-stu-id="c7f16-106">Fundamentals</span></span>

<span data-ttu-id="c7f16-107">Convertisseurs de valeurs sont spécifiées en termes d’un `ModelClrType` et un `ProviderClrType`.</span><span class="sxs-lookup"><span data-stu-id="c7f16-107">Value converters are specified in terms of a `ModelClrType` and a `ProviderClrType`.</span></span> <span data-ttu-id="c7f16-108">Le type de modèle est le type .NET de la propriété dans le type d’entité.</span><span class="sxs-lookup"><span data-stu-id="c7f16-108">The model type is the .NET type of the property in the entity type.</span></span> <span data-ttu-id="c7f16-109">Le type de fournisseur est le type .NET interprété par le fournisseur de base de données.</span><span class="sxs-lookup"><span data-stu-id="c7f16-109">The provider type is the .NET type understood by the database provider.</span></span> <span data-ttu-id="c7f16-110">Par exemple, pour enregistrer des enums sous forme de chaînes dans la base de données, le type de modèle est le type de l’énumération, et le type de fournisseur est `String`.</span><span class="sxs-lookup"><span data-stu-id="c7f16-110">For example, to save enums as strings in the database, the model type is the type of the enum, and the provider type is `String`.</span></span> <span data-ttu-id="c7f16-111">Ces deux types peuvent être identiques.</span><span class="sxs-lookup"><span data-stu-id="c7f16-111">These two types can be the same.</span></span>

<span data-ttu-id="c7f16-112">Conversions sont définies à l’aide de deux `Func` arborescences : un dans `ModelClrType` à `ProviderClrType` et l’autre à partir de `ProviderClrType` à `ModelClrType`.</span><span class="sxs-lookup"><span data-stu-id="c7f16-112">Conversions are defined using two `Func` expression trees: one from `ModelClrType` to `ProviderClrType` and the other from `ProviderClrType` to `ModelClrType`.</span></span> <span data-ttu-id="c7f16-113">Arborescences d’expressions sont utilisées afin qu’ils peuvent être compilés dans le code d’accès de base de données pour les conversions efficace.</span><span class="sxs-lookup"><span data-stu-id="c7f16-113">Expression trees are used so that they can be compiled into the database access code for efficient conversions.</span></span> <span data-ttu-id="c7f16-114">Pour les conversions complexes, l’arborescence d’expression peut être un simple appel à une méthode qui effectue la conversion.</span><span class="sxs-lookup"><span data-stu-id="c7f16-114">For complex conversions, the expression tree may be a simple call to a method that performs the conversion.</span></span>

## <a name="configuring-a-value-converter"></a><span data-ttu-id="c7f16-115">Configuration d’un convertisseur de valeurs</span><span class="sxs-lookup"><span data-stu-id="c7f16-115">Configuring a value converter</span></span>

<span data-ttu-id="c7f16-116">Conversions de valeurs sont définies sur les propriétés dans le `OnModelCreating` de votre `DbContext`.</span><span class="sxs-lookup"><span data-stu-id="c7f16-116">Value conversions are defined on properties in the `OnModelCreating` of your `DbContext`.</span></span> <span data-ttu-id="c7f16-117">Par exemple, considérez un type enum et une entité défini en tant que :</span><span class="sxs-lookup"><span data-stu-id="c7f16-117">For example, consider an enum and entity type defined as:</span></span>
``` csharp
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
<span data-ttu-id="c7f16-118">Conversions peuvent être défini dans `OnModelCreating` pour stocker les valeurs d’énumération sous forme de chaînes (par exemple, « Donkey », « Chevaux »,...) dans la base de données :</span><span class="sxs-lookup"><span data-stu-id="c7f16-118">Then conversions can be defined in `OnModelCreating` to store the enum values as strings (for example, "Donkey", "Mule", ...) in the database:</span></span>
``` csharp
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
> <span data-ttu-id="c7f16-119">Un `null` valeur ne sera jamais passée à un convertisseur de valeur.</span><span class="sxs-lookup"><span data-stu-id="c7f16-119">A `null` value will never be passed to a value converter.</span></span> <span data-ttu-id="c7f16-120">Cela facilite l’implémentation de conversions et leur permet d’être partagées entre des propriétés nullables et non nullable.</span><span class="sxs-lookup"><span data-stu-id="c7f16-120">This makes the implementation of conversions easier and allows them to be shared amongst nullable and non-nullable properties.</span></span>

## <a name="the-valueconverter-class"></a><span data-ttu-id="c7f16-121">La classe ValueConverter</span><span class="sxs-lookup"><span data-stu-id="c7f16-121">The ValueConverter class</span></span>

<span data-ttu-id="c7f16-122">Appel `HasConversion` comme indiqué ci-dessus créera un `ValueConverter` de l’instance et le définir sur la propriété.</span><span class="sxs-lookup"><span data-stu-id="c7f16-122">Calling `HasConversion` as shown above will create a `ValueConverter` instance and set it on the property.</span></span> <span data-ttu-id="c7f16-123">Le `ValueConverter` à la place peuvent être créés explicitement.</span><span class="sxs-lookup"><span data-stu-id="c7f16-123">The `ValueConverter` can instead be created explicitly.</span></span> <span data-ttu-id="c7f16-124">Exemple :</span><span class="sxs-lookup"><span data-stu-id="c7f16-124">For example:</span></span>
``` csharp
var converter = new ValueConverter<EquineBeast, string>(
    v => v.ToString(),
    v => (EquineBeast)Enum.Parse(typeof(EquineBeast), v));

modelBuilder
    .Entity<Rider>()
    .Property(e => e.Mount)
    .HasConversion(converter);
```
<span data-ttu-id="c7f16-125">Cela peut être utile lorsque plusieurs propriétés utilisent la même conversion.</span><span class="sxs-lookup"><span data-stu-id="c7f16-125">This can be useful when multiple properties use the same conversion.</span></span>

> [!NOTE]  
> <span data-ttu-id="c7f16-126">Il n’existe actuellement aucun moyen de spécifier au même endroit que chaque propriété d’un type donné doive utiliser le même convertisseur de valeurs.</span><span class="sxs-lookup"><span data-stu-id="c7f16-126">There is currently no way to specify in one place that every property of a given type must use the same value converter.</span></span> <span data-ttu-id="c7f16-127">Cette fonctionnalité sera considérée pour une version ultérieure.</span><span class="sxs-lookup"><span data-stu-id="c7f16-127">This feature will be considered for a future release.</span></span>

## <a name="built-in-converters"></a><span data-ttu-id="c7f16-128">Convertisseurs intégrés</span><span class="sxs-lookup"><span data-stu-id="c7f16-128">Built-in converters</span></span>

<span data-ttu-id="c7f16-129">EF Core est fourni avec un ensemble de prédéfinis `ValueConverter` classes, figurant dans le `Microsoft.EntityFrameworkCore.Storage.ValueConversion` espace de noms.</span><span class="sxs-lookup"><span data-stu-id="c7f16-129">EF Core ships with a set of pre-defined `ValueConverter` classes, found in the `Microsoft.EntityFrameworkCore.Storage.ValueConversion` namespace.</span></span> <span data-ttu-id="c7f16-130">Ces équivalents sont :</span><span class="sxs-lookup"><span data-stu-id="c7f16-130">These are:</span></span>
* <span data-ttu-id="c7f16-131">`BoolToZeroOneConverter` -Bool à zéro et un</span><span class="sxs-lookup"><span data-stu-id="c7f16-131">`BoolToZeroOneConverter` - Bool to zero and one</span></span>
* <span data-ttu-id="c7f16-132">`BoolToStringConverter` -Bool pour les chaînes telles que « Y » et « N »</span><span class="sxs-lookup"><span data-stu-id="c7f16-132">`BoolToStringConverter` - Bool to strings such as "Y" and "N"</span></span>
* <span data-ttu-id="c7f16-133">`BoolToTwoValuesConverter` -Bool aux deux valeurs</span><span class="sxs-lookup"><span data-stu-id="c7f16-133">`BoolToTwoValuesConverter` - Bool to any two values</span></span>
* <span data-ttu-id="c7f16-134">`BytesToStringConverter` -Tableau d’octets en chaîne codée en Base64</span><span class="sxs-lookup"><span data-stu-id="c7f16-134">`BytesToStringConverter` - Byte array to Base64-encoded string</span></span>
* <span data-ttu-id="c7f16-135">`CastingConverter` -Les conversions qui requièrent uniquement un cast de type</span><span class="sxs-lookup"><span data-stu-id="c7f16-135">`CastingConverter` - Conversions that require only a type cast</span></span>
* <span data-ttu-id="c7f16-136">`CharToStringConverter` -Char en chaîne de caractères unique</span><span class="sxs-lookup"><span data-stu-id="c7f16-136">`CharToStringConverter` - Char to single character string</span></span>
* <span data-ttu-id="c7f16-137">`DateTimeOffsetToBinaryConverter` -DateTimeOffset par rapport à la valeur de 64 bits codée en binaire</span><span class="sxs-lookup"><span data-stu-id="c7f16-137">`DateTimeOffsetToBinaryConverter` - DateTimeOffset to binary-encoded 64-bit value</span></span>
* <span data-ttu-id="c7f16-138">`DateTimeOffsetToBytesConverter` -DateTimeOffset en tableau d’octets</span><span class="sxs-lookup"><span data-stu-id="c7f16-138">`DateTimeOffsetToBytesConverter` - DateTimeOffset to byte array</span></span>
* <span data-ttu-id="c7f16-139">`DateTimeOffsetToStringConverter` -DateTimeOffset par rapport à la chaîne</span><span class="sxs-lookup"><span data-stu-id="c7f16-139">`DateTimeOffsetToStringConverter` - DateTimeOffset to string</span></span>
* <span data-ttu-id="c7f16-140">`DateTimeToBinaryConverter` -DateTime en valeur 64 bits, y compris DateTimeKind</span><span class="sxs-lookup"><span data-stu-id="c7f16-140">`DateTimeToBinaryConverter` - DateTime to 64-bit value including DateTimeKind</span></span>
* <span data-ttu-id="c7f16-141">`DateTimeToStringConverter` -DateTime en chaîne</span><span class="sxs-lookup"><span data-stu-id="c7f16-141">`DateTimeToStringConverter` - DateTime to string</span></span>
* <span data-ttu-id="c7f16-142">`DateTimeToTicksConverter` -DateTime en graduations</span><span class="sxs-lookup"><span data-stu-id="c7f16-142">`DateTimeToTicksConverter` - DateTime to ticks</span></span>
* <span data-ttu-id="c7f16-143">`EnumToNumberConverter` -Enum nombre sous-jacent</span><span class="sxs-lookup"><span data-stu-id="c7f16-143">`EnumToNumberConverter` - Enum to underlying number</span></span>
* <span data-ttu-id="c7f16-144">`EnumToStringConverter` -Enum chaîne</span><span class="sxs-lookup"><span data-stu-id="c7f16-144">`EnumToStringConverter` - Enum to string</span></span>
* <span data-ttu-id="c7f16-145">`GuidToBytesConverter` -Guid en tableau d’octets</span><span class="sxs-lookup"><span data-stu-id="c7f16-145">`GuidToBytesConverter` - Guid to byte array</span></span>
* <span data-ttu-id="c7f16-146">`GuidToStringConverter` -Guid en chaîne</span><span class="sxs-lookup"><span data-stu-id="c7f16-146">`GuidToStringConverter` - Guid to string</span></span>
* <span data-ttu-id="c7f16-147">`NumberToBytesConverter` -N’importe quelle valeur numérique en tableau d’octets</span><span class="sxs-lookup"><span data-stu-id="c7f16-147">`NumberToBytesConverter` - Any numerical value to byte array</span></span>
* <span data-ttu-id="c7f16-148">`NumberToStringConverter` -N’importe quelle valeur numérique en chaîne</span><span class="sxs-lookup"><span data-stu-id="c7f16-148">`NumberToStringConverter` - Any numerical value to string</span></span>
* <span data-ttu-id="c7f16-149">`StringToBytesConverter` -Chaîne UTF-8 octets</span><span class="sxs-lookup"><span data-stu-id="c7f16-149">`StringToBytesConverter` - String to UTF8 bytes</span></span>
* <span data-ttu-id="c7f16-150">`TimeSpanToStringConverter` -TimeSpan en chaîne</span><span class="sxs-lookup"><span data-stu-id="c7f16-150">`TimeSpanToStringConverter` - TimeSpan to string</span></span>
* <span data-ttu-id="c7f16-151">`TimeSpanToTicksConverter` -Intervalle de temps en graduations</span><span class="sxs-lookup"><span data-stu-id="c7f16-151">`TimeSpanToTicksConverter` - TimeSpan to ticks</span></span>

<span data-ttu-id="c7f16-152">Notez que `EnumToStringConverter` est inclus dans cette liste.</span><span class="sxs-lookup"><span data-stu-id="c7f16-152">Notice that `EnumToStringConverter` is included in this list.</span></span> <span data-ttu-id="c7f16-153">Cela signifie qu’il est inutile de spécifier la conversion explicitement, comme indiqué ci-dessus.</span><span class="sxs-lookup"><span data-stu-id="c7f16-153">This means that there is no need to specify the conversion explicitly, as shown above.</span></span> <span data-ttu-id="c7f16-154">Au lieu de cela, utilisez simplement le convertisseur intégré :</span><span class="sxs-lookup"><span data-stu-id="c7f16-154">Instead, just use the built-in converter:</span></span>
``` csharp
var converter = new EnumToStringConverter<EquineBeast>();

modelBuilder
    .Entity<Rider>()
    .Property(e => e.Mount)
    .HasConversion(converter);
```
<span data-ttu-id="c7f16-155">Notez que tous les convertisseurs intégrés sont sans état, et donc une seule instance peut être partagée en toute sécurité par plusieurs propriétés.</span><span class="sxs-lookup"><span data-stu-id="c7f16-155">Note that all the built-in converters are stateless and so a single instance can be safely shared by multiple properties.</span></span>

## <a name="pre-defined-conversions"></a><span data-ttu-id="c7f16-156">Conversions prédéfinies</span><span class="sxs-lookup"><span data-stu-id="c7f16-156">Pre-defined conversions</span></span>

<span data-ttu-id="c7f16-157">Pour les conversions courantes pour lesquelles il existe un convertisseur intégré, il est inutile de spécifier le convertisseur explicitement.</span><span class="sxs-lookup"><span data-stu-id="c7f16-157">For common conversions for which a built-in converter exists there is no need to specify the converter explicitly.</span></span> <span data-ttu-id="c7f16-158">Au lieu de cela, il suffit de configurer le type de fournisseur doit être utilisé et Entity Framework l’utilisera automatiquement le convertisseur intégré approprié.</span><span class="sxs-lookup"><span data-stu-id="c7f16-158">Instead, just configure which provider type should be used and EF will automatically use the appropriate built-in converter.</span></span> <span data-ttu-id="c7f16-159">Énumération pour les conversions de chaîne sont utilisés comme un exemple ci-dessus, mais EF permettra de faire cela automatiquement si le type de fournisseur est configuré :</span><span class="sxs-lookup"><span data-stu-id="c7f16-159">Enum to string conversions are used as an example above, but EF will actually do this automatically if the provider type is configured:</span></span>
``` csharp
modelBuilder
    .Entity<Rider>()
    .Property(e => e.Mount)
    .HasConversion<string>();
```
<span data-ttu-id="c7f16-160">La même chose peut être obtenue en spécifiant explicitement le type de colonne.</span><span class="sxs-lookup"><span data-stu-id="c7f16-160">The same thing can be achieved by explicitly specifying the column type.</span></span> <span data-ttu-id="c7f16-161">Par exemple, si le type d’entité est défini comme donc :</span><span class="sxs-lookup"><span data-stu-id="c7f16-161">For example, if the entity type is defined like so:</span></span>
``` csharp
public class Rider
{
    public int Id { get; set; }

    [Column(TypeName = "nvarchar(24)")]
    public EquineBeast Mount { get; set; }
}
```
<span data-ttu-id="c7f16-162">Puis les valeurs enum seront enregistrés sous forme de chaînes dans la base de données sans aucune configuration supplémentaire dans `OnModelCreating`.</span><span class="sxs-lookup"><span data-stu-id="c7f16-162">Then the enum values will be saved as strings in the database without any further configuration in `OnModelCreating`.</span></span>

## <a name="limitations"></a><span data-ttu-id="c7f16-163">Limitations</span><span class="sxs-lookup"><span data-stu-id="c7f16-163">Limitations</span></span>

<span data-ttu-id="c7f16-164">Il existe quelques limitations actuelles connues du système de conversion de valeur :</span><span class="sxs-lookup"><span data-stu-id="c7f16-164">There are a few known current limitations of the value conversion system:</span></span>
* <span data-ttu-id="c7f16-165">Comme indiqué ci-dessus, `null` ne peut pas être converti.</span><span class="sxs-lookup"><span data-stu-id="c7f16-165">As noted above, `null` cannot be converted.</span></span>
* <span data-ttu-id="c7f16-166">Il n’existe actuellement aucun moyen de se propager une conversion d’une propriété à plusieurs colonnes, ou vice versa.</span><span class="sxs-lookup"><span data-stu-id="c7f16-166">There is currently no way to spread a conversion of one property to multiple columns or vice-versa.</span></span>
* <span data-ttu-id="c7f16-167">Utilisation de conversions de valeurs peut impacter la capacité d’EF Core pour traduire des expressions to SQL.</span><span class="sxs-lookup"><span data-stu-id="c7f16-167">Use of value conversions may impact the ability of EF Core to translate expressions to SQL.</span></span> <span data-ttu-id="c7f16-168">Un avertissement est enregistré dans ces cas.</span><span class="sxs-lookup"><span data-stu-id="c7f16-168">A warning will be logged for such cases.</span></span>
<span data-ttu-id="c7f16-169">Suppression de ces limitations est envisagée pour une version ultérieure.</span><span class="sxs-lookup"><span data-stu-id="c7f16-169">Removal of these limitations is being considered for a future release.</span></span>
