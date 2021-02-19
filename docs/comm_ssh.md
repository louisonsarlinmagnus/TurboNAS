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