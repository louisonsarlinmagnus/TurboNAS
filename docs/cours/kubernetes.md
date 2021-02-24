# Kubernetes
<center><img src="https://louisonsarlinmagnus.github.io/TurboNAS/img/logo_kubernetes.png" width="350"/></td></center>

!!! summary "Définitions"
    Kubernetes:
    >C'est une plate-forme permettant d'automatiser le déploiement, la montée en charge et la mise en œuvre de conteneurs d'application sur des clusters de serveurs.  
    *Wikipedia*

    Node:
    >Un Node aussi appelé Worker ou Minion est une machine unique (ou une machine virtuelle) où des conteneurs sont déployés.  
    *Wikipedia*

    Pod:
    >Un pod consiste en un ou plusieurs conteneurs qui ont la garantie d'être co-localisés sur une machine hôte et peuvent en partager les ressources.  
    Chaque pod dans Kubernetes possède une adresse IP unique à l'intérieur du cluster.  
    *Wikipedia*

    Labels et selectors:
    >On peut attacher des paires clés-valeurs appelées "labels" à n'importe quel objet d'API dans le système, par exemple les pods et les nodes.  
    *Wikipedia*

    Services:
    >Un service Kubernetes est un groupe de pods travaillant ensemble, l'ensemble des pods qui constituent un service sont définis par un label selector.  
    *Wikipedia*

## Installation

!!! tip "Avant tout mettre a jour le système"
    ```bash
    sudo apt-get update
    sudo apt-get upgrade
    ```

!!! example "Installation de minikube et kubeclt"
    === "minikube"
        Minikube est un outil facilitant l’exécution locale de Kubernetes  

        Téléchargement du binaire
        ```sh
        wget https://storage.googleapis.com/minikube/releases/latest/minikube-linux-amd64
        ```

        Attribution des droits d'exécution
        ```sh
        chmod +x minikube-linux-amd64
        ```

        Mise dans le PATH
        ```sh
        sudo mv minikube-linux-amd64 /usr/local/bin/minikube
        ```

        Vérification de l'installation
        ```bash
        minikube version
        ```
    === "kubectl"
        L'outil en ligne de commande de kubernetes  

        Téléchargement du binaire
        ```sh
        curl -LO https://storage.googleapis.com/kubernetes-release/release/`curl -s https://storage.googleapis.com/kubernetes-release/release/stable.txt`/bin/linux/amd64/kubectl
        ```

        Attribution des droits d'exécussion
        ```sh
        chmod +x ./kubectl
        ```

        Mise dans le PATH
        ```sh
        sudo mv ./kubectl /usr/local/bin/kubectl
        ```

        Vérification de l'installation
        ```bash
        kubectl version -o json
        ```
    !!! warning "Erreur"
        A l'exécussion de la dernière commande on reçoit une erreur:
        ```diff
        -The connection to the server localhost:8080 was refused - did you specify the right host or port?
        ```
        On pense que puisque nous n'avons lancer aucun cluster il ne peut accéder à `#!sh localhost:8080` car il n'y a tout simplement rien
