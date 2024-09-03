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
                                                                                                                                                                                                                                                  