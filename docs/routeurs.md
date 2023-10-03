![routeurs](routeur.png)
# Guide d'installation des routeurs cisco R1_ORL et R2_ORL


## Réinitialisation des routeurs

Les boutons des routeurs ne fonctionnaient plus, nous avons donc réinitialisés les routeurs à la ligne de commande.

1. Dès que les informations apparaissent à votre écran, appuyer sur les touches **`CTRL+PAUSE`** pour accéder au **`ROM MONITOR MODE`**
2. si **`prompt>`** apparaît à l'écran alors exécuter les commandes suivantes: **`confreg 0x2142`** et ensuite **`reset`**
3. Le routeur va redémarrer et demandera à faire le setup, écrivez **`no`**
4. Taper **`enable`** et exécuter la commande suivante: **`Copy startup-config running-config`**
5. Créer un mot de passe pour le mode privilege **`Enable secret "mdp"`** puis pour l'accès à la console **`Password "mdp"`**
6. Pour finir taper **`confreg 2102`** et **`copy startup-config running-config`**

## Installer SSH [**ici**](/OrleansSIO.github.io/switch/)

## Adressage IP d'une interface d'un routeur cisco 

<br>

**Routeur R1_ORL**

1. Taper la commande **`enable`** puis **`conf t`**
2. Exécuter les commandes suivantes: **`int g0/1`** ; **`ip address 183.44.145.1 255.255.255.252`** ;
3. Activer le nat en extérieur car ce réseau n'est pas en local **`ip nat outside`** Activer l'interface **`no sh`** puis quitter **`exit`**
4. Ensuite, **`int g0/0.240`** et **`ip address 172.28.96.100 255.255.255.0`** activer l'interface et quitter
5. Pareil avec une autre interface virtuelle: **`int g0/0.249`** ; **`ip address 172.28.105.253 255.255.255.0`** puis activer le nat en intérieur **`ip nat inside`** 
activer également l'interface et quitter
6. Activer la passerelle par défaut : **`ip route 0.0.0.0 0.0.0.0 183.44.145.2`**
7. créer une acl : **`access-list 1 permit 172.28.96.0 0.0.31.255`**
8. Rentrer dans l'interface g0/0 **`int g0/0`** et appliquer l'acl **`ip access-group 1 in`**
9. Sur les interfaces virtuelles g0/0.249 et g0/0.240, il faut configurer le tag dot1Q 249 et 240. Ecriver les commandes suivantes:<br>
            **`-int g0/0.249`**
            <br>
            **`-encapsulation dot1Q 249`**
            <br>
            **`-exit`**
            <br>
             **`-int g0/0.240`**
            <br>
            **`-encapsulation dot1Q 240`**
            <br>
            **`-exit`**
<br>

**Routeur R2_ORL**

1. Taper la commande **`enable`** puis **`conf t`**
2. Exécuter les commandes suivantes: **`int g0/1`** ; **`ip address 221.87.145.2 255.255.255.252`** ;
3. Activer le nat en extérieur car ce réseau n'est pas en local **`ip nat outside`** Activer l'interface **`no sh`** puis quitter **`exit`**
4. Ensuite, **`int g0/0.240`** et **`ip address 172.28.96.100 255.255.255.0`** activer l'interface et quitter
5. Pareil avec une autre interface virtuelle: **`int g0/0.249`** ; **`ip address 172.28.105.253 255.255.255.0`** puis activer le nat en intérieur **`ip nat inside`** 
activer également l'interface et quitter
6. Activer la passerelle par défaut : **`ip route 0.0.0.0 0.0.0.0 183.44.145.2`**
7. créer une acl : **`access-list 1 permit 172.28.96.0 0.0.31.255`**
8. Rentrer dans l'interface g0/0 **`int g0/0`** et appliquer l'acl **`ip access-group 1 in`**
9. Sur les interfaces virtuelles g0/0.249 et g0/0.240, il faut configurer le tag dot1Q 249 et 240. Ecriver les commandes suivantes:<br>
            **`-int g0/0.249`**
            <br>
            **`-encapsulation dot1Q 249`**
            <br>
            **`-exit`**
            <br>
             **`-int g0/0.240`**
            <br>
            **`-encapsulation dot1Q 240`**
            <br>
            **`-exit`**
