# Quick Setup

Sur Windows 10 Pro équipé du WSL Ubuntu:
- Installer Multipass: `sudo snap install multipass`

# Liens utiles

[debug Reseau](https://www.reddit.com/r/HomeServer/comments/e2v2me/nextcloud_and_plex_on_1_public_ip/)



# Machine utilisée

| Pièce           | Ref             |
|-----------------|-----------------|
| CPU             | AMD FX 8370     |
| GPU             | GTX 1060 3 Go   |
| Mémoire         | 16 Go DDR3      |
| Stockage        | 3x 3To Firecuda |
| Stockage OS     | 256 Go SSD      |
| Carte mère      | MSI 990 FXA     |
| Alimentation    | RM750X          |


# Principe de fonctionnement

Installer Ubuntu sur la machine.
Installer Plex et Nextcloud sur Ubuntu.

Plex et Nextcloud sont dispo comme appliances sur Ubuntu:  
- [Appliance NEXTCLOUD](https://ubuntu.com/appliance/nextcloud)  
- [Appliance PLEX](https://ubuntu.com/appliance/plex)



# Installation d'Ubuntu

Version choisie: 20.04 LTS Server

## Etape 1: Création de la clé Bootable

- Télécherger l'[ISO d'Ubuntu 20.04](https://lecrabeinfo.net/telecharger-iso-ubuntu-20-04-lts-et-ses-variantes.html#ubuntu-20-04-lts)
- Veriffier l'intégrité du fichier avec le SHA-256
- Créer une clé USB Bootable (Rufus)

## Etape 2: Installation d'Ubuntu

- Démarrer le PC avec la clé branchée
- Choisir "Ubuntu" puis "Install Ubuntu"
- Choisir "Effacer le disque et installer Ubuntu"
- Une fois l'installation finie cliquer sur "Redemarrer" et **enlever la clé USB**
- Choisir le configurer Livepatch
- Installer les dernières mises à jour

# Communication SSH avec le serveur

Pré-requis: [Créer une clée SSH](https://www.illuminiastudios.com/dev-diaries/ssh-on-windows-subsystem-for-linux/) qui permettra de communiquer de manière sécurisée
Etant donnée que le serveur sera installé dans une autre pièce et que je souhaite pouvoir le configurer confortablement. Je vais mettre en place une communication ssh.

> SSH (Secure Shell): Il permet la connexion à une machine distante (serveur) via une liaison sécurisée dans le but de transférer des fichiers ou des commandes.

Une fois les 2 partie Setup réalisée il suffit de se connecter avec la ligne: `ssh <username>@<adresse_ip> -p <port>` (ex: `ssh louison@192.168.1.11 -p 22`)

## Setup SSH côté Serveur

Il "suffit":
- d'autoriser la connexion par SSH: `sudo ufw allow ssh`
- de démarrer le service SSH: `sudo systemctl start ssh` ou `sudo service ssh start`

## Setup SSH côté Client

Du côté client j'utilise déjà un Windows Subsystème Linux (Ubuntu 20.04 LTS).

Il "suffit":
- d'autoriser la connexion par SSH: `sudo ufw allow ssh`
- de démarrer le service SSH: `sudo systemctl start ssh` ou `sudo service ssh start`

# Utilisation de multipass

Installation de multipass: `sudo snap install multipass`

|Action|Commande|Exemple|
|:-|:-|:-|
|Liste les multipass disponibles|`multipass find`||
|Lancer/installer une instance: |`multipass launch <image_name> --name <nom_custom>`|`multipass launch nextcloud --name moncloud`|
|Lancer/installer une instance customisé avec **cloud-init**|`multipass launch -n <image_name> --cloud-init <fichier_init>.yaml`|`multipass launch -n nextcloud --cloud-init init-moncloud.yaml`|
|Lister les mes instances<br>(pour récuperer l'IP): |`multipass list`<br>`multipass ls`||
|Ouvrir un bash: |`multipass shell <nom_instance>`|`multipass shell moncloud`|
|Démarrer une instance: |`multipass start <nom_instance>`|`multipass start moncloud`|
|Arrêter une instance: |`multipass stop <nom_instance>`|`multipass stop moncloud`|
|Monter un volume|`multipass mount <nom_volume> <nom_instance>`|`multipass mount $HOME moncloud:/chemain/voulu`|
|Transmettre une commande a l'instance|`multipass exec <nom_instance> -- <la_commande>`|`multipass exec moncloud -- sudo apt update`|
|Copier des fichiers|`multipass copy-files <a_copier> <cible>`|`multipass copy-files moncloud:/chemin/voulu.txt /chemin/local/`<br>`multipass copy-files /chemin/local/ moncloud:/chemin/voulu.txt`|
|Supprimer une instance|`multipass delete <nom_instance`|`multipass delete moncloud`|
|Récupérer une instance supprimée|`multipass recover <nom_instance>`|`multipass recover moncloud`|
|Purger les instances supprimées|`multipass purge`||

<br>

# Utilisation des cloud-init

**Cloud-init** st le standard pour personnaliser des instances cloud. Il est possible de customiser une instance **multipass** en utilisant **cloud-init**.  
Les fichier **cloud-init** sont des **YAML**.

Construction du `fichier.yaml`
|Commande|Signification|Exemple|
|:-|:-|:-|
|`package_update`|Mise à jour de la base apt au démarrage|`True/False`|
||||
|`package`|Liste des paquets que je souhaite installer|<code>packages:<br>&emsp;- apt-transport-https<br>&emsp;- ca-certificates<br>&emsp;- curl<br>&emsp;- gnupg-agent<br>&emsp;- software-properties-common</code>|
|`runcmd`|Liste de commandes à exécuter|<code>runcmd:<br>&emsp;- apt-get update -y</code>|
|`final_message`|Message affiché à la fin du premier démarrage|`"L'installation à été réalisée avec succès`|
<br>
<table>
    <tr>
        <td><img src="https://i.imgur.com/Vn0FSkf.png" width="350"/></td>
        <td><img src="https://i.imgur.com/sfISPUR.png" width="350"/></td>
    </tr>
</table>

Grace à ce script on peut:
- Depuis l'instance:
    + Pinger google (`ping google.com`)  
    Donc on a accès à internet
- Depuis le server:
    + Pinger la vm (`ping 10.8.248.188`)
- Depuis le réseau:
    + **PAS** pinger la vm (`ping 10.8.248.188`)
    + Pinger le server (`ping 192.168.1.11`)

Je pense qu'il faudrait exposer la vm sur un port du server. Pour accéder a la vm depuis le réseau il faudrait faire `bind <ip_instance> <no_port>` puis donc `ping <ip_server>:<port_choisi>`

J'ai un problème avec ce code:
```yaml
# Create default system user
# Edit the name and ssh-authorized-keys according to your needs
system_info:
  default_user:
    name: louison
    home: /home/louison
    shell: /bin/bash
    lock_passwd: True
    sudo: ALL=(ALL) NOPASSWD:ALL
```


NOTE: On peut remplacer ifconfig par: `alias ifconfig="ip -c a | sed -e 's/\// \//g'"`

# PLEX

[Fichier cloud-init]()

[Ici](https://ubuntu.com/appliance/plex/vm)



Pour [configurer facilement PLEX](http://<IP-of-your-appliance>:32400/web).


# NEXTCLOUD

[Ici](https://ubuntu.com/appliance/nextcloud/vm)  

Pour configurer facilement PLEX: `http://<IP-of-your-appliance>`



# Accès au serveur a distance (SSH?)


# Quelques explications

**Ubuntu Appliance** est un "store" propose des ISO customs vous permettant de déployer dans VM comme Multipass, une application.

[**Multipass**](https://multipass.run/) est un gestionnaire de machines virtuelles permettant l'utilisation de script d'initialisation.
