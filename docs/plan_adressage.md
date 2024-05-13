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
|  core2dmz / 242 |  172.28.98.0 /24 | 98.1 - 98.254 |172.28.98.255|
|   DMZ Privée / 243 | 192.168.145.254|  145.1-145.254| 192.168.145.255 |
|  DMZ / 244 | 192.168.45.254 /24 | 45.1 - 45.254 | 192.168.45.255 |
|  Utilisateurs / 245 | 172.28.101.0 /24 | 101.1 - 101.254 | 172.28.100.255 |
|  VLAN 246 | 172.28.102.0 /24 | 102.1 - 102.254 | 172.28.100.255 |
|  VLAN 247 | 172.28.103.0 /24 | 103.1 - 103.254 | 172.28.100.255 |
|  VLAN 248| 172.28.104.0 /24 | 104.1 - 104.254 | 172.28.100.255 |
|  VLAN 249 | 172.28.105.0 /24 | 105.1 - 105.254 | 172.28.100.255 |

</br>

## Site d'Orléans
| Nom | Adresse | 
|-----------|---------|
|VLAN 240 Management  |172.28.96.254 /24|
|VLAN 241 Serveur     |172.28.97.254 /24|
|VLAN 242 CORE2DMZ    |172.28.98.1 /24|
|VLAN 243 DMZ Privée     |Pas d'ip attribuée|
|VLAN 245 Utilisateur |172.28.101.254 /24|
|VLAN 249 transport   |Pas d'ip attribuée |
|VLAN 244 DMZ         |Pas d'ip attribuée |
|Firewall virtuel     |172.28.98.254 /24|
|Firewall IN(LAN)     |192.168.45.254 /24|
|Firewall OUT(WAN)    |172.28.105.1 /24|

</br>

## Ip fixes
| Nom | Adresse | 
|-----------|---------|
|R1-ORL                |(int g0/0.249) 172.28.105.253 /24 ; (int g0/0.240) 172.28.96.100 /24|
|R2-ORL                |(int g0/0.249) 172.28.105.252 /24 ; (int g0/0.240) 172.28.96.200 /24|
|OPNsense               |OUT(WAN) 192.168.45.1 /24  ;  IN(LAN) 172.28.98.254 /24 ; interface management 172.28.96.250 /24; DMZ PRIVEE 192.168.45.254/24|
|Contrôleur de Domaine |172.28.96.10 /24  ; 172.28.97.10 /24|
|Serveur web (Linux - Debian12)|192.168.45.253 /24 ; interface management 172.28.96.251 /24|
|SQUID|172.28.98.253 /24  ; interface management 172.28.96.253 /24;|
|Serveur DNS forwarder (bind9)|172.28.97.20 /24|
|Serveur Split DNS (bind9)|192.168.45.252 /24|
|Serveur Docker|192.168.45.251 /24|
|Serveur MariaDB|192.168.145.10 /24|
|Serveur Reverse Proxy|192.168.45.250 /24|
|PASSIF Reverse PROXY|192.168.45.248 /24|
|Serveur VPN|192.168.45.249 /24|
|INT WIREGUARD (Sur serveur VPN)|10.255.255.1/24|
|VIP REVERSE PROXY|192.168.45.200/24|
|PASSIF Serveur WEB|192.168.45.247/24|
|HMail Serveur|192.168.45.240/24|
|HTTP SLAM Serveur|192.168.45.246/24|
|Serveur Guacamole|172.28.97.30/24 ; 172.28.96.252|
|Point d'acces | 172.28.96.248|







