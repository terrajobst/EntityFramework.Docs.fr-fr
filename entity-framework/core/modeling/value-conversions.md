---
title: Conversions de valeurs - EF Core
author: ajcvickers
ms.author: divega
ms.date: 02/19/2018
ms.assetid: 3154BF3C-1749-4C60-8D51-AE86773AA116
ms.technology: entity-framework-core
uid: core/modeling/value-conversions
ms.openlocfilehash: 3e97c05a87ad9b4817c03f446031ea6c74704f5b
ms.sourcegitcommit: 605e42232854ce44bae09624a6eebc35b8e2473b
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/16/2018
---
# <a name="value-conversions"></a>Conversions de valeurs

> [!NOTE]  
> Cette fonctionnalité est une nouveauté dans EF Core 2.1.

Convertisseurs de valeur autorisent les valeurs de propriété à convertir lors de la lecture ou de l’écriture dans la base de données. Cette conversion peut être d’une valeur à un autre du même type (par exemple, les chaînes de chiffrement) ou à partir d’une valeur d’un type avec une valeur d’un autre type (par exemple, lors de la conversion valeurs enum vers et à partir de chaînes dans la base de données.)

## <a name="fundamentals"></a>Notions de base

Convertisseurs de valeurs sont spécifiées en termes d’un `ModelClrType` et un `ProviderClrType`. Le type de modèle est le type .NET de la propriété dans le type d’entité. Le type de fournisseur est le type .NET interprété par le fournisseur de base de données. Par exemple, pour enregistrer les enums sous forme de chaînes dans la base de données, le type de modèle est le type de l’énumération, et le type de fournisseur est `String`. Ces deux types peuvent être identiques.

Conversions sont définies à l’aide de deux `Func` arborescences d’expression : un dans `ModelClrType` à `ProviderClrType` et l’autre de `ProviderClrType` à `ModelClrType`. Arborescences d’expression sont utilisées afin qu’ils peuvent être compilés dans le code d’accès de base de données pour les conversions efficace. Pour les conversions complexes, l’arborescence d’expression peut être un simple appel à une méthode qui effectue la conversion.

## <a name="configuring-a-value-converter"></a>Configuration d’un convertisseur de valeurs

Conversions de valeurs sont définies sur les propriétés de la OnModelCreating de votre DbContext. Par exemple, considérez un type enum et l’entité défini en tant que :
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
Puis les conversions peuvent être définies dans OnModelCreating pour stocker les valeurs enum sous forme de chaînes (par exemple) « Âne », « Chevaux »,...) dans la base de données :
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
> A `null` valeur ne sera jamais passée à un convertisseur de valeurs. Cela facilite l’implémentation de conversions et leur permet d’être partagées entre les propriétés acceptant les valeurs NULL et non nullable.

## <a name="the-valueconverter-class"></a>La classe ValueConverter

Appel de `HasConversion` comme indiqué ci-dessus va créer un `ValueConverter` de l’instance et la définir sur la propriété. Le `ValueConverter` peut être créée à la place explicitement. Exemple :
```Csharp
var converter = new ValueConverter<EquineBeast, string>(
    v => v.ToString(),
    v => (EquineBeast)Enum.Parse(typeof(EquineBeast), v));

modelBuilder
    .Entity<Rider>()
    .Property(e => e.Mount)
    .HasConversion(converter);
```
Cela peut être utile lorsque plusieurs propriétés d’utilisent la même conversion.

> [!NOTE]  
> Il n’existe actuellement aucun moyen de spécifier au même endroit que toutes les propriétés d’un type donné doivent utiliser le même convertisseur de valeur. Cette fonctionnalité sera utilisée pour une version ultérieure.

## <a name="built-in-converters"></a>Convertisseurs intégrés

Est de base EF fourni avec un ensemble de prédéfinis `ValueConverter` classes, de la `Microsoft.EntityFrameworkCore.Storage.ValueConversion` espace de noms. Ces équivalents sont :
* `BoolToZeroOneConverter` -Bool à zéro et un
* `BoolToStringConverter` -Bool pour les chaînes telles que « Y » et « N »
* `BoolToTwoValuesConverter` -Bool toutes les deux valeurs
* `BytesToStringConverter` -Tableau d’octets chaîne codée en Base64
* `CastingConverter` -Les conversions qui nécessitent uniquement une conversion Csharp
* `CharToStringConverter` -Char en chaîne de caractères unique
* `DateTimeOffsetToBinaryConverter` -DateTimeOffset valeur codée en binaire de 64 bits
* `DateTimeOffsetToBytesConverter` -DateTimeOffset en tableau d’octets
* `DateTimeOffsetToStringConverter` -DateTimeOffset en chaîne
* `DateTimeToBinaryConverter` -DateTime en valeur 64 bits, y compris DateTimeKind
* `DateTimeToStringConverter` -DateTime en string
* `DateTimeToTicksConverter` -DateTime en graduations
* `EnumToNumberConverter` -Enum nombre sous-jacent
* `EnumToStringConverter` -Enum chaîne
* `GuidToBytesConverter` -Guid en tableau d’octets
* `GuidToStringConverter` -Guid en chaîne
* `NumberToBytesConverter` -N’importe quelle valeur numérique en tableau d’octets
* `NumberToStringConverter` -N’importe quelle valeur numérique en chaîne
* `StringToBytesConverter` -Chaîne UTF-8 octets
* `TimeSpanToStringConverter` -TimeSpan en chaîne
* `TimeSpanToTicksConverter` -Intervalle de temps en graduations

Notez que `EnumToStringConverter` est inclus dans cette liste. Cela signifie qu’il n’est pas nécessaire de spécifier la conversion explicite, comme indiqué ci-dessus. Utilisez simplement le convertisseur intégré :
```Csharp
var converter = new EnumToStringConverter<EquineBeast>();

modelBuilder
    .Entity<Rider>()
    .Property(e => e.Mount)
    .HasConversion(converter);
```
Notez que tous les convertisseurs intégrés sont sans état et par conséquent, une seule instance peut être partagée en toute sécurité par plusieurs propriétés.

## <a name="pre-defined-conversions"></a>Conversions prédéfinies

Pour les conversions courantes pour lesquelles il existe un convertisseur intégré, il est inutile de spécifier le convertisseur explicitement. Au lieu de cela, simplement configurer le type de fournisseur doit être utilisé et EF utilisent automatiquement le convertisseur de build approprié. Énumération pour les conversions de chaînes sont utilisées comme exemple ci-dessus, mais EF réellement fait automatiquement si le type de fournisseur est configuré :
```Csharp
modelBuilder
    .Entity<Rider>()
    .Property(e => e.Mount)
    .HasConversion<string>();
```
La même chose peut être obtenue en spécifiant explicitement le type de colonne. Par exemple, si le type d’entité est défini comme donc :
```Csharp
public class Rider
{
    public int Id { get; set; }

    [Column(TypeName = "nvarchar(24)")]
    public EquineBeast Mount { get; set; }
}
```
Ensuite, les valeurs enum seront enregistrés sous forme de chaînes dans la base de données sans aucune configuration supplémentaire dans OnModelCreating.

## <a name="limitations"></a>Limitations

Il existe quelques limitations actuelles connues du système de conversion de valeur :
* Comme indiqué précédemment, `null` ne peut pas être converti.
* Il n’existe actuellement aucun moyen pour répartir une conversion d’une propriété à plusieurs colonnes, ou vice versa.
* Utilisation des conversions de valeurs peut affecter la capacité de EF Core à traduire des expressions SQL. Un avertissement sera consigné pour ces cas.
La suppression de ces limitations est envisagée pour une version ultérieure.
