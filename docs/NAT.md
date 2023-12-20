## Redirection de port de nos routeurs :


| Nom d'hôte| Adresse IP locale | Protocole | Port de début | Port de Fin | Interface Physique |
|-----------|-----------|---------|-----------|-----------|-----------|
| SRV-DNS |192.168.45.252|TCP| 53 | 53 | g0/1|
| SRV-DNS |192.168.45.252|UDP| 53 | 53 | g0/1|
| SRV-VPN |192.168.45.249|UDP| 51820 | 518 | g0/1|
| SRV-VPN |192.168.45.249|TCP| 51820 | 518 | g0/1|
| VIP Reverse-Proxy |192.168.45.200|TCP| 81 | 81 | g0/1|
| VIP Reverse-Proxy |192.168.45.200|TCP| 80 | 80 | g0/1|
| VIP Reverse-Proxy |192.168.45.200|TCP| 443 | 443 | g0/1|
| SRV-Hmail |192.168.45.240|TCP| 25 | 25 | g0/1|
| SRV-Hmail |192.168.45.240|TCP| 143 | 143 | g0/1|
