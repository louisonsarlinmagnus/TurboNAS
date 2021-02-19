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