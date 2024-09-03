# MySecondSymfony

Installation de la version lts avec la majorité des bibliothèques pour un site web

```symfony new MySecondSymfony --webapp --version=lts```

Mise à jour des versions de sécurités

```composer update ```

## Lancement du serveur

```symfony server:start -d```

ou

``symfony serve -d ``

Pour le fermer

``symfony server:stop``

L'adresse est généralement de type  https://127.0.0.1:8000/

## Création d'un contrôleur

``symfony console make:controller``

ou

``php bin/console make:controller NomController``

Le nom doit être en Pascal case terminé par controller, mais symfony se charge de 
le corriger en cas d'oubli
                                             
 
On va vérifier la route par défaut

``php bin/console debug:route``

## Modification de la route

```
 src/controller/HomeController.php
  #[Route('/home', name: 'app_home')]
    public function index(): Response
    {
    {
        return $this->render('home/index.html.twig', [
            'title' => 'Homepage',
        ]);
    }
 
```

On peut accéder à l'accueil depuis la racine de notre site

``https://127.0.0.1:8000/``

## Modification de 'HomeController'
```php
<?php



class HomeController extends AbstractController
{
    #[Route('/', name: 'homepage')]
    public function index(): Response
    {
        return $this->render('home/index.html.twig', [
            'title' => 'Homepage',
        ]);
    }

    #[Route('/about', name: 'about_me')]
    public function aboutMe(): Response
    {
        return $this->render('home/about.html.twig', [
            'title' => 'About me',
        ]);
    }
}

````

## Modification de base.html.twig

```twig
 {# templates/base.html.twig #}
 
  <title>{% block title %}MySecondSymfony{% endblock %}</title>
```

## Modification de index.html.twig

```twig
{% extends 'base.html.twig' %}
{# on surcharge le bloc title avec {{ parent  }} et le titre
passé par le contrôleur #}
{% block title %}{{ parent() }} | {{ title }}{% endblock %}

{% block body %}
<div class="container">
    <h1>{{ title }}</h1>
    {# Inclusion depuis la racine du projet  (templates #}
   {% include 'home/menu.html.twig' %}
</div>

{% endblock %}


```

## Création de menu.html.twig

Nous utilisons path() pour les liens vers le nom des routes pour pouvoir les changer à 
un seul endroit : 'src/controller'

```twig
<nav>
    {# On utilise path('nom du chemin') lorsqu'on veut un lien vers une autre page #}
    <a href="{{ path('homepage') }}">Homepage</a>
    <a href="{{ path('about_me') }}">About me </a>
</nav>
```


