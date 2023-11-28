# Guide de Configuration du Serveur DNS sur Nutanix

## Étape 1: Création du Serveur DNS sur Nutanix

### 1.1 Prérequis

Avant de commencer, assurez-vous d'avoir une instance Nutanix provisionnée et prête à être configurée. Connectez-vous à la console d'administration pour accéder à la configuration de la machine virtuelle.

### 1.2 Installation de BIND9

Sur la machine virtuelle Nutanix, installez BIND9 en utilisant les commandes suivantes :

```bash
sudo apt-get update
sudo apt-get install bind9
```

### 1.3 Configuration de BIND9

Après l'installation, vous pouvez configurer le serveur DNS en modifiant le fichier `/etc/bind/named.conf.options` en utilisant la commande suivante :

```bash
sudo nano /etc/bind/named.conf.options
```

Dans le fichier de configuration, ajoutez les lignes suivantes :

```bash
forwarders {t-get install bind9
options {
    directory "/var/cache/bind";

    // If there is a firewall between you and nameservers you want
    // to talk to, you may need to fix the firewall to allow multiple
    // ports to talk.  See http://www.kb.cert.org/vuls/id/800113

    // If your ISP provided one or more IP addresses for stable
    // nameservers, you probably want to use them as forwarders.
    // Uncomment the following block, and insert the addresses replacing
    // the all-0's placeholder.

    // forwarders {
    //     121.183.90.205;
    // };

    dnssec-validation no;
   
    allow-query {
        127.0.0.0/8;
        172.28.0.0/16;
        192.168.45.0/24;
        192.168.145.0/24;
        121.183.90.0/24; 
    };

    listen-on-v6 { none; };
    listen-on { any; };
};
```

### 1.4 Redémarrage de BIND9

Après avoir configuré BIND9, vous pouvez redémarrer le service en utilisant la commande suivante :

```bash
sudo systemctl restart bind9
```
## Étape 2: Création des fichiers de zone

### 2.1: Création du fichier de zone interne
Nous avons besoin d'un fichier de zone interne, pour le créer:
```bash
sudo cat db.internal
```
Dans ce fichier doit se trouver ça :

```bash

$TTL 86400
@   IN  SOA     ns1.orleans.sportludique.fr. admin.orleans.sportludique.fr. (
                 2019101601 ; Serial
                 10800      ; Refresh
                 3600       ; Retry
                 604800     ; Expire
                 86400 )    ; Minimum TTL

@           IN  NS      ns1.orleans.sportludique.fr.
ns1         IN  A       192.168.45.252
www         IN  A       192.168.45.253



local.orleans.sportludique.fr.   IN      NS      orl-dc-01.local.orleans.sportludique.fr.
orl-dc-01.local.orleans.sportludique.fr.         IN      A       172.28.97.10

blog IN CNAME www.orleans.sportludique.fr.

rp1 IN A 192.168.45.250
node IN CNAME rp1.orleans.sportludique.fr.

double-reverse-proxy IN A 192.168.45.248
dbrp   IN CNAME double-reverse-proxy.orleans.sportludique.fr.

chat IN CNAME rp1.orleans.sportludique.fr.

```
un fichier de zone interne facilite la résolution des noms de domaine à l'intérieur d'un réseau privé, en associant des noms conviviaux à des adresses IP et en organisant la structure des ressources au sein du réseau local. Cela contribue à une gestion efficace des ressources et à une navigation plus conviviale pour les utilisateurs du réseau.

### 2.2 Création du fichier de zone externe

Même principe que le fichier de zone interne, sauf que le fichier doit s'appeler **`db.external`**
```bash
User

$TTL 86400
@   IN  SOA     ns1.orleans.sportludique.fr. admin.orleans.sportludique.fr. (
                 2019101601 ; Serial
                 10800      ; Refresh
                 3600       ; Retry
                 604800     ; Expire
                 86400 )    ; Minimum TTL


@           IN  NS      ns1.orleans.sportludique.fr.
ns1         IN  A       183.44.45.1
ns1         IN  A       221.87.145.2
www         IN  CNAME   ns1.orleans.sportludique.fr.

blog IN CNAME www.orleans.sportludique.fr.

node IN CNAME ns1.orleans.sportludique.fr.
chat IN CNAME ns1.orleans.sportludique.fr.
```
Le fichier de zone externe permet d'associer des noms de domaine conviviaux à des adresses IP accessibles depuis l'extérieur du réseau local. Cela permet aux autres utilisateurs d'accéder à des services tels que WordPress, Node.js, Chat et Apache en utilisant des noms de domaine plutôt que des adresses IP. Ainsi, les utilisateurs peuvent accéder à ces services en utilisant des URL plus conviviales.

## Étape 3: Ajout des zones DNS

### 3.1 Création des zones DNS

Après avoir configuré BIND9 ainsi que les fichiers de zone, vous pouvez ajouter les zones DNS en modifiant le fichier `/etc/bind/named.conf.local` en utilisant la commande suivante :

```bash
sudo nano /etc/bind/named.conf.local
```

Dans le fichier de configuration, ajoutez les lignes suivantes :

```bash
view "externe" {
        match-clients{
                !172.28.0.0/16;
                !192.168.45.0/24;
                !127.0.0.0/8;
                any;
        };
        zone "orleans.sportludique.fr." {
        type master;
                file "/etc/bind/db.external";
                allow-update { none; };
        };

};


view "interne" {
        match-clients{
                127.0.0.0/8;
                172.28.0.0/16;
                192.168.45.0/24;
        };
        zone "orleans.sportludique.fr." {
        type master;
                file "/etc/bind/db.internal";
                allow-update { none; };
        };

};

```


Le code ci-dessus représente une configuration de zones DNS pour le serveur BIND9, définissant deux vues distinctes : "externe" et "interne".

### Vue Externe

La vue "externe" est destinée aux clients externes au réseau local. Elle spécifie les adresses IP des clients autorisés à accéder à cette vue à l'aide de la directive `match-clients`. Dans cet exemple, les adresses IP des réseaux `172.28.0.0/16`, `192.168.45.0/24`, et `127.0.0.0/8` sont exclues, ce qui signifie que tous les autres clients sont autorisés.

La zone "orleans.sportludique.fr." est définie comme une zone maître (master) dans cette vue. Elle utilise le fichier de zone `/etc/bind/db.external` pour stocker les enregistrements DNS de cette zone. La directive `allow-update` est définie sur "none", ce qui signifie qu'aucune mise à jour dynamique n'est autorisée pour cette zone.

### Vue Interne

La vue "interne" est destinée aux clients internes au réseau local. Elle spécifie les adresses IP des clients autorisés à accéder à cette vue. Dans cet exemple, les adresses IP des réseaux `127.0.0.0/8`, `172.28.0.0/16`, et `192.168.45.0/24` sont autorisées.

La zone "orleans.sportludique.fr." est également définie comme une zone maître dans cette vue. Elle utilise le fichier de zone `/etc/bind/db.internal` pour stocker les enregistrements DNS de cette zone. La directive `allow-update` est définie sur "none", ce qui signifie qu'aucune mise à jour dynamique n'est autorisée pour cette zone.

Ces configurations permettent de gérer les zones DNS de manière différente en fonction de la source des requêtes DNS. Les clients externes auront accès à la vue "externe" avec les enregistrements DNS correspondants, tandis que les clients internes auront accès à la vue "interne" avec les enregistrements DNS correspondants.

**Note :** Il est important de redémarrer le service bind9 à chaque modification des fichiers de configuration.

```bash 
sudo systemctl restart bind9
```
