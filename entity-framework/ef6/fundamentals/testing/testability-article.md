---
title: Testabilité et Entity Framework 4.0
author: divega
ms.date: 2016-10-23
ms.prod: entity-framework
ms.author: divega
ms.manager: avickers
ms.technology: entity-framework-6
ms.topic: article
ms.assetid: 9430e2ab-261c-4e8e-8545-2ebc52d7a247
caps.latest.revision: 3
ms.openlocfilehash: 61773f8a23ff54ddb78aeeb5656c669b12f91478
ms.sourcegitcommit: f05e7b62584cf228f17390bb086a61d505712e1b
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/08/2018
ms.locfileid: "39121187"
---
# <a name="testability-and-entity-framework-40"></a>Testabilité et Entity Framework 4.0
Scott Allen

Date de publication : Mai 2010

## <a name="introduction"></a>Introduction

Ce livre blanc décrit et montre comment écrire du code testable avec l’ADO.NET Entity Framework 4.0 et Visual Studio 2010. Ce document n’essaie pas de vous concentrer sur une méthodologie de test spécifique, telles que la conception pilotée par des tests (TDD) ou de la conception pilotée par comportement (BDD). Au lieu de cela, ce document se focalise sur la façon d’écrire du code qui utilise ADO.NET Entity Framework encore reste facile à isoler et de tester de manière automatique. Nous allons examiner les modèles de conception courants qui facilitent la scénarios de test dans les données access et voir comment appliquer ces modèles lors de l’utilisation de l’infrastructure. Nous examinerons également les fonctionnalités spécifiques de l’infrastructure pour voir comment ces fonctionnalités peuvent fonctionnent dans le code testable.

## <a name="what-is-testable-code"></a>Quel est le Code Testable ?

La possibilité de vérifier un morceau de logiciel à l’aide de tests unitaires automatisés offre de nombreux avantages souhaitables. Tout le monde sait que les bons tests réduira le nombre de défauts logiciels dans une application et l’augmentation de qualité de l’application - des tests unitaires en place va bien au-delà de trouver les bogues.

Une suite de tests unitaires corrects permet à une équipe de développement gagner du temps et gardez le contrôle des logiciels qu’ils créent. Une équipe peut apporter des modifications au code existant, refactoriser, nouvelle conception, et la restructuration des logiciels pour répondre aux nouvelles exigences, ajouter de nouveaux composants dans une application tout en connaître la suite de tests peuvent vérifier le comportement de l’application. Tests unitaires font partie d’un cycle de commentaires rapide pour faciliter la modification et de conserver la facilité de maintenance du logiciel en tant que la complexité augmente.

Tests unitaires est fourni avec un prix, toutefois. Une équipe doit consacrer du temps pour créer et gérer des tests unitaires. La quantité de l’effort requis pour créer ces tests est directement liée à la **testabilité** du logiciel sous-jacent. Simple est le logiciel à tester ? Une équipe de conception de logiciels de testabilité à l’esprit créera plus rapidement que l’équipe travaillant avec le logiciel-testable tests efficaces.

Microsoft a conçu l’ADO.NET Entity Framework 4.0 (EF4) de testabilité à l’esprit. Cela ne signifie pas que les développeurs doit écrire les tests unitaires sur le code du framework lui-même. Au lieu de cela, les objectifs de testabilité de EF4 facilitent créer du code testable qui s’appuie sur l’infrastructure. Avant d’examiner des exemples spécifiques, il est utile de comprendre les qualités de code testable.

### <a name="the-qualities-of-testable-code"></a>Les qualités de Code Testable

Code qui est facile à tester présente toujours au moins deux caractéristiques. Tout d’abord, testable code est facile à **observer**. Étant donné un ensemble d’entrées, il devrait être facile à observer la sortie du code. Par exemple, test de la méthode suivante est facile, car la méthode retourne directement le résultat d’un calcul.

``` csharp
    public int Add(int x, int y) {
        return x + y;
    }
```

Une méthode de test est difficile si la méthode écrit la valeur calculée dans un socket réseau, une table de base de données ou un fichier comme le code suivant. Le test doit effectuer un travail supplémentaire pour récupérer la valeur.

``` csharp
    public void AddAndSaveToFile(int x, int y) {
         var results = string.Format("The answer is {0}", x + y);
         File.WriteAllText("results.txt", results);
    }
```

Deuxièmement, le code testable est facile de **isoler**. Nous allons utiliser le pseudo-code suivant un mauvais exemple de code testable.

``` csharp
    public int ComputePolicyValue(InsurancePolicy policy) {
        using (var connection = new SqlConnection("dbConnection"))
        using (var command = new SqlCommand(query, connection)) {

            // business calculations omitted ...               

            if (totalValue > notificationThreshold) {
                var message = new MailMessage();
                message.Subject = "Warning!";
                var client = new SmtpClient();
                client.Send(message);
            }
        }
        return totalValue;
    }
```

La méthode est facile à observer : nous pouvons transmettre d’assurance et vérifiez la valeur de retour correspond à un résultat attendu. Toutefois, pour tester la méthode nous allons devoir disposer d’une base de données installée avec le schéma correct et configurer le serveur SMTP dans le cas où la méthode essaie d’envoyer un message électronique.

Le test unitaire uniquement besoin de vérifier la logique de calcul à l’intérieur de la méthode, mais le test peut échouer, car le serveur de messagerie est hors connexion ou de déplacer le serveur de base de données. Les deux de ces échecs ne sont pas liées au comportement que souhaite pour vérifier que le test. Le comportement est difficile à isoler.

Les développeurs de logiciels qui s’efforcent d’écrire du code testable souvent s’efforcent de maintenir une séparation des responsabilités dans le code qu’ils écrivent. La méthode ci-dessus doit se concentrer sur les calculs d’entreprise et déléguer les détails d’implémentation de base de données et de courrier électronique à d’autres composants. Martin appelle cela le principe de responsabilité unique. Un objet doit encapsuler une responsabilité unique, étroite, comme le calcul de la valeur d’une stratégie. Toutes les autres tâches de base de données et de notification doit être la responsabilité d’un autre objet. Le code écrit de cette manière est plus facile à isoler, car il se concentre sur une seule tâche.

Dans .NET, nous avons les abstractions que nous devons suivre le principe de responsabilité unique et d’isolation. Nous pouvons utiliser des définitions d’interface et forcer le code à utiliser l’abstraction de l’interface au lieu d’un type concret. Plus loin dans ce document, nous allons voir comment une méthode, telles que l’exemple incorrect présenté ci-dessus peut fonctionner avec les interfaces que *rechercher* comme ils parlera à la base de données. Au moment du test, toutefois, nous pouvons remplacer par une implémentation factice qui ne communiquer avec la base de données, mais au lieu de cela maintient les données en mémoire. Cette implémentation factice sera isoler le code à partir des problèmes non liés dans la configuration de la base de données ou un code d’accès aux données.

Il existe d’autres avantages à l’isolation. Le calcul de l’entreprise dans la dernière méthode ne devrait prendre que quelques millisecondes pour exécuter, mais le test lui-même peut s’exécuter pendant plusieurs secondes en tant que les tronçons code autour du réseau et les discussions à divers serveurs. Tests unitaires doivent s’exécuter rapidement pour faciliter les modifications mineures. Tests unitaires doivent également être reproductibles et pas échouer, car un composant non lié au test a un problème. Écriture de code qui est facile à observer et à isoler signifie que les développeurs bénéficient d’une heure plus facile l’écriture de tests pour le code, passez moins temps d’attente des tests à exécuter, et plus important encore, allégez les bogues qui n’existent pas.

J’espère que vous pouvez apprécier les avantages des tests et comprendre les qualités qui expose le code testable. Nous sommes sur le point de quelle manière d’écrire du code qui fonctionne avec EF4 pour enregistrer les données dans une base de données tout en restant observables et facile d’isoler, mais tout d’abord nous allons affiner notre pour discuter des conceptions testables pour accéder aux données.

## <a name="design-patterns-for-data-persistence"></a>Modèles de conception pour la persistance des données

Les deux exemples incorrects présentés précédemment avaient trop de responsabilités. Le premier exemple incorrect dû effectuer un calcul *et* écrire dans un fichier. Le deuxième exemple de mauvaise avait lire les données à partir d’une base de données *et* effectuent un calcul de l’entreprise *et* envoyer un courrier électronique. En concevant des méthodes plus petits qui séparent les préoccupations et déléguer la responsabilité à d’autres composants, vous allez très grandes avancées vers la rédaction de code testable. L’objectif consiste à créer des fonctionnalités en composant des actions à partir des abstractions réduites et focalisées.

Lorsqu’il s’agit de persistance des données le petit et abstractions ayant le focus, nous recherchons sont si communes qu’ils avoir été décrit en tant que modèles de conception. Livre de Martin Fowler Patterns of Enterprise Application Architecture a été le premier travail pour décrire ces modèles à l’impression. Nous vous fournirons une brève description de ces modèles dans les sections suivantes avant de vous montrer comment ces ADO.NET Entity Framework implémente et fonctionne avec ces modèles.

### <a name="the-repository-pattern"></a>Le modèle de référentiel

Fowler indique qu’un référentiel » sert d’intermédiaire entre le domaine et les données des couches de mappage à l’aide d’une interface semblable à la collection pour l’accès aux objets de domaine ». L’objectif du modèle dépôt est d’isoler le code à partir de la minutie et impact de l’accès aux données, et comme nous l’avons vu isolation antérieures est une caractéristique requise pour la testabilité.

La clé à l’isolation est comment le référentiel expose les objets à l’aide d’une interface semblable à la collection. La logique que vous écrivez pour n’utiliser le référentiel ne sait aucun comment le référentiel matérialisera les objets que vous demandez. Le référentiel peut communiquer avec une base de données, ou elle peut simplement retourner des objets à partir d’une collection en mémoire. Tout votre code doit connaître est que le référentiel s’affiche pour conserver la collection, et vous pouvez récupérer, ajouter et supprimer des objets de la collection.

Dans les applications .NET existantes un référentiel concret hérite souvent une interface générique comme suit :

``` csharp
    public interface IRepository<T> {       
        IEnumerable<T> FindAll();
        IEnumerable<T> FindBy(Expression<Func\<T, bool>> predicate);
        T FindById(int id);
        void Add(T newEntity);
        void Remove(T entity);
    }
```

Nous allons apporter quelques modifications à la définition d’interface lorsque nous fournir une implémentation pour EF4, mais le concept de base reste le même. Code peut utiliser un référentiel concrète qui implémente cette interface pour récupérer une entité par sa valeur de clé primaire, pour récupérer une collection d’entités en fonction de la version d’évaluation d’un prédicat, ou simplement récupérer toutes les entités disponibles. Le code peut également ajouter et supprimer des entités via l’interface de référentiel.

Étant donné un objets IRepository d’employé, code peut effectuer les opérations suivantes.

``` csharp
    var employeesNamedScott =
        repository
            .FindBy(e => e.Name == "Scott")
            .OrderBy(e => e.HireDate);
    var firstEmployee = repository.FindById(1);
    var newEmployee = new Employee() {/*... */};
    repository.Add(newEmployee);
```

Étant donné que le code utilise une interface (IRepository d’employé), nous pouvons fournir le code avec des implémentations différentes de l’interface. Une implémentation peut être une implémentation bénéficie d’EF4 et objets persistants dans une base de données Microsoft SQL Server. Une implémentation différente (celui utilisé pendant le test) peut être sauvegardée par un objet de liste d’employés en mémoire. L’interface vous aidera à isoler le code.

Notez le IRepository&lt;T&gt; interface n’expose pas une opération d’enregistrement. Comment nous mettre à jour les objets existants ? Vous pouvez trouver des définitions de IRepository qui n’incluent pas l’opération d’enregistrement, et les implémentations de ces référentiels devront immédiatement conserver un objet dans la base de données. Toutefois, dans de nombreuses applications, nous ne voulons de conserver des objets individuellement. Au lieu de cela, nous voulons mettre des objets à durée de vie, peut-être à partir de différents référentiels, modifier ces objets dans le cadre d’une activité d’entreprise, puis conserve tous les objets dans le cadre d’une seule opération atomique. Heureusement, il existe un modèle pour permettre ce type de comportement.

### <a name="the-unit-of-work-pattern"></a>La modèle unité de travail

Fowler indique qu’une unité de travail « conserve une liste d’objets affectés par une transaction commerciale et coordonne l’écriture en dehors des modifications et la résolution des problèmes d’accès concurrentiel ». Il incombe à l’unité de travail pour suivre les modifications apportées aux objets nous donner vie à partir d’un référentiel et conserver toutes les modifications que nous avons apportées aux objets lorsque nous indiquons à l’unité de travail pour valider les modifications. Il est également la responsabilité de l’unité de travail pour prendre les nouveaux objets que nous avons ajoutée à tous les dépôts et insérer des objets dans une base de données, ainsi que la suppression de gérer.

Si vous avez fait tout travail avec des jeux de données ADO.NET vous serez déjà familiarisé avec la modèle unité de travail. DataSets ADO.NET eu la possibilité de suivre nos mises à jour, suppressions et l’insertion d’objets DataRow et pu rapprocher (à l’aide d’un TableAdapter) tous les changements à une base de données. Toutefois, les objets de jeu de données modèle un sous-ensemble déconnecté de la base de données sous-jacente. La modèle unité de travail présente le même comportement, mais fonctionne avec des objets métier et les objets de domaine qui sont isolés à partir de code d’accès aux données et pas au courant de la base de données.

Une abstraction pour modéliser l’unité de travail dans le code .NET peut se présenter comme suit :

``` csharp
    public interface IUnitOfWork {
        IRepository<Employee> Employees { get; }
        IRepository<Order> Orders { get; }
        IRepository<Customer> Customers { get; }
        void Commit();
    }
```

En exposant les références de référentiel à partir de l’unité de travail, que nous pouvons garantir une seule unité de travail objet a la possibilité de suivre toutes les entités matérialisées pendant une transaction commerciale. L’implémentation de la méthode de validation pour une véritable unité de travail est où toute la magie se produit pour harmoniser les modifications en mémoire avec la base de données. 

Étant donné une référence IUnitOfWork, le code peut apporter des modifications à des objets métier récupérées à partir d’un ou plusieurs référentiels et enregistrer toutes les modifications à l’aide de l’opération de validation atomique.

``` csharp
    var firstEmployee = unitofWork.Employees.FindById(1);
    var firstCustomer = unitofWork.Customers.FindById(1);
    firstEmployee.Name = "Alex";
    firstCustomer.Name = "Christopher";
    unitofWork.Commit();
```

### <a name="the-lazy-load-pattern"></a>Le modèle de chargement différé

Fowler utilise le chargement différé de nom pour décrire des « un objet qui ne contient pas toutes les données vous avez besoin mais sait comment l’obtenir ». Le chargement différé transparent est une fonctionnalité importante à lorsqu’ils sont l’écriture de code testable métier et l’utilisation avec une base de données relationnelle. Par exemple, prenons le code suivant.

``` csharp
    var employee = repository.FindById(id);
    // ... and later ...
    foreach(var timeCard in employee.TimeCards) {
        // .. manipulate the timeCard
    }
```

Comment la collection de cartes de pointage est remplie ? Il existe deux réponses possibles. Une réponse est que le référentiel d’employé, lorsque vous êtes invité à extraire un employé, émet une requête pour récupérer les deux l’employé ainsi que des informations de carte de temps associée de l’employé. Dans les bases de données relationnelles en général une requête avec une clause JOIN et cela peut peut entraîner l’extraction de plus d’informations qu’une application a besoin. Que se passe-t-il si l’application doit jamais toucher la propriété de cartes de pointage ?

Une deuxième réponse consiste à charger la propriété de cartes de pointage « à la demande ». Le chargement différé est implicite et transparent pour la logique métier, car le code n’appelle pas des API spéciales pour récupérer des informations de carte de temps. Le code suppose que les informations de carte de temps sont présentes si nécessaire. Certains magique est impliqué avec le chargement différé qui implique généralement l’interception d’exécution d’appels de méthode. Le code d’interception est responsable de la communication avec la base de données et récupérer des informations de carte de temps tout en laissant la logique métier en être la logique métier. Cette commande magique de chargement différé permet au code de l’entreprise pour lui-même isoler des opérations d’extraction de données et les résultats dans plus de code testable.

L’inconvénient d’un chargement différé est que lorsqu’une application *est* besoin des informations de carte de temps le code s’exécute une requête supplémentaire. Ce n’est pas un critère important pour de nombreuses applications, mais pour les applications sensibles performances ou des applications effectuant une boucle dans un nombre d’objets de l’employé et l’exécution d’une requête pour récupérer des cartes de temps lors de chaque itération de la boucle (un problème souvent appelé N + 1 problème de requête), le chargement différé est une opération de glissement. Dans ces scénarios une application peut souhaiter vous empresser de charger les informations de carte de l’heure de la manière la plus efficace possible.

Heureusement, nous allons voir comment EF4 prend en charge les deux charges tardives implicites et effectue un chargement hâtif efficace comme nous déplacer dans la section suivante et implémenter ces modèles.

## <a name="implementing-patterns-with-the-entity-framework"></a>Implémentation de modèles avec Entity Framework

La bonne nouvelle est que tous les modèles de conception que nous avons décrit dans la dernière section sont faciles à implémenter avec EF4. Pour illustrer, nous allons utiliser une application ASP.NET MVC simple pour modifier et afficher des employés et leurs informations de carte de temps associée. Nous allons commencer à l’aide des suivante « plain old CLR objects » (OCT). 

``` csharp
    public class Employee {
        public int Id { get; set; }
        public string Name { get; set; }
        public DateTime HireDate { get; set; }
        public ICollection<TimeCard> TimeCards { get; set; }
    }

    public class TimeCard {
        public int Id { get; set; }
        public int Hours { get; set; }
        public DateTime EffectiveDate { get; set; }
    }
```

Ces définitions de classe change légèrement nous explorons les différentes approches et les fonctionnalités de EF4, mais l’objectif est de conserver ces classes ignorent la persistance (PI) que possible. Un objet PI ne sait pas *comment*, voire *si*, l’état, sa valeur est réside dans une base de données. PI et oct vont de pair avec le logiciel testable. Objets à l’aide d’une approche POCO sont moins limités, plus flexible et plus faciles à tester, car ils peuvent traiter sans une base de données actuelle.

Avec les objets oct en place, nous pouvons créer un modèle EDM (Entity Data Model) dans Visual Studio (voir figure 1). Nous n’utiliserons pas le modèle EDM pour générer le code pour nos entités. Au lieu de cela, nous souhaitons utiliser les entités que nous affectueusement abrégé créer manuellement. Nous utiliserons uniquement le modèle EDM pour générer notre schéma de base de données et fournir les métadonnées qu'ef4 a besoin de mapper des objets dans la base de données.

![eftest_01](~/ef6/media/eftest-01.jpg)

**Figure 1**

Remarque : Si vous souhaitez développer le modèle EDM tout d’abord, il est possible de générer, nettoyer, code POCO à partir du modèle EDM. Pour cela, avec une extension de Visual Studio 2010 fournie par l’équipe programmabilité des données. Pour télécharger l’extension, lancez le Gestionnaire d’extensions dans le menu Outils dans Visual Studio et rechercher dans la galerie en ligne des modèles pour « POCO » (voir figure 2). Il existe plusieurs modèles POCO pour EF. Pour plus d’informations sur l’utilisation du modèle, consultez « [procédure pas à pas : modèle de POCO pour Entity Framework](http://blogs.msdn.com/adonet/pages/walkthrough-poco-template-for-the-entity-framework.aspx)».

![eftest_02](~/ef6/media/eftest-02.png)

**Figure 2**

À partir de ce point de départ de POCO, nous allons examiner deux approches différentes pour code testable. La première approche, j’appelle l’approche Entity Framework, car il tire parti des abstractions de l’API Entity Framework pour implémenter des unités de travail et des référentiels. Dans la deuxième approche, nous créer nos propres abstractions de dépôt personnalisé et ensuite voir les avantages et inconvénients de chaque approche. Nous allons commencer en explorant l’approche Entity Framework.  

### <a name="an-ef-centric-implementation"></a>Une implémentation orientée EF

Envisagez l’action de contrôleur suivante à partir d’un projet ASP.NET MVC. L’action récupère un objet Employee et retourne un résultat pour afficher une vue détaillée de l’employé.

``` csharp
    public ViewResult Details(int id) {
        var employee = _unitOfWork.Employees
                                  .Single(e => e.Id == id);
        return View(employee);
    }
```

Le code est testable ? Il existe au moins deux tests, que nous devons vérifier le comportement de l’action. Tout d’abord, nous souhaitons vérifier que l’action retourne la bonne vue – un test simple. Il est également possible que nous voulons écrire un test pour vérifier l’action récupère l’employé correct, et nous souhaitons faire sans l’exécution de code pour interroger la base de données. Rappelez-vous que nous souhaitons isoler le code sous test. Isolation garantit que le test n’échoue en raison d’un bogue dans le code d’accès aux données ou la configuration de la base de données. Si le test échoue, nous saura que nous avons un bogue dans la logique du contrôleur et non dans un composant de système de niveau inférieur.

Pour isoler nous devrons certaines abstractions telles que les interfaces que nous avons présenté précédemment pour les référentiels et les unités de travail. N’oubliez pas que le modèle dépôt vise à servant d’intermédiaire entre les objets de domaine et de la couche de mappage de données. Dans ce scénario EF4 *est* les mappage des données de couche et fournit déjà une abstraction de type de référentiel nommée IObjectSet&lt;T&gt; (à partir de l’espace de noms System.Data.Objects). La définition d’interface est similaire à celui-ci.

``` csharp
    public interface IObjectSet<TEntity> :
                     IQueryable<TEntity>,
                     IEnumerable<TEntity>,
                     IQueryable,
                     IEnumerable
                     where TEntity : class
    {
        void AddObject(TEntity entity);
        void Attach(TEntity entity);
        void DeleteObject(TEntity entity);
        void Detach(TEntity entity);
    }
```

IObjectSet&lt;T&gt; répond à la configuration requise pour un référentiel, car il ressemble à une collection d’objets (par le biais de IEnumerable&lt;T&gt;) et fournit des méthodes pour ajouter et supprimer des objets de la collection simulée. Les méthodes d’attachement et détachement exposent des fonctionnalités supplémentaires de l’API EF4. Pour utiliser IObjectSet&lt;T&gt; en tant que l’interface pour les dépôts nous avons besoin d’une unité de travail abstraction pour lier ensemble des dépôts.

``` csharp
    public interface IUnitOfWork {
        IObjectSet<Employee> Employees { get; }
        IObjectSet<TimeCard> TimeCards { get; }
        void Commit();
    }
```

Une implémentation concrète de cette interface parlera à SQL Server et il est facile de créer en utilisant la classe ObjectContext EF4. La classe ObjectContext est la véritable unité de travail dans l’API EF4.

``` csharp
    public class SqlUnitOfWork : IUnitOfWork {
        public SqlUnitOfWork() {
            var connectionString =
                ConfigurationManager
                    .ConnectionStrings[ConnectionStringName]
                    .ConnectionString;
            _context = new ObjectContext(connectionString);
        }

        public IObjectSet<Employee> Employees {
            get { return _context.CreateObjectSet<Employee>(); }
        }

        public IObjectSet<TimeCard> TimeCards {
            get { return _context.CreateObjectSet<TimeCard>(); }
        }

        public void Commit() {
            _context.SaveChanges();
        }

        readonly ObjectContext _context;
        const string ConnectionStringName = "EmployeeDataModelContainer";
    }
```

Mettre un IObjectSet&lt;T&gt; à durée de vie est aussi simple que l’appel de la méthode CreateObjectSet de l’objet ObjectContext. Dans les coulisses, l’infrastructure utilisera les métadonnées nous fourni dans le modèle EDM pour produire un ObjectSet concrète&lt;T&gt;. Nous nous en tiendrons avec retour le IObjectSet&lt;T&gt; interface, car elle vous aide à préserver la testabilité dans le code client.

Cette implémentation concrète est utile dans la production, mais nous devons nous concentrer sur la façon dont nous allons utiliser notre abstraction IUnitOfWork pour faciliter les tests.

### <a name="the-test-doubles"></a>Les Doubles de Test

Pour isoler l’action du contrôleur, nous devrons la possibilité de basculer entre l’unité réelle de travail (pris en charge par un ObjectContext) et une unité de double ou « faux » de test de travail (en effectuant les opérations en mémoire). L’approche courante pour effectuer ce type de basculement de doit ne pas laisser le contrôleur MVC instancier une unité de travail, mais passe l’unité de travail dans le contrôleur en tant que paramètre de constructeur.

``` csharp
    class EmployeeController : Controller {
      publicEmployeeController(IUnitOfWork unitOfWork)  {
          _unitOfWork = unitOfWork;
      }
      ...
    }
```

Le code ci-dessus est un exemple d’injection de dépendances. Nous n’autorisons le contrôleur pour créer sa dépendance (l’unité de travail), mais injecter la dépendance dans le contrôleur. Dans un projet MVC, il est courant d’utiliser une fabrique de contrôleur personnalisée en association avec un conteneur d’inversion de contrôle (IoC) pour automatiser l’injection de dépendances. Ces rubriques sont dépasse le cadre de cet article, mais vous pouvez en savoir plus en suivant les références à la fin de cet article.

Une unité factice d’implémentation de travail que nous pouvons utiliser pour le test peut se présenter comme suit.

``` csharp
    public class InMemoryUnitOfWork : IUnitOfWork {
        public InMemoryUnitOfWork() {
            Committed = false;
        }
        public IObjectSet<Employee> Employees {
            get;
            set;
        }

        public IObjectSet<TimeCard> TimeCards {
            get;
            set;
        }

        public bool Committed { get; set; }
        public void Commit() {
            Committed = true;
        }
    }
```

Notez que la fausse unité de travail expose une propriété de validation. Il est parfois utile d’ajouter des fonctionnalités à une classe factice qui facilitent le test. Dans ce cas, il est facile à observer si le code valide une unité de travail en vérifiant la propriété validée.

Nous aurons également besoin d’un faux IObjectSet&lt;T&gt; pour contenir les objets Employee et TimeCard en mémoire. Nous pouvons fournir une implémentation unique de l’utilisation de génériques.

``` csharp
    public class InMemoryObjectSet<T> : IObjectSet<T> where T : class
        public InMemoryObjectSet()
            : this(Enumerable.Empty<T>()) {
        }
        public InMemoryObjectSet(IEnumerable<T> entities) {
            _set = new HashSet<T>();
            foreach (var entity in entities) {
                _set.Add(entity);
            }
            _queryableSet = _set.AsQueryable();
        }
        public void AddObject(T entity) {
            _set.Add(entity);
        }
        public void Attach(T entity) {
            _set.Add(entity);
        }
        public void DeleteObject(T entity) {
            _set.Remove(entity);
        }
        public void Detach(T entity) {
            _set.Remove(entity);
        }
        public Type ElementType {
            get { return _queryableSet.ElementType; }
        }
        public Expression Expression {
            get { return _queryableSet.Expression; }
        }
        public IQueryProvider Provider {
            get { return _queryableSet.Provider; }
        }
        public IEnumerator<T> GetEnumerator() {
            return _set.GetEnumerator();
        }
        IEnumerator IEnumerable.GetEnumerator() {
            return GetEnumerator();
        }

        readonly HashSet<T> _set;
        readonly IQueryable<T> _queryableSet;
    }
```

Cette double de test délègue la majorité de son travail à un HashSet sous-jacent&lt;T&gt; objet. Notez que IObjectSet&lt;T&gt; nécessite une contrainte générique de l’application T en tant que classe (type référence) et également nous oblige à implémenter IQueryable&lt;T&gt;. Il est facile de faire une collection en mémoire apparaître comme un IQueryable&lt;T&gt; à l’aide de l’opérateur LINQ standard AsQueryable.

### <a name="the-tests"></a>Les Tests

Tests unitaires traditionnels utilisera une seule classe de test pour contenir tous les tests pour toutes les actions dans un seul contrôleur MVC. Nous pouvons écrire ces tests, ou n’importe quel type de test unitaire, à l’aide de la mémoire dans fakes que nous avons créé. Toutefois, pour cet article, nous pouvons éviter le monolithiques approche de la classe de test et de groupe au lieu de cela nos tests se concentrer sur une partie spécifique des fonctionnalités.  Par exemple, « créer nouvel employé » peut être la fonctionnalité que nous voulons tester, et nous allons utiliser une seule classe de test pour vérifier l’action de contrôleur unique responsable de la création d’un nouvel employé.

Il existe du code le programme d’installation courants, que nous avons besoin pour toutes ces classes de test affinée. Par exemple, nous devons toujours créer nos référentiels d’en mémoire et d’un fausse unité de travail. Nous devons également une instance du contrôleur employé avec l’unité factice de travail injecté. Nous vous présenterons ce code de programme d’installation commun entre les classes de test à l’aide d’une classe de base.

``` csharp
    public class EmployeeControllerTestBase {
        public EmployeeControllerTestBase() {
            _employeeData = EmployeeObjectMother.CreateEmployees()
                                                .ToList();
            _repository = new InMemoryObjectSet<Employee>(_employeeData);
            _unitOfWork = new InMemoryUnitOfWork();
            _unitOfWork.Employees = _repository;
            _controller = new EmployeeController(_unitOfWork);
        }

        protected IList<Employee> _employeeData;
        protected EmployeeController _controller;
        protected InMemoryObjectSet<Employee> _repository;
        protected InMemoryUnitOfWork _unitOfWork;
    }
```

La « mère d’objet « nous utilisons dans la classe de base est un modèle courant pour la création de données de test. Une mère de l’objet contient des méthodes de fabrique pour instancier des entités de test à utiliser dans plusieurs contextes de test.

``` csharp
    public static class EmployeeObjectMother {
        public static IEnumerable<Employee> CreateEmployees() {
            yield return new Employee() {
                Id = 1, Name = "Scott", HireDate=new DateTime(2002, 1, 1)
            };
            yield return new Employee() {
                Id = 2, Name = "Poonam", HireDate=new DateTime(2001, 1, 1)
            };
            yield return new Employee() {
                Id = 3, Name = "Simon", HireDate=new DateTime(2008, 1, 1)
            };
        }
        // ... more fake data for different scenarios
    }
```

Nous pouvons utiliser le EmployeeControllerTestBase comme classe de base pour un nombre de contextes de test (voir figure 3). Chaque contexte de test teste une action de contrôleur spécifique. Par exemple, un contexte de test se concentrera sur test de l’action de création utilisée lors d’une demande HTTP GET (pour afficher la vue pour la création d’un employé) et une fixture différentes se concentrera sur l’action de création utilisée dans une requête HTTP POST (pour tirer des informations fournies par le utilisateur de créer un employé). Chaque classe dérivée est uniquement responsable de la configuration requise dans son contexte spécifique et de fournir les assertions que nécessaires pour vérifier les résultats pour son contexte de test spécifique.

![eftest_03](~/ef6/media/eftest-03.png)

**Figure 3**

Le style de test et de la convention d’affectation de noms présenté ici n’est pas requis pour le code testable : il s’agit simplement d’une approche. Figure 4 montre les tests en cours d’exécution dans le Resharper cerveaux Jet testent le plug-in de test runner pour Visual Studio 2010.

![eftest_04](~/ef6/media/eftest-04.png)

**Figure 4**

Avec une classe de base pour gérer le code de programme d’installation partagé, les tests unitaires pour chaque action du contrôleur sont petits et facile à écrire. Les tests seront exécute rapidement (étant donné que nous exécutons des opérations en mémoire) et ne doit pas échouer en raison d’infrastructure indépendant ou des préoccupations environnementales (étant donné que nous avons isolé le test unitaire).

``` csharp
    [TestClass]
    public class EmployeeControllerCreateActionPostTests
               : EmployeeControllerTestBase {
        [TestMethod]
        public void ShouldAddNewEmployeeToRepository() {
            _controller.Create(_newEmployee);
            Assert.IsTrue(_repository.Contains(_newEmployee));
        }
        [TestMethod]
        public void ShouldCommitUnitOfWork() {
            _controller.Create(_newEmployee);
            Assert.IsTrue(_unitOfWork.Committed);
        }
        // ... more tests

        Employee _newEmployee = new Employee() {
            Name = "NEW EMPLOYEE",
            HireDate = new System.DateTime(2010, 1, 1)
        };
    }
```

Dans ces tests, la classe de base effectue l’essentiel du travail d’installation. N’oubliez pas que le constructeur de classe de base crée le référentiel en mémoire, une fausse unité de travail et une instance de la classe EmployeeController. La classe de test dérive de cette classe de base et se concentre sur les spécificités de la méthode de création de tests. Dans ce cas les spécificités résument les étapes « organiser, agir et assert » s’affiche dans l’unité de procédure de test :

-   Créez un objet de nouvel employé pour simuler des données entrantes.
-   Appeler l’action de création de la EmployeeController et transmettez le nouvel employé.
-   Vérifier que l’action de création génère les résultats attendus (l’employé s’affiche dans le référentiel).

Nous avons créé permet de tester toutes les actions EmployeeController. Par exemple, lorsque nous écrire des tests pour l’action de l’Index du contrôleur employé nous pouvons hériter de la classe de base de test pour établir la même configuration de base pour nos tests. La classe de base créera une nouvelle fois le référentiel en mémoire, la fausse unité de travail et une instance de la EmployeeController. Les tests pour l’action Index suffit de vous concentrer sur l’appel de l’action de l’Index et les qualités du modèle de l’action de test retourne.

``` csharp
    [TestClass]
    public class EmployeeControllerIndexActionTests
               : EmployeeControllerTestBase {
        [TestMethod]
        public void ShouldBuildModelWithAllEmployees() {
            var result = _controller.Index();
            var model = result.ViewData.Model
                          as IEnumerable<Employee>;
            Assert.IsTrue(model.Count() == _employeeData.Count);
        }
        [TestMethod]
        public void ShouldOrderModelByHiredateAscending() {
            var result = _controller.Index();
            var model = result.ViewData.Model
                         as IEnumerable<Employee>;
            Assert.IsTrue(model.SequenceEqual(
                           _employeeData.OrderBy(e => e.HireDate)));
        }
        // ...
    }
```

Les tests, nous allons créer avec InMemory fakes sont orientées vers test la *état* du logiciel. Par exemple, lorsque vous testez l’action Create que nous souhaitons examiner l’état du référentiel après l’exécution de l’action create : le référentiel contient le nouvel employé ?

``` csharp
    [TestMethod]
    public void ShouldAddNewEmployeeToRepository() {
        _controller.Create(_newEmployee);
        Assert.IsTrue(_repository.Contains(_newEmployee));
    }
```

Nous examinerons plus tard interaction en fonction de test. Interaction en fonction de test vous demande si le code testé appelé les méthodes appropriées sur nos objets et passé les paramètres corrects. Pour l’instant, nous allons passer en couverture un autre modèle de conception : le chargement différé.

## <a name="eager-loading-and-lazy-loading"></a>Le chargement hâtif et le chargement différé

À un moment donné dans le site web ASP.NET MVC application que nous pourrions souhaitez afficher les informations d’un employé et inclure l’employé associé à des cartes de temps. Par exemple, nous pourrions avoir un affichage du résumé des temps carte qui affiche le nom de l’employé et le nombre total de cartes de temps dans le système. Il existe plusieurs approches pour implémenter cette fonctionnalité.

### <a name="projection"></a>Projection

Une approche facile de créer la synthèse consiste à construire un modèle dédié aux informations que nous souhaitons afficher dans la vue. Dans ce scénario, le modèle peut se présenter comme suit.

``` csharp
    public class EmployeeSummaryViewModel {
        public string Name { get; set; }
        public int TotalTimeCards { get; set; }
    }
```

Notez que le EmployeeSummaryViewModel n’est pas une entité – en d’autres termes qu’il n’est pas quelque chose que nous souhaitons conserver dans la base de données. Nous allons utiliser cette classe pour réorganiser les données dans la vue d’une manière fortement typée. Le modèle de vue est comme un transfert de données (DTO) d’objet, car il ne contient aucun comportement (méthodes aucun) : seules les propriétés. Les propriétés contiendra les données que nous devons déplacer. Il est facile d’instancier ce modèle de vue à l’aide d’opérateur de projection standard de LINQ : l’opérateur Select.

``` csharp
    public ViewResult Summary(int id) {
        var model = _unitOfWork.Employees
                               .Where(e => e.Id == id)
                               .Select(e => new EmployeeSummaryViewModel
                                  {
                                    Name = e.Name,
                                    TotalTimeCards = e.TimeCards.Count()
                                  })
                               .Single();
        return View(model);
    }
```

Il existe deux fonctions importantes pour le code ci-dessus. Tout d’abord, le code est facile à tester car il est toujours facile à observer et à isoler. L’opérateur Select fonctionne aussi bien par rapport à notre fakes en mémoire comme il le fait par rapport à l’unité de travail réelle.

``` csharp
    [TestClass]
    public class EmployeeControllerSummaryActionTests
               : EmployeeControllerTestBase {
        [TestMethod]
        public void ShouldBuildModelWithCorrectEmployeeSummary() {
            var id = 1;
            var result = _controller.Summary(id);
            var model = result.ViewData.Model as EmployeeSummaryViewModel;
            Assert.IsTrue(model.TotalTimeCards == 3);
        }
        // ...
    }
```

La deuxième fonctionnalité notable est comment le code permet d’EF4 générer une requête unique et efficace pour assembler les informations employé et carte de temps entre eux. Nous avons chargé des informations sur les employés et les informations de carte de temps dans le même objet sans utiliser des API spéciales. Le code exprimé simplement les informations nécessaires à l’aide des opérateurs LINQ standard qui fonctionnent sur les sources de données en mémoire, ainsi que des sources de données distantes. EF4 a été capable de convertir les arborescences d’expression générées par la requête LINQ et C\# compilateur dans une requête T-SQL unique et efficace.

``` SQL
    SELECT
    [Limit1].[Id] AS [Id],
    [Limit1].[Name] AS [Name],
    [Limit1].[C1] AS [C1]
    FROM (SELECT TOP (2)
      [Project1].[Id] AS [Id],
      [Project1].[Name] AS [Name],
      [Project1].[C1] AS [C1]
      FROM (SELECT
        [Extent1].[Id] AS [Id],
        [Extent1].[Name] AS [Name],
        (SELECT COUNT(1) AS [A1]
         FROM [dbo].[TimeCards] AS [Extent2]
         WHERE [Extent1].[Id] =
               [Extent2].[EmployeeTimeCard_TimeCard_Id]) AS [C1]
              FROM [dbo].[Employees] AS [Extent1]
               WHERE [Extent1].[Id] = @p__linq__0
         )  AS [Project1]
    )  AS [Limit1]
```

Il existe parfois lorsque nous ne souhaitons pas travailler avec un modèle de vue ou d’un objet DTO, mais avec des entités réelles. Lorsque nous savons que nous avons besoin d’un employé *et* fiches de pointage de l’employé, nous pouvons charger dynamiquement les données associées de manière efficace et discrète.

### <a name="explicit-eager-loading"></a>Chargement hâtif Explicit

Lorsque vous souhaitez charger dynamiquement des informations sur les entités connexes nous avons besoin d’un mécanisme pour la logique métier (ou dans ce scénario, la logique d’action de contrôleur) pour exprimer sa volonté dans le référentiel. L’ObjectQuery EF4&lt;T&gt; classe définit une méthode Include pour spécifier les objets connexes à récupérer durant une requête. N’oubliez pas l’ObjectContext EF4 expose des entités par le biais de l’ObjectSet concrète&lt;T&gt; classe qui hérite d’ObjectQuery&lt;T&gt;.  Si nous utilisions ObjectSet&lt;T&gt; références dans notre action de contrôleur que nous pourrions écrire le code suivant pour spécifient un chargement hâtif des informations de carte de temps pour chaque employé.

``` csharp
    _employees.Include("TimeCards")
              .Where(e => e.HireDate.Year > 2009);
```

Toutefois, étant donné que nous essayons de garder notre code testable, nous ne sommes pas exposer ObjectSet&lt;T&gt; à partir de la véritable classe d’unité de travail à l’extérieur. Au lieu de cela, nous nous appuyons sur le IObjectSet&lt;T&gt; interface qui est plus facile à falsifier, mais IObjectSet&lt;T&gt; ne définit pas une méthode Include. L’avantage de LINQ est que nous pouvons créer notre propre opérateur Include.

``` csharp
    public static class QueryableExtensions {
        public static IQueryable<T> Include<T>
                (this IQueryable<T> sequence, string path) {
            var objectQuery = sequence as ObjectQuery<T>;
            if(objectQuery != null)
            {
                return objectQuery.Include(path);
            }
            return sequence;
        }
    }
```

Notez cet opérateur Include est défini comme une méthode d’extension pour IQueryable&lt;T&gt; au lieu de IObjectSet&lt;T&gt;. Cela nous donne la possibilité d’utiliser la méthode avec un éventail plus large de types possibles, y compris IQueryable&lt;T&gt;, IObjectSet&lt;T&gt;, ObjectQuery&lt;T&gt;et ObjectSet&lt;T&gt;. En cas de la séquence sous-jacente n’est pas une véritable ObjectQuery EF4&lt;T&gt;, il n’existe aucun risque effectuée et l’opérateur Include est une absence d’opération. Si sous-jacent séquence *est* ObjectQuery&lt;T&gt; (ou dérivé d’ObjectQuery&lt;T&gt;), EF4 sera voir notre besoin pour les données supplémentaires et formuler SQL appropriée requête.

Avec ce nouvel opérateur en place, nous pouvons demander explicitement un chargement hâtif des informations de carte de temps à partir du référentiel.

``` csharp
    public ViewResult Index() {
        var model = _unitOfWork.Employees
                               .Include("TimeCards")
                               .OrderBy(e => e.HireDate);
        return View(model);
    }
```

Lors de l’exécuter par rapport à un ObjectContext réel, le code génère la requête unique. La requête collecte suffisamment d’informations à partir de la base de données dans un voyage pour matérialiser les objets de l’employé et entièrement remplir leur propriété de cartes de pointage.

``` SQL
    SELECT
    [Project1].[Id] AS [Id],
    [Project1].[Name] AS [Name],
    [Project1].[HireDate] AS [HireDate],
    [Project1].[C1] AS [C1],
    [Project1].[Id1] AS [Id1],
    [Project1].[Hours] AS [Hours],
    [Project1].[EffectiveDate] AS [EffectiveDate],
    [Project1].[EmployeeTimeCard_TimeCard_Id] AS [EmployeeTimeCard_TimeCard_Id]
    FROM ( SELECT
         [Extent1].[Id] AS [Id],
         [Extent1].[Name] AS [Name],
         [Extent1].[HireDate] AS [HireDate],
         [Extent2].[Id] AS [Id1],
         [Extent2].[Hours] AS [Hours],
         [Extent2].[EffectiveDate] AS [EffectiveDate],
         [Extent2].[EmployeeTimeCard_TimeCard_Id] AS
                    [EmployeeTimeCard_TimeCard_Id],
         CASE WHEN ([Extent2].[Id] IS NULL) THEN CAST(NULL AS int)
         ELSE 1 END AS [C1]
         FROM  [dbo].[Employees] AS [Extent1]
         LEFT OUTER JOIN [dbo].[TimeCards] AS [Extent2] ON [Extent1].[Id] = [Extent2].[EmployeeTimeCard_TimeCard_Id]
    )  AS [Project1]
    ORDER BY [Project1].[HireDate] ASC,
             [Project1].[Id] ASC, [Project1].[C1] ASC
```

La bonne nouvelle est que le code à l’intérieur de la méthode d’action peut encore être entièrement testée. Nous n’avez pas besoin de fournir des fonctionnalités supplémentaires pour notre fakes prendre en charge l’opérateur Include. La mauvaise nouvelle est que nous avons dû utiliser l’opérateur d’inclure dans le code que nous souhaitons conserver persistance ignorant la. Il s’agit d’un bon exemple du type de compromis, que vous devrez évaluer lors de la génération de code testable. Il est parfois lorsque vous avez besoin pour vous permettre de fuite de problèmes de persistance en dehors de l’abstraction de référentiel pour répondre aux objectifs de performances.

Le chargement hâtif consiste à chargement différé. Le moyen de chargement différé que nous faisons *pas* devez notre code d’entreprise d’annoncer explicitement la configuration requise pour les données associées. Au lieu de cela, nous utilisons nos entités dans l’application et si les données supplémentaires sont nécessaires Entity Framework charge les données à la demande.

### <a name="lazy-loading"></a>Chargement différé

Il est facile d’imaginer un scénario où nous ne savons pas ce que devez données auxquelles un élément de logique métier. Nous pourrions savons la logique a besoin d’un objet employee, mais nous pouvons créer une branche dans les différents chemins d’exécution où certaines de ces chemins d’accès nécessitent des informations de carte de temps à partir de l’employé, et d’autres pas. Scénarios comme celui-ci sont parfaits pour implicite le chargement différé, car les données apparaissent comme par magie dans une en fonction des besoins.

Le chargement différé, également appelé différé du chargement, place certaines exigences sur nos objets entité. Oct avec ignorance de persistance true pas sont confrontées les exigences à partir de la couche de persistance, mais ignorance de persistance true est pratiquement impossible à atteindre.  Au lieu de cela, nous évaluons ignorance de persistance en degrés relatifs. Il est malheureux s’il est nécessaire pour hériter d’une classe de base de persistance orientée services, ou utiliser une collection spécialisée pour effectuer le chargement différé dans oct. Heureusement, EF4 dispose d’une solution moins intrusive.

### <a name="virtually-undetectable"></a>Pratiquement indétectables

Lorsque vous utilisez des objets POCO, EF4 peut générer dynamiquement des proxys de runtime pour les entités. Ces proxys invisible encapsulent les objets oct matérialisées et fournissent des services supplémentaires en interceptant chaque propriété obtiennent et définir l’opération à effectuer du travail supplémentaire. Un de ces services est la fonctionnalité de chargement différé que nous recherchons. Un autre service est un mécanisme qui peut enregistrer quand le programme modifie les valeurs de propriété d’une entité de suivi efficace. La liste des modifications est utilisée par l’ObjectContext pendant la méthode SaveChanges pour conserver toutes les entités modifiées à l’aide des commandes de mise à jour.

Pour ces proxies de fonctionner, cependant, dont ils ont besoin d’un moyen auquel se raccorder get de propriété et les opérations de jeu sur une entité et les proxys atteignent cet objectif en substituant les membres virtuels. Par conséquent, si nous voulons qu’un chargement différé implicit et suivi des modifications efficace nous devons revenir à notre définitions de classe POCO et marquer les propriétés comme virtuels.

``` csharp
    public class Employee {
        public virtual int Id { get; set; }
        public virtual string Name { get; set; }
        public virtual DateTime HireDate { get; set; }
        public virtual ICollection<TimeCard> TimeCards { get; set; }
    }
```

Nous pouvons toujours indiquer que l’entité Employee est principalement ignorant la persistance. La seule exigence consiste à utiliser les membres virtuels, et cela n’affecte pas la testabilité du code. Nous n’avez pas besoin de dériver à partir de n’importe quelle classe de base particulière, ou même utiliser une collection spéciale dédiée à chargement différé. Comme illustré dans le code, toute classe qui implémente ICollection&lt;T&gt; est disponible pour contenir des entités associées.

Il existe également une modification mineure, que nous devons apporter à l’intérieur de notre unité de travail. Le chargement différé est *hors* par défaut lorsque vous travaillez directement avec un objet ObjectContext. Il existe une propriété que nous pouvons définir sur la propriété ContextOptions pour activer le chargement différé, et nous pouvons définir cette propriété à l’intérieur de notre unité réelle de travail si vous souhaitez activer le chargement différé partout.

``` csharp
    public class SqlUnitOfWork : IUnitOfWork {
         public SqlUnitOfWork() {
             // ...
             _context = new ObjectContext(connectionString);
             _context.ContextOptions.LazyLoadingEnabled = true;
         }    
         // ...
     }
```

Avec implicit chargement différé est activé, le code de l’application peut utiliser un employé et l’employé associé des cartes de temps tout en restant ignorent du travail requis pour Entity Framework charger les données supplémentaires.

``` csharp
    var employee = _unitOfWork.Employees
                              .Single(e => e.Id == id);
    foreach (var card in employee.TimeCards) {
        // ...
    }
```

Le chargement différé rend le code d’application plus facile à écrire, et avec la magie de proxy, le code reste complètement testable. Fakes en mémoire de l’unité de travail peuvent précharger simplement fausses entités avec des données associées si nécessaire pendant un test.

À ce stade nous porter notre attention de la création de référentiels à l’aide de IObjectSet&lt;T&gt; et examinez les abstractions pour masquer tous les signes de l’infrastructure de persistance.

## <a name="custom-repositories"></a>Dépôts personnalisés

Lorsque nous avons présenté en premier l’unité de travail du modèle de design dans cet article, que nous avons fourni un exemple de code de l’unité de travail qui peut se présenter comme. Nous allons présenter cette idée d’origine à l’aide de l’employé et le scénario de carte de temps d’employé avec que nous avons travaillé.

``` csharp
    public interface IUnitOfWork {
        IRepository<Employee> Employees { get; }
        IRepository<TimeCard> TimeCards { get;  }
        void Commit();
    }
```

La principale différence entre cette unité de travail et l’unité de travail, nous avons créé dans la dernière section est la façon dont cette unité de travail n’utilise pas les abstractions à partir de l’infrastructure d’EF4 (il n’existe aucun IObjectSet&lt;T&gt;). IObjectSet&lt;T&gt; fonctionne également comme une interface de référentiel, mais l’API qu’il expose ne peut pas aligner parfaitement avec les besoins de notre application. Dans cette approche à venir, nous représentera référentiels à l’aide d’un IRepository personnalisé&lt;T&gt; abstraction.

De nombreux développeurs qui suivent la conception pilotée par les tests, conception pilotée par comportement et la conception de méthodologies axée sur le domaine préfèrent le IRepository&lt;T&gt; approche pour plusieurs raisons. Tout d’abord, le IRepository&lt;T&gt; interface représente une couche « lutte contre la corruption ». Comme décrit par Eric Evans dans son livre de conception pilotée par domaine une couche de lutte contre la corruption conserve votre code de domaine en dehors de l’infrastructure API, comme une API de persistance. Deuxièmement, les développeurs peuvent créer des méthodes dans le référentiel qui répondent aux besoins exacts d’une application (comme il est détecté lors de l’écriture de tests). Par exemple, il faut souvent localiser une entité unique à l’aide d’une valeur d’ID, donc nous pouvons ajouter une méthode FindById à l’interface de référentiel.  Notre IRepository&lt;T&gt; définition doit ressembler à ce qui suit.

``` csharp
    public interface IRepository<T>
                    where T : class, IEntity {
        IQueryable<T> FindAll();
        IQueryable<T> FindWhere(Expression<Func\<T, bool>> predicate);
        T FindById(int id);       
        void Add(T newEntity);
        void Remove(T entity);
    }
```

Notez que nous allons déposer revenir à l’aide d’un IQueryable&lt;T&gt; interface à exposer des collections d’entités. IQueryable&lt;T&gt; permet des arborescences d’expression LINQ de flux dans le fournisseur d’EF4 et de donner le fournisseur d’une vue holistique de la requête. Une seconde option consisterait à retourner IEnumerable&lt;T&gt;, ce qui signifie que le fournisseur LINQ d’EF4 ne voient que les expressions intégrées à l’intérieur du référentiel. Tout regroupement, de classement et de projection effectuée en dehors du référentiel ne seront pas composées dans la commande SQL envoyée à la base de données, ce qui peut nuire aux performances. En revanche, un référentiel retournant uniquement IEnumerable&lt;T&gt; résultats jamais vous étonnera avec une nouvelle commande SQL. Les deux approches ne fonctionnent pas, et les deux approches restent faciles à tester.

Il est simple de fournir une implémentation unique de la IRepository&lt;T&gt; interface à l’aide de génériques et l’API ObjectContext EF4.

``` csharp
    public class SqlRepository<T> : IRepository<T>
                                    where T : class, IEntity {
        public SqlRepository(ObjectContext context) {
            _objectSet = context.CreateObjectSet<T>();
        }
        public IQueryable<T> FindAll() {
            return _objectSet;
        }
        public IQueryable<T> FindWhere(
                               Expression<Func\<T, bool>> predicate) {
            return _objectSet.Where(predicate);
        }
        public T FindById(int id) {
            return _objectSet.Single(o => o.Id == id);
        }
        public void Add(T newEntity) {
            _objectSet.AddObject(newEntity);
        }
        public void Remove(T entity) {
            _objectSet.DeleteObject(entity);
        }
        protected ObjectSet<T> _objectSet;
    }
```

Le IRepository&lt;T&gt; approche nous donne un contrôle supplémentaire sur nos requêtes, car un client doit appeler une méthode pour accéder à une entité. À l’intérieur de la méthode nous pouvons vous fournir des vérifications supplémentaires et des opérateurs LINQ pour appliquer des contraintes de l’application. Notez que l’interface a deux contraintes sur le paramètre de type générique. La première contrainte est le goût inconvénients classe requis par l’ObjectSet&lt;T&gt;, et la seconde contrainte force nos entités pour implémenter IEntity – une abstraction créée pour l’application. L’interface IEntity force les entités aient une propriété Id lisible, et nous pouvons ensuite utiliser cette propriété dans la méthode FindById. IEntity est défini par le code suivant.

``` csharp
    public interface IEntity {
        int Id { get; }
    }
```

IEntity pourrait être considéré comme une violation de petites d’ignorance de la persistance dans la mesure où notre entités sont requises pour implémenter cette interface. N’oubliez pas d’ignorance de la persistance est compromis, et pour de nombreuses fonctionnalités FindById va à compenser la contrainte imposée par l’interface. L’interface n’a aucun impact sur la testabilité.

Instanciation d’un live IRepository&lt;T&gt; nécessite un ObjectContext EF4, donc une unité concrète de l’implémentation de travail doit gérer l’instanciation.

``` csharp
    public class SqlUnitOfWork : IUnitOfWork {
        public SqlUnitOfWork() {
            var connectionString =
                ConfigurationManager
                    .ConnectionStrings[ConnectionStringName]
                    .ConnectionString;

            _context = new ObjectContext(connectionString);
            _context.ContextOptions.LazyLoadingEnabled = true;
        }

        public IRepository<Employee> Employees {
            get {
                if (_employees == null) {
                    _employees = new SqlRepository<Employee>(_context);
                }
                return _employees;
            }
        }

        public IRepository<TimeCard> TimeCards {
            get {
                if (_timeCards == null) {
                    _timeCards = new SqlRepository<TimeCard>(_context);
                }
                return _timeCards;
            }
        }

        public void Commit() {
            _context.SaveChanges();
        }

        SqlRepository<Employee> _employees = null;
        SqlRepository<TimeCard> _timeCards = null;
        readonly ObjectContext _context;
        const string ConnectionStringName = "EmployeeDataModelContainer";
    }
```

### <a name="using-the-custom-repository"></a>À l’aide du référentiel personnalisé

À l’aide de notre référentiel personnalisé n’est pas fondamentalement différent d’utiliser le référentiel selon IObjectSet&lt;T&gt;. Au lieu d’appliquer des opérateurs LINQ directement à une propriété, nous devons tout d’abord un appeler des méthodes du référentiel pour capter IQueryable&lt;T&gt; référence.

``` csharp
    public ViewResult Index() {
        var model = _repository.FindAll()
                               .Include("TimeCards")
                               .OrderBy(e => e.HireDate);
        return View(model);
    }
```

Notez que l’opérateur Include personnalisé, que nous avons implémenté précédemment fonctionnera sans modification. Méthode de FindById du référentiel supprime logique en double à partir d’actions tente de récupérer une entité unique.

``` csharp
    public ViewResult Details(int id) {
        var model = _repository.FindById(id);
        return View(model);
    }
```

Il n’existe aucune différence significative dans la testabilité des deux approches, que nous avons examiné. Nous pouvons vous fournir des implémentations factices à IRepository&lt;T&gt; en créant des classes concrètes soutenus par HashSet&lt;employé&gt; , tout comme nous l’avons fait dans la dernière section. Toutefois, certains développeurs préfèrent utiliser des objets fictifs et simuler les infrastructures d’objets au lieu de créer des substituts. Nous allons examiner à l’aide d’objets fictifs pour tester notre implémentation et traite des différences entre les objets fictifs et aux éléments fictifs dans la section suivante.

### <a name="testing-with-mocks"></a>Tests avec les simulacres

Il existe différentes approches pour créer les appels de Martin Fowler un « double de test ». Un test en double (par exemple, un film de phares double) est un objet que vous générez pour « remplacer » pour le real, les objets de production pendant les tests. Les dépôts en mémoire que nous avons créé sont des doubles de test pour les référentiels de communiquer avec SQL Server. Nous avons vu comment utiliser ces doubles de test pendant les tests unitaires pour isoler le code et conserver les tests en cours d’exécution rapide.

Les doubles de test que nous avons créé ont des implémentations réelles, l’utilisation. Dans les coulisses, chacune d’elles stocke une collection d’objets concrets, et ils seront ajouter et supprimer des objets de cette collection comme nous manipuler le référentiel pendant un test. Certains développeurs préfèrent créer leurs doubles de test ainsi – avec le code réel et les implémentations de travail.  Ces doubles de test sont ce que nous appelons *fakes*. Ils ont des implémentations de travail, mais ils ne sont pas suffisamment réels pour la production. Le référentiel factice n’écrit pas vraiment à la base de données. Le serveur SMTP faux n’envoyer un message électronique sur le réseau.

### <a name="mocks-versus-fakes"></a>Objets fictifs et Fakes

Il existe un autre type de test appelé double un *simuler*. Tandis que fakes ont des implémentations de travail, les simulacres sont fournis avec aucune implémentation. À l’aide d’une infrastructure de l’objet factice nous construire ces objets fictifs au moment de l’exécution et les utiliser comme des doubles de test. Dans cette section, nous allons utiliser open source framework Moq de simulation. Voici un exemple simple d’utilisation Moq pour créer dynamiquement un test double pour un référentiel de l’employé.

``` csharp
    Mock<IRepository<Employee>> mock =
        new Mock<IRepository<Employee>>();
    IRepository<Employee> repository = mock.Object;
    repository.Add(new Employee());
    var employee = repository.FindById(1);
```

Nous demandons un IRepository Moq&lt;employé&gt; implémentation et il génère un dynamiquement. Nous pouvons obtenir à l’objet implémentant IRepository&lt;employé&gt; en accédant à la propriété d’objet de l’objet factice&lt;T&gt; objet. Il est cet objet interne, que nous pouvons passer dans nos contrôleurs, et ils ne sont pas savoir s’il s’agit d’un double de test ou dans le référentiel réel. Comme nous serait appeler des méthodes sur un objet avec une implémentation réelle, nous pouvons appeler des méthodes sur l’objet.

Vous devez vous demandez peut-être ce que le référentiel factice faire lorsque nous appelons la méthode Add. Dans la mesure où il n’existe aucune implémentation derrière l’objet factice, ajouter ne fait rien. Il n’existe aucune collection concrets dans les coulisses tels que nous avions avec les fakes que nous avons écrit, l’employé est supprimé. Qu’en est-il de la valeur de retour de FindById ? Dans ce cas l’objet factice effectue la seule chose que faire, c'est-à-dire en retournée une valeur par défaut. Étant donné que nous allons retourner un type référence (un employé), la valeur de retour est une valeur null.

Objets fictifs peuvent sembler inutiles ; Toutefois, il existe de nouvelles fonctionnalités de simulacres, que nous n’avons pas parlé. Tout d’abord, le framework Moq enregistre tous les appels effectués sur l’objet factice. Plus loin dans le code, nous pouvons poser Moq si tout le monde appelé la méthode Add, ou si tout le monde a appelé la méthode FindById. Nous verrons plus tard comment nous pouvons utiliser cette fonctionnalité d’enregistrement de « boîte noire » dans les tests.

La deuxième fonctionnalité intéressante est comment nous pouvons utiliser Moq à programmer un objet factice avec *attentes*. Une attente indique à l’objet factice comment répondre à toute interaction donnée. Par exemple, nous pouvons programmer une attente dans notre simulacre et lui dire de retourner un objet employee lorsque quelqu'un appelle FindById. Le framework Moq utilise une API d’installation et les expressions lambda à programmer ces attentes.

``` csharp
    [TestMethod]
    public void MockSample() {
        Mock<IRepository<Employee>> mock =
            new Mock<IRepository<Employee>>();
        mock.Setup(m => m.FindById(5))
            .Returns(new Employee {Id = 5});
        IRepository<Employee> repository = mock.Object;
        var employee = repository.FindById(5);
        Assert.IsTrue(employee.Id == 5);
    }
```

Dans cet exemple, nous demandons Moq pour générer dynamiquement un référentiel, puis nous utilisons le référentiel avec une attente. L’attente indique à l’objet factice pour retourner un nouvel objet employé avec une valeur d’Id de 5 lorsque quelqu'un appelle la méthode FindById en passant une valeur de 5. Ce test réussisse, et nous n’avions pas besoin générer une implémentation complète à faux IRepository&lt;T&gt;.

Nous allons revoir les tests que nous avons écrit précédemment et reprise du travail pour utiliser les simulacres au lieu de fakes. Comme avant, nous allons utiliser une classe de base pour configurer les éléments courants de l’infrastructure que nous avons besoin pour tous les tests du contrôleur.

``` csharp
    public class EmployeeControllerTestBase {
        public EmployeeControllerTestBase() {
            _employeeData = EmployeeObjectMother.CreateEmployees()
                                                .AsQueryable();
            _repository = new Mock<IRepository<Employee>>();
            _unitOfWork = new Mock<IUnitOfWork>();
            _unitOfWork.Setup(u => u.Employees)
                       .Returns(_repository.Object);
            _controller = new EmployeeController(_unitOfWork.Object);
        }

        protected IQueryable<Employee> _employeeData;
        protected Mock<IUnitOfWork> _unitOfWork;
        protected EmployeeController _controller;
        protected Mock<IRepository<Employee>> _repository;
    }
```

Le code d’installation reste essentiellement la même. Au lieu d’utiliser fakes, nous allons utiliser Moq pour construire des objets fictifs. La classe de base fait en sorte que la simulacre unité de travail retourner un référentiel factice lorsque le code appelle la propriété d’employés. Le reste de l’Assistant Déploiement aura lieu dans les contextes de test dédié à chaque scénario spécifique. Par exemple, le contexte de test pour l’action Index va configurer le référentiel factice pour retourner une liste des employés lors de l’action appelle la méthode FindAll du référentiel fictif.

``` csharp
    [TestClass]
    public class EmployeeControllerIndexActionTests
               : EmployeeControllerTestBase {
        public EmployeeControllerIndexActionTests() {
            _repository.Setup(r => r.FindAll())
                        .Returns(_employeeData);
        }
        // .. tests
        [TestMethod]
        public void ShouldBuildModelWithAllEmployees() {
            var result = _controller.Index();
            var model = result.ViewData.Model
                          as IEnumerable<Employee>;
            Assert.IsTrue(model.Count() == _employeeData.Count());
        }
        // .. and more tests
    }
```

Nos tests ressembler à l’exception des prévisions, pour les tests que nous avions auparavant. Toutefois, avec la possibilité de l’enregistrement d’une infrastructure factice nous pouvons approche du test à partir d’un angle différent. Nous allons examiner cette nouvelle perspective dans la section suivante.

### <a name="state-versus-interaction-testing"></a>État par rapport à l’Interaction de test

Il existe différentes techniques que vous pouvez utiliser pour tester les logiciels avec des objets fictifs. Une approche consiste à utiliser l’état en fonction de test, qui est ce que nous avons fait dans ce document. État en fonction des assertions de test rend sur l’état du logiciel. Dans le dernier test nous appelé une méthode d’action sur le contrôleur et effectué une assertion sur le modèle, qu'il doit être généré. Voici quelques autres exemples d’état de test :

-   Vérifiez que le référentiel contient le nouvel objet employé après l’exécution de Create.
-   Vérifiez que le modèle conserve une liste de tous les employés après l’exécution de l’Index.
-   Vérifiez que le référentiel ne contient pas un employé donné après l’exécution de la suppression.

Une autre approche, vous verrez par des objets fictifs consiste à vérifier *interactions*. Alors que l’état en fonction des assertions de test rend sur l’état des objets, interaction en fonction des assertions de test rend sur la façon dont les objets interagissent. Exemple :

-   Vérifiez que le contrôleur appelle méthode Add du référentiel lors de l’exécution de Create.
-   Vérifiez que le contrôleur appelle la méthode FindAll du référentiel lors de l’Index s’exécute.
-   Vérifiez que le contrôleur appelle l’unité de la méthode de validation du travail pour enregistrer les modifications lors de la modification s’exécute.

Test de l’interaction nécessite souvent moins de données test, car nous ne l’exploration à l’intérieur des collections et la vérification des nombres. Par exemple, si nous savons que l’action détails appelle la méthode de FindById d’un référentiel avec la valeur correcte, puis l’action est probablement qu’elles comportent correctement. Nous pouvons vérifier ce comportement sans configurer les données de test à retourner à partir de FindById.

``` csharp
    [TestClass]
    public class EmployeeControllerDetailsActionTests
               : EmployeeControllerTestBase {
         // ...
        [TestMethod]
        public void ShouldInvokeRepositoryToFindEmployee() {
            var result = _controller.Details(_detailsId);
            _repository.Verify(r => r.FindById(_detailsId));
        }
        int _detailsId = 1;
    }
```

Le programme d’installation uniquement requis dans le contexte de test ci-dessus est le programme d’installation fourni par la classe de base. Lorsque nous appelons l’action du contrôleur, Moq enregistre les interactions avec le référentiel factice. À l’aide de l’API de Moq vérifier, nous pouvons poser Moq si le contrôleur appelé FindById avec la valeur d’ID appropriée. Si le contrôleur n’a pas appelé la méthode, ou a appelé la méthode avec une valeur de paramètre inattendu, la méthode Vérifiez lève une exception et le test échoue.

Voici un autre exemple pour vérifier que l’action Create appelle la validation sur l’unité de travail en cours.

``` csharp
    [TestMethod]
    public void ShouldCommitUnitOfWork() {
        _controller.Create(_newEmployee);
        _unitOfWork.Verify(u => u.Commit());
    }
```

L’un des risques avec les tests d’interaction sont la tendance à sur spécifient les interactions. La capacité de l’objet factice pour enregistrer et de vérifier chaque interaction avec l’objet factice ne signifie pas que le test doit tenter de vérifier chaque interaction. Certaines interactions sont des détails d’implémentation et vous devez vérifier seulement les interactions *requis* pour satisfaire le test actuel.

Le choix entre les objets fictifs ou fakes dépend en grande partie du système que vous testez et votre personnel (ou de l’équipe) Préférences. Objets fictifs peuvent réduire considérablement la quantité de code, que vous devez implémenter des doubles de test, mais pas tout le monde est à l’aise attentes en matière de programmation et la vérification des interactions.

## <a name="conclusions"></a>Conclusions

Dans ce document, nous avons présenté plusieurs approches de création de code testable lors de l’utilisation d’ADO.NET Entity Framework pour la persistance des données. Nous pouvons tirer parti des abstractions intégrées comme IObjectSet&lt;T&gt;, ou créer nos propres abstractions comme IRepository&lt;T&gt;.  Dans les deux cas, la prise en charge POCO dans ADO.NET Entity Framework 4.0 permet les consommateurs de ces abstractions de rester persistant ignorant l’et hautement testable. Fonctionnalités d’EF4 supplémentaires telles que le chargement différé implicit permet d’entreprise et d’application de service du code de fonctionner sans se préoccuper de savoir les détails d’un magasin de données relationnelles. Enfin, les abstractions que nous créons sont faciles à fictifs ou faux à l’intérieur des tests unitaires, et nous pouvons utiliser ces doubles de test pour atteindre rapides en cours d’exécution, hautement isolée et les tests fiables.

### <a name="additional-resources"></a>Ressources supplémentaires

-   Martin, « [le principe de responsabilité unique](http://www.objectmentor.com/resources/articles/srp.pdf)»
-   Martin Fowler, [catalogue de modèles](http://www.martinfowler.com/eaaCatalog/index.html) de *modèles d’Architecture d’Application Enterprise*
-   Caprio Griffin, « [l’Injection de dépendances](https://msdn.microsoft.com/magazine/cc163739.aspx)»
-   Blog de programmabilité de données, « [procédure pas à pas : développement avec Entity Framework 4.0 piloté par Test](http://blogs.msdn.com/adonet/pages/walkthrough-test-driven-development-with-the-entity-framework-4-0.aspx)».
-   Blog de programmabilité de données, « [des modèles à l’aide de référentiel et unité de travail avec Entity Framework 4.0](http://blogs.msdn.com/adonet/archive/2009/06/16/using-repository-and-unit-of-work-patterns-with-entity-framework-4-0.aspx)»
-   Dave Astels, « [BDD Intro](http://blog.daveastels.com/files/BDD_Intro.pdf)»
-   Aaron Jensen, « [introduisant des spécifications Machine](http://codebetter.com/blogs/aaron.jensen/archive/2008/05/08/introducing-machine-specifications-or-mspec-for-short.aspx)»
-   Eric Lee, « [BDD avec MSTest](http://blogs.msdn.com/elee/archive/2009/01/20/bdd-with-mstest.aspx)»
-   Eric Evans, « [Domain Driven Design](http://books.google.com/books?id=7dlaMs0SECsC&printsec=frontcover&dq=evans%20domain%20driven%20design&hl=en&ei=cHztS6C8KIaglAfA_dS1CA&sa=X&oi=book_result&ct=result&resnum=1&ved=0CCoQ6AEwAA)»
-   Martin Fowler, « [Mocks aren ' t Stubs](http://martinfowler.com/articles/mocksArentStubs.html)»
-   Martin Fowler, « [Double de Test](http://martinfowler.com/bliki/TestDouble.html)»
-   Jeremy Miller, « [état par rapport à l’Interaction test](http://codebetter.com/blogs/jeremy.miller/articles/129544.aspx)»
-   Moq, \<http://code.google.com/p/moq/>

### <a name="biography"></a>Biographie

Scott Allen est un membre du personnel technique chez Pluralsight et fondateur de OdeToCode.com. Dans les 15 années de développement de logiciels commerciaux, Scott a travaillé sur des solutions pour tous les éléments à partir de périphériques intégrés de 8 bits pour les applications web ASP.NET hautement évolutives. Vous pouvez contacter Scott sur son blog à l’adresse OdeToCode ou sur Twitter à l’adresse \< http://twitter.com/OdeToCode>.
