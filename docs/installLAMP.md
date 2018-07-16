# Installation de Linux-Apache-MySQL-PHP

GLPI fonctionnant depuis une interface web, il est nécessaire de lui procurer un environnement LAMP (Linux-Apache-MySQL-PHP). Il est à noter que GLPI supporte également la base de données MariaDB mais l'installation concerne ici MySQL.

## Installation d'Apache

Il faut commencer par installer Apache avec :
`sudo apt-get install apache2`.

Pour vérifier sa bonne installation, ouvrir un navigateur web et se rendre sur localhost. La page d'accueil d'Apache doit s'afficher.

## Installation de MySQL

MySQL peut être installé par la commande suivante :
`sudo apt-get install mysql-server`.

!!! note

	La version de MySQL peut être connue par `mysql -V`.

## Installation de PHP

GLPI nécessite au minimum la version 5.9 de PHP. Afin de pouvoir utiliser les commandes qui suivent il peut être utile de mettre à jour la version de Linux :
`sudo apt-get upgrade`.

Ainsi la dernière version de PHP ainsi que les modules nécessaires peuvent être installés :  
`sudo apt-get install php libapache2-mod-php php-mcrypt php-mysql php-cgi php-curl php-json`.

!!! note

	La version de PHP peut être connue par `php -v`.

## Gestion du serveur web

Les lancements automatiques du serveur Apache et de la base de données peuvent s'effectuer grâce aux commandes :  
`sudo systemctl enable apache2`  
`sudo systemctl enable mysql`.

Si le lancement automatique n'est pas souhaité il conviendra de changer le mot clé `enable` par `disable`.

Il pourra de plus être utile de relancer le serveur et la base de données lors de debug avec les commandes :  
`sudo systemctl restart apache2`  
`sudo systemctl restart mysql`.

Afin de vérifier qu'Apache est bien en cours d'exécution, de même que MySQL :  
`sudo systemctl status apache2`  
`sudo systemctl status mysql`.

## Paramétrage de la base de données

GLPI utilise les bases de données afin de stocker les inventaires et les informations annexes. Il faut pour cela lui créer une base et un utilisateur habilité à s'y connecter.  
`sudo mysql -u root -p`  
`mysql > CREATE database glpi;`  
`mysql > GRANT ALL PRIVILEGES ON glpi.* to glpiUser@localhost identified by 'mdpGlpiUser';`  
`mysql > FLUSH privileges;`  
`mysql > exit;`

Le nom d'utilisateur et le mot de passe seront à renseigner durant l'installation de GLPI.
