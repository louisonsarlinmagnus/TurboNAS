# Docker
<center><img src="/img/logo_docker.png" width="350"/></td></center>

!!! summary "Définition"
    Docker est un outil qui peut empaqueter une application et ses dépendances dans un conteneur isolé, qui pourra être exécuté sur n'importe quel serveur.  
    *451 Research*


## Installation
Source: [doc Docker](https://docs.docker.com/engine/install/ubuntu/)

!!! example "Installation de Docker"

    Installation des dépendances nécessaires:
    ```sh
    sudo apt install \
        apt-transport-https \
        ca-certificates \
        curl \
        gnupg2 \
        software-properties-common
    ```

    Récupération de la clé du dépot:
    ```sh
    curl -fsSL https://download.docker.com/linux/debian/gpg | sudo apt-key add -
    ```

    Selection de la version stable:
    ```sh
    sudo add-apt-repository \
    "deb [arch=amd64] https://download.docker.com/linux/debian \
    $(lsb_release -cs) \
    stable" 
    ```
    !!! tip "Tip: mise a jour de l'index `apt`"
        ```sh
        sudo apt update
        ```

    Installation du moteur Docker
    ```sh
    sudo apt update && sudo apt-get install docker-ce docker-ce-cli containerd.io
    ```

