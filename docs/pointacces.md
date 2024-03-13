# Configuration du point d'acces WIFI Orleans et Orleans INVITE
### Creation des SSIDs

Tout d'abord, il nous faut rentrer sur l'interface WEB du point d'acces pour cela on entre son adresse ip sur le navigateur :
![image](https://github.com/eRaETatsuya/OrleansSIO.github.io/assets/144692551/5bd23b1c-6061-49df-be8a-03237d614623)

Ensuite on se login avec les identifiants par defaut (admin admin) on arrivera sur l'interface de gestion :



## Configuration de l'interface VLAN 245 :

- **Objectif :** Attribution d'adresses IP pour le VLAN 245.

- **Étapes réalisées :**
  1. Attribution d'une plage d'adresses IP sur le serveur DHCP pour le VLAN 245.
  2. Configuration du relais DHCP sur le commutateur pour rediriger les requêtes DHCP vers le serveur DHCP approprié.
  3. Vérification de la connectivité réseau des clients du VLAN 245.

## Configuration de l'interface VLAN 246 :

- **Objectif :** Attribution d'adresses IP pour le VLAN 246 et autorisation d'accès à Internet.

- **Étapes réalisées :**
  1. Attribution d'une plage d'adresses IP sur le serveur DHCP pour le VLAN 246.
  2. Configuration du relais DHCP sur le commutateur pour rediriger les requêtes DHCP vers le serveur DHCP approprié.
  3. Configuration d'une ACL pour le VLAN 246 permettant l'accès à la passerelle par défaut et à Internet.
  4. Vérification de la connectivité réseau des clients du VLAN 246.
