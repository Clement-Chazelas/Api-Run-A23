# Api Run A23



# Installation de Passbolt via Docker

J'ai voulu installer Passbolt sur mon serveur via docker. J'ai choisit de le faire à l'aide d'un docker-compose

## Processus d'Installation Suivit

J'ai suivis la documentation de Passbolt pour l'installation. Il a fallu exécuter les commandes suivantes

1. `wget "https://download.passbolt.com/ce/docker/docker-compose-ce.yaml"`

2. `wget "https://github.com/passbolt/passbolt_docker/releases/latest/download/docker-compose-ce-SHA512SUM.txt"`

3. `sha512sum -c docker-compose-ce-SHA512SUM.txt && echo "Checksum OK" || (echo "Bad checksum. Aborting" && rm -f docker-compose-ce.yaml)`

4. `docker-compose -f docker-compose-ce.yaml up -d`

J'ai renommé le fichier docker-compose-ce.yaml en compose.yml 

J'ai ici rencontré une erreur. Le port 443 configuré par défaut dans le docker-compose-ce.yaml était déjà utilisé dans mon serveur. J'ai donc changé le mapping du port 443 au port 8443 qui lui n'était pas utilisé. 
Même chose pour le port 80, qui lui a été mappé sur le port 8081 puisque les port 80 et 8080 étaient déjà utilisés

Il a ensuite fallut créer un utilisateur admin en exécutant la commande suivante :

```docker-compose -f docker-compose-ce.yaml exec passbolt su -m -c "/usr/share/php/passbolt/bin/cake \```
```passbolt register_user \```
```-u &lt;your@email.com&gt; \```
```-f &lt;yourname&gt; \```
```-l &lt;surname&gt; \```
```-r admin" -s /bin/sh www-data```

En remplaçant les champs &lt;your@email.com&gt; &lt;yourname&gt; &lt;surname&gt; respectivement par "clement.chazelas@etu.utc.fr" "Clément" "Chazelas"

J'ai ensuite configuré un reverse proxy afin de pouvoir accéder au service

On a ainsi le téléchargement de l'extension chrome à l'adresse : http://passbolt.103.picagraine.net/setup/start/b373dd96-386d-4b38-ad4b-c8e7d325d9c6/aeb8513d-69a5-4da3-b4a7-1dc3525ff76b?locale=fr-FR
(Après l'installation de l'extention cette page nous renverra directement sur notre compte passbolt)

Ainsi que l'accès à passbolt à l'adresse : http://passbolt.103.picagraine.net/


## Utilisation d'un proxy conteneurisé

J'ai également fait une version avec le reverse proxy monté sur un docker debian. Pour cela il a fallut créer une nginx.conf ainsi qu'un Dockerfile. 
Pour utiliser le service de cette manière il faut arréter le nginx local (qui tourne sur le serveur), sinon on ne pourra pas utiliser les ports. 
On éxecute donc la commande suivante :

```sudo service nginx stop```

On va ensuite ajouter une partie concernant le docker nginx dans le compose.yml. On spécifie la section build pour que le docker soit construit automatiquement à partir du Dockerfile à chaque appel de ```docker compose up```



## Pistes de solutions de sauvegarde 

1. Base de données MariaDB :  
Il faut régulièrement sauvegarder la base de données MariaDB qui stocke les données de Passbolt. Pour cela il est possible d'utiliser des outils comme mysqldump pour créer des sauvegardes SQL.

2. Volumes Docker :  
Il faut sauvegarder les volumes Docker qui stockent les données de Passbolt pour ne pas perdre les fichiers importants.

3. Images Docker :  
Il peut être interessant de sauvegarder des images Docker personnalisées, il faut donc sauvegarder les Dockerfiles et les configurations nécessaires pour les reconstruire

4. Configuration nginx :  
Il peut être intéressant de sauvegarder sa configuration nginx si elle est personnalisée afin de pouvoir la restaurer rapidement en cas de besoin

### Automatisation de la sauvegarde 

Pour automatiser le processus de sauvegarde il faudrait utiliser des scripts de sauvegarde ou planifier des tâches dans un système de gestion de tâches, par exemple cron sur Linux qui permet à l'utilisateur d'exécuter automatiquement des scripts, des commandes ou des logiciels à une date et une heure spécifiées à l'avance, ou selon un cycle défini à l'avance.




