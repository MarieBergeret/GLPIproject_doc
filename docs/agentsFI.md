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

Enfin, redémarrer l'agent :  
`sudo systemctl restart fusioninventory-agent`.

## Déploiement de masse Windows

Les agents peuvent être déployés sur les machines Windows à distance. Pour cela une GPO ordinateur et un script fourni par FusionInventory seront nécessaires. Le script, en vbs, est disponible à [cette adresse](https://raw.githubusercontent.com/fusioninventory/fusioninventory-agent/2.4.x/contrib/windows/fusioninventory-agent-deployment.vbs).  
L'option /S va permettre le déploiement silencieux de l'agent. Il faudra en revanche rajouter l'option /M pour que l'agent soit visible dans le menu Démarrer de Windows. Il sera tout de même possible de voir le statut de l'agent sur chacun des postes en se rendant sur http://127.0.0.1:62354.

Le déploiement de masse par GPO étant spécifique aux machines Windows, il reste  possible d'utiliser un script SSH pour déployer les agents sur les ordinateurs Linux mais également sous Mac.
