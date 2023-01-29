## Intro

Cette section est plus un apparté sur la notion d'autoloading: le chargement automatique des classes PHP. Les examples cités dans cette leçon en particulier ne sont n'ont rien à voir avec notre application MVC; c'est juste à titre éducatif. Néanmoins, il est fortement recommandé de passer un peu de temps dessus, de comprendre, et de tester les différentes méthodes qui seront abordés.

## Pourquoi utiliser l'autoloading

Quand nous développons des applications PHP, nous avons souvent besoin d'utiliser des librairies tiers. Et pour les utiliser, nous avons besoin de les inclure dans notre code source en utilisant `require` ou `include`.

Les directive `require` et `include` sont bien à utiliser dans de petites applications. Par contre, au fur et à mesure que notre application grandit, le nombre de directive `require` ou `include` augmente, ce qui devient difficile à maintenir. L'autre problème qui se pose est qu'avec cette approche, nous chargeons toute une librairie dans notre application, incluant les parties que nous n'utilisons même pas. Ceci conduit à une application plus lourde que nécessaire.

Pour pallier à ce problème, il serait ideal de ne charger les classes que quand nous en avons besoin. D'où l'utilisation de l'autoloading. De manière basique, quand nous utilisons une classe dans notre application, l'autoloadeur vérifie nous l'avons chargé; sinon cette classe est chargée par l'autoloadeur. Du coup, une classe sera sera chargée à la volée quand ce sera necessaire; on parle d'*autolading*.

## Autoloading sans Composer

Nous pouvons implémenter la fonctionalité d'autoloading en PHP pure avec la fonction ```spl_autoload_register()```. Cette fonction permet de définir des fonctions qui seront exécutées quand PHP essayera de charger les classes.

Voici un example relativement simple:

```php
<?php
function custom_autoloader($class) {
    include 'lib/' . $class . '.php';
}

spl_autoload_register('custom_autoload');

$objFooBar = new FooBar();
?>
```

Dans l'example ci-dessus, nous avons définis une fonction ```custom_autoloader()``` comme notre fonction d'autoloading. Ensuite, nous essayons d'instancier la class `FooBar` qui n'est pas chargée. PHP exécutera alors notre fonction d'autoloading. Dans l'example, nous supposons que la class `FooBar` est définit dans le fichier `lib/FooBar.php`.

Sans l'autoloading, nous aurons besoin d'utiliser `require` ou `include` afin de charger la classe `FooBar`. Il faut noter que cette implémentation de l'autoloadeur est relativement simple, mais nous pouvons définir plusieurs autoloadeur pour différents type de classes.

En pratique, nous n'allons pas souvent écrire nos propres autoloadeurs. Composer s'en charge.

## Autoloading avec Composer

Premièrement, assurez-vous d'avoir [installer composer sur votre système](https://getcomposer.org/download/) afin de pouvoir continuer. Il existe 4 différentes méthodes pour charger automatiquement les classes avec Composer.

1. file autoloading ou autoloading de fichiers
2. classmap autoloading ou autoloading de classmap
3. PSR-0 autoloading
4. PSR-4 autoloading

Selon la [documentation officielle de Composer](https://getcomposer.org), l'autoloading à l'aide de la spécification PSR-4 est la plus recommandée. Nous en parlerons plus en détails plus bas. Dans cette section, nous parlerons des trois autres options.

### Le fichier composer.json

Mais avant, voyons les étapes necessaires pour pouvoir utiliser l'autoloading avec Composer.


* Créez un fichier *composer.json* à la racine de votre espace de travail
* Exécutez la commande `composer dump-autoload` pour générer les fichiers necessaires à composer pour effectuer l'autoloading
* Incluez la déclaration `require 'vendor/autoad.php'` tout en haut du fichier `index.php`.

## Autoloading: les directives de _fichiers_

Le chargement automatique des fichiers marche comme l'utilisation des directive `require` ou `include`. Il suffit à l'aide de la directive `files` de lister tous les fichiers que nous voudrons charger automatiquement.

```php
{
    "autoload": {
        "files": ["lib/Foo.php", "lib/Bar.php"]
    }
}
```

Comme vous pouvez le voir, nous pouvons fournir une liste de fichiers dans la directive `files` que nous voulons charger automatiquement avec Composer. Après avoir crée le fichier *composer.json* à la racine de notre projet, nous avons juste besoin d'exécuter la commande `composer dump-autoload` pour créer les fichiers necessaires. Ces derniers seront créés dans le dossier `vendor`. Pour finir, nous avons besoin d'inclure la déclaration `require 'vendor/autoload.php'` en haut du fichier où nous voulons autoloader (ici index.php) les fichiers avec Composer comme dans l'example ci-dessous.

```php
<?php
require 'vendor/autoload.php';

// code utilisant les classes déclarées dans les fichiers 'lib/Foo.php' ou 'lib/Bar.php'
?>
```

## Autoloading: les directive de _classmap_

L'autoloading avec les classmap est une version améliorée d'autoloading de fichiers. Vous avez juste besoin de fournir une liste de dossiers, et Composer scannera tous les fichiers à l'intérieur de ce dossiers. Pour chaque fichier, Composer fera une liste des classes définies; puis quand on aura besoin de l'une d'elles, elle sera chargée automatiquement.

Voici comment se présente un fichier *composer.json* utilisant les classmap:

```php
{
    "autoload": {
        "classmap": ["lib"]
    }
}
```

Executez la commande `composer dump-autoload`, et Composer lira les fichiers dans le dossier *lib* afin d'énumérer les classes qui y sont présentes.

## Autoloading: PSR-0

PSR-0 est un standard recommandé par le groupe PHP-FIG pour faire de l'autoloading. Selon le standard PSR-0, nous devons utiliser les espaces de nom (namespace) pour définir nos librairies. Le nom qualifié d'une classe doit désormais respecter la structure `\<Nom Vendeur>\(<Espace de Nom>\)*<Nom de Classe>`. De plus, les classes doivent être créées dans des fichiers dont la structure de dossier est pareille à celle de l'espace de nom. L'example ci-dessous est plus explicite:

```php
{
    "autoload": {
        "psr-0": {
            "Highfive\\Library": "src";
        }
    }
}
```

Pour réussir l'autoloading PSR-0, nous devons faire correspondre les espaces de nom aux dossiers. Dans l'example ci-dessus, nous disons à Composer que toutes les classes qui commencent par le namespace `Highfive\Library` devrait se trouver dans le dossier `src\Highfive\Libray`.

Par example, si nous voulons definir une classe `Foo` dans le dossier `src\Highfive\Library`, nous devons créer le fichier `src\Highfive\Library\Foo.php` comme suit:

```php
<?php
namespace Highfive\Librayr;

class Foo {
    // ...
}
?>
```

Comme vous pouvez le voir, la class `Foo` a été définis dans le namespace `Highfive\Library`. Aussi, le nom du fichier correspond au nom de la classe. Voyons comment autoloader la classe "Foo".

```php
<?php
require "vendor/autoload.php";

$objFoo = new Highfive\Library\Foo();
?>
```

Composer va charger automatiquement la classe `Foo` depuis le dossier `src\Highfive\Library`.

C'était une simple explication du chargement automatique de fichiers, de classmap and PSR-0 avec Composer. A présent, voyons comment le chargement automatique PSR-4 marche.

## Autoloading PSR-4

Dans la section précédante, nous avons discuté du chargement automatique selon le PSR-0. Le PSR-4 est similaire au PSR-0 en ce sens que nous devons utiliser les espaces de noms (namespace). Cependant, nous n'avons pas besoin de faire la structure de dossiers et les espace de noms.

En PSR-0, nous devons définir les classes dans un espace de nom qui correspond à la structure de dossier. Comme nous l'avons dis dans la section précédante, pour charger la classe `Highfive\Library\Foo`, elle doit se trouver dans le dossier `src\Highfive\Library\Foo.php`. En PSR-4, nous pouvons réduire la syntax en rendant la structure de dossier plus courte. Modifions l'example précédant et veuillons voir si vous pouvez voir la différence.

Voici à quoi ressemble le fichier `composer.json` avec le PSR-4:
```php
{
    "autoload": {
        "psr-4": {
            "Highfive\\Library\\": "src"
        }
    }
}
```

Il est important de noter que nous avons ajouter des backslash à la fin de l'espace de nom. Le code ci-dessus dis à Composer que toute classe commencant par `Highfive\Library` devrait se trouver dans le dossier **src**. Donc nous n'avons pas besoin de créer les dossier **Highfive** et **Library**. Par example, si nous utilisons la classe `Highfive\Library\Foo`, Composer cherchera à charger le fichier `src\Foo.php`.

Il est important de comprendre que la classe `Foo` est toujours définit dans le namespace `Highfive\Library`; sauf que nous n'avons plus besoin de créer les dossiers qui mimiquent l'espace de nom.  Le contenu du fichier `src\Foo.php` sera identifque à celui du fichier `src\Highfive\Library\Foo.php` de la section précédante.

Comme vous pouvez le voir, PSR-4 fournir une structure de dossier plus simple, vu que nous pouvons ommettre de créer des dossiers en plus. C'est d'ailleur le standard le plus accepté dans la communauté PHP. Et c'est elle que nous utiliserons dans notre architecture MVC.