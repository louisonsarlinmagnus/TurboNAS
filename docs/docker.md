# Docker

!!! summary "Définition"
    > "Docker est un outil qui peut empaqueter une application et ses dépendances dans un conteneur isolé, qui pourra être exécuté sur n'importe quel serveur"
    > 451 Research

Pour déployer une **container docker** nous allons avoir besoin de:

- Installer Docker
- Obtenir l'**image docker** de l'application
- Expliciter les paramètres du déploiement (réseaux, mots de passes, volumes)

# Installations
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

!!! example "Installation de **docker-compose**"

    Installation
    ```sh
    sudo curl -L "https://github.com/docker/compose/releases/download/1.25.3/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose
    ```
    On donne les droits d'exécution
    ```sh
    sudo chmod +x /usr/local/bin/docker-compose
    ```

    !!! tip "Verification"
        ```sh
        sudo docker-compose --version
        ```

!!! example "Installation de Traefik"
    ```sh
    
    ```

# Utilisations
      
??? example "docker-compose.yml"

    Le fichier s'appelle par défaut `docker-compose.yml`  
    ```yaml
    version: '3.7'
    services:
      database:
        image: mariadb:10
        command: --transaction-isolation=READ-COMMITTED --binlog-format=ROW
        restart: unless-stopped
        environment:
          MYSQL_RANDOM_ROOT_PASSWORD: 'yes'
          MYSQL_USER_FILE: '/run/secrets/mysql-user'
          MYSQL_DATABASE_FILE: '/run/secrets/mysql-database'
          MYSQL_PASSWORD_FILE: '/run/secrets/mysql-password'
        secrets:
          - mysql-user
          - mysql-database
          - mysql-password
        networks:
          - lan 
        volumes: 
          - db:/var/lib/mysql

      nextcloud:
        depends_on:
          - database
        image: nextcloud:18
        restart: unless-stopped
        environment:
          MYSQL_HOST: 'database'
          MYSQL_DATABASE_FILE: '/run/secrets/mysql-database'
          MYSQL_USER_FILE: '/run/secrets/mysql-user'
          MYSQL_PASSWORD_FILE: '/run/secrets/mysql-password'
          NEXTCLOUD_ADMIN_PASSWORD: ${NEXTCLOUD_ADMIN_PASSWORD}
          NEXTCLOUD_ADMIN_USER: ${NEXTCLOUD_ADMIN_USER}
          NEXTCLOUD_TRUSTED_DOMAINS: ${NEXTCLOUD_URL}
        secrets:
          - mysql-user
          - mysql-database
          - mysql-password
        networks:
          - lan
          - traefik_web
        volumes:
          - html:/var/www/html
        labels:
          traefik.enable: true
          traefik.docker.network: traefik_web
          traefik.http.routers.nextcloud.entrypoints: websecure
          traefik.http.routers.nextcloud.rule: 'Host(`${NEXTCLOUD_URL}`)'
          traefik.http.services.nextcloud.loadbalancer.server.port: 80
          
    volumes:
      db:
      html:

    networks:
      traefik_web:
        external: true

      lan:

    secrets:
      mysql-user:
        file: ./secrets/mysql-user.txt

      mysql-database:
        file: ./secrets/mysql-database.txt

      mysql-password:
        file: ./secrets/mysql-password.txt
    ```