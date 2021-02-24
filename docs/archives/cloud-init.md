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
        <td><img src="https://louisonsarlinmagnus.github.io/TurboNAS/img/code_reso.png" width="350"/></td>
        <td><img src="https://louisonsarlinmagnus.github.io/TurboNAS/img/reseau.png" width="350"/></td>
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


NOTE: On peut remplacer ifconfig par: `#!bash alias ifconfig="ip -c a | sed -e 's/\// \//g'"`
