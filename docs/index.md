# Plan d'adressage du réseau 

>L'ensemble du réseau se repose sur l'adresse réseau **`172.28.96.0/19`**.

## Réseau d'Orléans

| @Reseau | @Diffusion | VLAN| Masque décimal  / CIDR |
|---------|------------|-----|------------------------|
|  172.28.96.0  |  172.28.127.255   | 240-249| 255.255.224.0 /19 |

</br>

## Plan adressage sous réseau (VLAN)

| VLAN | @Réseau | Plage d'adresse | @Diffusion|
|-----------|---------|-----------------|-----------|
|Management / 240|172.28.96.0 /24| 172.28.96.1 - 172.28.96.254 | 172.28.96.255|
|  Serveur / 241 | 172.28.97.0 /24| 97.1 - 172.28.97.254 |172.28.97.255|
|  VLAN 242 |   172.28.98.0 /24 | 98.1 - 98.254 |172.28.98.255|
|  VLAN 243 |  172.28.99.0 /24 | 99.1 - 99.254 | 172.28.99.255 |
|  VLAN 244 | 172.28.100.0 /24 | 100.1 - 100.254 | 172.28.100.255 |

</br>

## Site d'Orléans
| Nom | Adresse | 
|-----------|---------|
|Controleur de Domaine|172.28.96.10 ; 172.28.97.10|
|DMZ|192.168.45.1|

