## Le controlleur frontal

Dans une application bien faite, un seul fichier reçoit toutes les requêtes. Ensuite, ce fichier analyse l'url de la requête puis décide quelle fonction sera exécutée: c'est le *controlleur frontal*.
Le fichier `index.php` sera notre controlleur frontal. Très souvent, c'est un fichier avec très peu de code. Il a juste besoin de 3 choses: démarrer l'application, charger les routes puis décider quelle fonction appeler.

- *Démarrer l'application*: implique qu'il faut charger toutes les fonctionalités (classes, fonctions, dépendances ...) nécessaire au bon fonctionnement de l'application;
- *charger les routes*: pour pouvoir savoir quels sont les urls que l'application accepte, et quelles fonctions exécuter quand nous recevons une requête;
- *décider quelle fonction appeler*:  selon que l'url est défini dans notre application ou pas. Par example, si nous avons prévu que l'url https://monsite.fr/contact affiche la page de contact, alors notre serveur devra toujour afficher cette dernière quand il reçoit cette requête. Par contre, si nous recevons une requête que nous n'avons pas définis (ex https://monsite.fr/nous-contacter) on devrait retourner une page '404 non trouvé'.

Ce n'est pas bien grave si vous n'arrivez pas à voir toutes les pièces du puzzle pour l'instant. L'idée, c'est d'y aller progressivement.

Donc, modifions `index.php` pour lui fournir les fonctionalités de base.
```php
# framework/index.php

require './core/Application.php';

// démarrage de l'application
$app = new Application();

// chargement des routes
$router->get('/'; function() {
    return 'Bonjour à tous';
});

// décision de quelle fonction sera exécutée
$app->run();
```

Pour le moment ce code nous génère une erreur. Notre application a besoin de définir la classe `Application` pour démarrer les fonctionalités inhérantes à notre application. Nous allons la définir en plus d'une classe `Router` pour le traitement les urls.

```php
// core/Application.php

require '../Router.php';

class Application {
    public Router $router;
    public function __construct() {
        $this->router = new Router();
    }
}
```
```php
// core/Router.php

class Router {
    public function __construct() {

    }
}
```