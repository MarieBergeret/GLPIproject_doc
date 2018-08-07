# Déploiement des agents FusionInventory

Les agents permettent de récupérer les informations d'un ordinateur grâce au protocole SNMP. Dans un premier temps, un agent doit être installé sur le serveur Linux. La procédure d'installation est disponible pour les différents OS sur le site de FusionInventory. Ici, [celle-ci](http://fusioninventory.org/documentation/agent/installation/linux/deb.html) est utilisée.

De nombreuses dépendances sont nécessaires au fonctionnement de l'agent :  
`sudo apt -y install dmidecode hwdata ucf hdparm`  
`sudo apt -y install perl libuniversal-require-perl libwww-perl libparse-edid-perl`  
`sudo apt -y install libproc-daemon-perl libfile-which-perl`  
`sudo apt -y install libxml-treepp-perl libyaml-perl libnet-cups-perl libnet-ip-perl`  
`sudo apt -y install libdigest-sha-perl libsocket-getaddrinfo-perl libtext-template-perl`  
`sudo apt -y install nmap libnet-snmp-perl libcrypt-des-perl libnet-nbname-perl`  
`sudo apt -y install libfile-copy-recursive-perl libparallel-forkmanager-perl`

Télécharger les paquets sur le lien ci-dessus et les installer comme indiqué.

Afin de configurer l'agent, il faut éditer le fichier agent.cfg :  
`sudo vim /etc/fusioninventory/agent.cfg`.

Les lignes suivantes doivent être modifiées, en précisant l'IP publique de la machine accueillant GLPI :  
> server = http://ipPubliqueDeLaMachineServeur/glpi/plugins/fusioninventory/  
> httpd-trust = ipPubliqueDeLaMachineServeur

!!! note

	Ne pas oublier d'indiquer HTTPS pour les connexions sécurisées.

Pour autoriser les tâches d'inventaire SNMP il conviendra de modifier :  
> tasks = netdiscovery,netinventory,inventory

Le champs "tag" peut être complété afin de faciliter le tri des équipements lors de la découverte. Il est par exemple possible de mentionner le nom d'un site physique tel que :  
> tag = cauterets  

Enfin, redémarrer l'agent :  
`sudo systemctl restart fusioninventory-agent`.

Le statut de l'agent sera ensuite visible sur le poste sur lequel il est installé via l'adresse http://localhost:62354.

## Déploiement de masse Windows

Les agents peuvent être déployés sur les machines Windows à distance. Pour cela une GPO ordinateur et un script fourni par FusionInventory sont nécessaires. Le script, en vbs, permettant de déployer l'agent 2.4 est disponible [ici](/deploiementWindows/fusioninventory-agent-deployment.vbs).  
L'option /S rajoutée dans la variable SetupOptions va permettre le déploiement silencieux de l'agent. Il faudra en revanche rajouter l'option /M pour que l'agent soit visible dans le menu Démarrer de Windows.  
Les options /execmode=service et /add-firewall-exception donnent la possibilité respectivement d'installer l'agent en tant que service Windows et d'ouvrir le port 62354 pour autoriser l'accès au serveur.  
La variable SetupLocation contient le chemin vers le dossier partagé du serveur Windows accueillant la GPO, sur lequel sont stockés les fichiers .exe pour les versions [x64](/deploiementWindows/fusioninventory-agent_windows-x64_2.4.exe) et [x86](/deploiementWindows/fusioninventory-agent_windows-x86_2.4.exe).  
De plus, les fichiers de configuration de l'agent, [FusionInventory.adml](/deploiementWindows/FusionInventory.adml) et [FusionInventory.admx](/deploiementWindows/FusionInventory.admx), sont à placer dans le sous-dossier PolicyDefinitions de SYSVOL. 

En résumé :  
>	1. Placer le fichier .vbs dans le dossier *SYSVOL > netLogon/Scripts*.  
>	2. Placer les fichiers .admx et .adml dans le dossier *SYSVOL > PolicyDefinitions*.  
>	3. Placer les fichiers .exe dans un dossier de partage.  
>	4. Dans la GPO, accéder à *Configuration ordinateur > Stratégies > Paramètres Windows > Scripts (démarrage/arrêt)*. Dans *Démarrage*, ajouter le chemin vers le script .vbs.
>	5. Configurer les agents dans *Configuration ordinateur > Stratégies > Modèles d'administration > Système > Agent fusion*. Y renseigner le serveur et le tag.
>	6. Lier la GPO à une OU.
>	7. Redémarrer les postes clients.

Le déploiement de masse par GPO étant spécifique aux machines Windows, il reste  possible d'utiliser un script SSH pour déployer les agents sur les ordinateurs Linux mais également sous Mac.
