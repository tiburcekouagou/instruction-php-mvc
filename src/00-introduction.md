## Pourquoi créer un framework de A - Z ?

Pourquoi voudrions-nous créer notre propre framework ? Quand on y pense, les gens nous dirons que ça ne sert à rien de réinventer la roue, qu'il serait préférable de choisir un framework déjà existant et d'oublier cette folie qu'est de penser créer un framework. S'ils auront certe raison la plupart du temps, voici certaines raisons de vouloir créer un framework fais-maison:

- apprendre davantage sur l'architecture de base des framework modernes;
- créer un framework sur mesure pour des besoins spécifiques;
- expérimenter la création d'un framework dans une approach learn-and-throw-away, c'est à dire dans une optique d'apprendre uniquement
- refactoriser un ancien projet qui a besoin d'une bonne dose de mise à jour suivant les dernières bonne pratique de codage;
- prouver au monde entier que vous pouvez créer un framework tout seul comme un grand (...sans même transpirer 😎)

Ce tutoriel vous guidera progressivement vers la création d'un framework. A chaque étape, vous aurez un framework fonctionnel que vous pourrez immédiatement tel quel. Nous débuterons avec un simple framework puis enventuellement nous en aurons un complet.

Et chaque étape sera l'opportunité d'en apprendre davantage sur le language PHP et les framework en général.

## Avant de commencer

Juste lire comment créer un framework ne suffit pas. Vous devrez suivre et saisir manuellement (pas de copie-coller) tous les examples inclus dans ce tutoriel. Pour cela, vous aurez besoin d'une version de PHP version 7.4 ou plus, d'un serveur comme Apache, nginx ... et d'une bonne connaissance de la programmation objet orienté PHP.

Vous êtes prêt ? C'est partie !

## Commencer

Pour créer votre nouveau framework, créez un dossier quelque part sur votre machine.
Vous pouvez le faire en utilisant l'interface graphique de votre ordinateur ou la ligne de commande linux suivante:

```shell
$ mkdir framework
$ cd framework
```

## Test
Une fois notre dossier crée, nous pouvons tester que tout va bien en ouvrant dans un éditeur de text (ex VS Code) le dossier. Commencons avec l'application web la plus simple possible:
```shell
// framework/index.php
$name = 'John Doe';

printf('Bonjour %s', $name);
```
Vous pouvez utiliser les outils tels que XAMPP, WAMPP, LAMPP, LARAGON pour lancer votre application ou vous pouvez utiliser la commande:
```shell
$ php -S localhost:8000
```