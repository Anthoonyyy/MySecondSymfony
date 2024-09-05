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


Le fichier '.env' est le fichier de configuration qui est mis sur 'git' donc 'github'

```'.env.local'```
```cp .env .env.local```

Ouvrir ``.env.local``

Changez cette ligne 

``App_ENV=dev`` en `App_ENV=prod`
``APP_SECRET=une_autre_clef_securise``

Si on retape `php bin/console debug:route`

On ne retrouve plus que les routes de production

Dans le fichier `.env.local`

trouvez la ligne de base de donnée : 

```bash
# ne pas oublier de remettre ``App_ENV=prod`` en `App_ENV=dev`
# DATABASE_URL="mysql://app:!ChangeMe!@127.0.0.1:3306/app?serverVersion=8.0.32&charset=utf8mb4"
# DATABASE_URL="mysql://app:!ChangeMe!@127.0.0.1:3306/app?serverVersion=10.11.2-MariaDB&charset=utf8mb4"
DATABASE_URL="postgresql://app:!ChangeMe!@127.0.0.1:5432/app?serverVersion=16&charset=utf8"
```

Commentez la ligne postgresql et décommenter la ligne mySQL

Ensuite, passer les paramètres de connexion dans l'ordre :

utilisateur:mot_de_passe@ip_serveur:port/nomdeladb?options

```bash
 DATABASE_URL="mysql://root:@127.0.0.1:3306/mysecondsymfony?serverVersion=8.0.31&charset=utf8mb4"
# DATABASE_URL="mysql://app:!ChangeMe!@127.0.0.1:3306/app?serverVersion=10.11.2-MariaDB&charset=utf8mb4"
# DATABASE_URL="postgresql://app:!ChangeMe!@127.0.0.1:5432/app?serverVersion=16&charset=utf8"
```

## Création de la DB

`php bin/console doctrine:database:create`

La base de donnée devrait être créé si mysql.exe est activé ou wamp démarré

## Création d'une entité

Une entité est la représentation objet d'un élément de sauvegarde de données, dans notre cas, en choisissant mysql, il s'agira d'une table

`php bin/console make:entity`

## Première migration

`` php bin/console make:migration``

puis ``php bin/console doctrine:migrations:migrate``

## Ajout de champs à l'entité Article

On utilise maker pour ça

``php bin/console make:entity Article``


```php
class Article
{
    
    # Getters et setters sauf pour les booleans

    public function getId(): ?int
    {
        return $this->id;
    }

    public function getTitle(): ?string
    {
        return $this->title;
    }

    public function setTitle(string $title): static
    {
        $this->title = $title;

        return $this;
    }

    public function getText(): ?string
    {
        return $this->text;
    }

    public function setText(string $text): static
    {
        $this->text = $text;

        return $this;
    }

    public function getDateCreated(): ?\DateTimeInterface
    {
        return $this->date_created;
    }

    public function setDateCreated(?\DateTimeInterface $date_created): static
    {
        $this->date_created = $date_created;

        return $this;
    }

    public function getDatePublished(): ?\DateTimeInterface
    {
        return $this->date_published;
    }

    public function setDatePublished(?\DateTimeInterface $date_published): static
    {
        $this->date_published = $date_published;

        return $this;
    }
 // ! pour boolean, is est rajouté pour le getter
 // bug si on avait choisi isPublished -> isPublished
    public function isPublished(): ?bool
    {
        return $this->published;
    }
 // le setter peut bug si on avait choisi comme nom isPublished
    public function setPublished(?bool $published): static
    {
        $this->published = $published;

        return $this;
    }
}
```
### On veut adapter la table à mySQL

La documentation 