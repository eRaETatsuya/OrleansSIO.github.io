# Création du serveur VPN

# Étape 1: Création du Serveur VPN sur Nutanix

## 1.1 Prérequis

Avant de commencer, il faut créer la machine virtuelle sur Nutanix, avec un OS Debian.

## 1.2 Installation de WireGuard

Pour installer Wireguard, on suit le tuto de it-connect
Sur la machine, recopier ces lignes.

    sudo apt-get update
    sudo apt-get install wireguard

création interface wireguard :

    en premier il faut générer une clé privé et une clé publique:
    wg genkey | sudo tee /etc/wireguard/wg-private.key | wg pubkey | sudo tee /etc/wireguard/wg-public.key

création du fichier de conf de l'interface :

    sudo nano /etc/wireguard/wg0.conf

contenu du fichier :

    [Interface]
    Address = 10.255.255.1/24
    SaveConfig = true
    ListenPort = 51820
    PrivateKey = <clé privée du serveur>

Pour obtenir la clé privé il faut faire la commande suivante :

    sudo cat /etc/wireguard/wg-private.key

Démarrage de l'interface : 

    sudo wg-quick up wg0

attention a chaque fois qu'on manipule le fichier de conf de l'interface il faut que cette dernière soit éteinte avec la commande :

    sudo wg-quick down wg0

## 1.3 Activation de l'IP Forwarding

Dans le fichier de conf suivant : 

    sudo nano /etc/sysctl.conf

Ajoutez cette ligne : 

    net.ipv4.ip_forward=1


## 1.4 Activation de l'IP Forwarding

Activer l'IP Masquerade. Cela revient à faire de notre serveur vpn un Routeur qui va NAT les requêtes du réseau vpn (10.255.255.0/24) vers les différents réseaux locaux.

Installer ufw :
    
    sudo apt-get install ufw

Autoriser des ports :
    
    ssh -> sudo ufw allow 22/tcp
    wireguard -> sudo ufw allow 51820/udp

Dans le fichier /etc/ufw/before.rules :

    sudo nano /etc/ufw/before.rules

    ajoutez ces lignes :

    # NAT - IP masquerade
    *nat
    :POSTROUTING ACCEPT [0:0]
    -A POSTROUTING -o ens3 -j MASQUERADE

    # End each table with the 'COMMIT' line or these rules won't be processed
    COMMIT


    # autoriser le forwarding pour le réseau distant de confiance (+ le réseau du VPN)
    -A ufw-before-forward -s 192.168.45.0/24 -j ACCEPT
    -A ufw-before-forward -d 192.168.45.0/24 -j ACCEPT
    -A ufw-before-forward -s 10.255.255.0/24 -j ACCEPT
    -A ufw-before-forward -d 10.255.255.0/24 -j ACCEPT

Après avoir modifier ce fichier :

    sudo ufw enable

    sudo systemctl restart ufw



# Étape 2: Installation Wireguard côté client :

## 2.1 Création d'une vm client sur virtualbox

## 2.2 Installation du logiciel wireguard sur cette vm

## 2.3 Création du tunnel

    - cliquer sur le bouton ajouter un tunnel vide

    - dans la fenêtre de configuration qui s'ouvre rentrer dans le bloc interface l'information : Address = 10.255.255.2/24

    - ajouter un bloc [Peer]

    - dans ce bloc ajouter la clé public du serveur vpn qu'on peut obtenir avec la commande "sudo cat /etc/wireguard/wg-public.key". PublicKey = <clé publique du serveur>

    - Ajouter l'information AllowedIPs = 10.255.255.0/24, 192.168.45.0/24, 172.28.0.0/16

    - Ajouter l'ip publique suivi du port : EndPoint = ip_publique:51820
    (au préalable il faut avoir rediriger le port 51820 vers le serveur vpn sur le routeur)

## 2.4 Déclaration du client du coté du serveur

Il faut en premier éteindre l'interface wireguard

    sudo wg-quick down wg0

Ensuite dans le fichier de conf de cette interface on va déclarer le client

    sudo nano /etc/wireguard/wg0.conf
    
    ajouter le bloc [Peer] avec la clé publique du client wireguard PublicKey = <clé publique client>
    ensuite l'adresse ip du client du réseau vpn AllowedIPs = 10.255.255.2/32

On peut maintenant rallumer l'interface après avoir enregistré la configuration

    sudo wg-quick up wg0


# Étape 3: Connexion au VPN depuis le client :

## 3.1 Se connecter au vpn du prof sur son pc : 

Se connecter avec OpenVPN au VPN du professeur

## 3.2 Se connecter au vpn Wireguard :

Il faut créer une vm windows 10 qui est connecté en réseau NAT dans la configuration de la VM.

Ensuite il faut importer la conf vpn dans le client wireguard et cliquer sur "Activer".