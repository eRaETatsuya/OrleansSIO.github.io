![Catalyst 3560](../img/catalyst.png)
# Guide d'Installation de SSH et Création de VLAN sur un Switch Cisco


## Installation de SSH sur le Switch

1. **Connexion au Switch :** On se connecte au switch Cisco via le câble console en utilisant un client de terminal.
2. On a généré une paire de clés SSH RSA pour l'authentification avec la commande **`switch(config)# crypto key generate rsa`** puis ensuite vous entrez **`2048`**.
3. **Configuration SSH :**
    On a activé le service SSH (version 2) en utilisant les commandes suivantes :
   </br>
    **`switch(config)# ip ssh version 2`** 
   </br>
   **`switch(config)# line vty 04`**
   </br>
   **`switch(config-line)# transport input ssh`**
   </br>
   **`switch(config-line)# transport output ssh`**
   </br>
   **`switch(config-line)# login local`**
   </br>
   **`switch(config-line)# exit`**
   </br>
   Il ne faut pas oublier d'entrer :
   </br>
   **`copy running-config startup-config`**
   </br>



## Création de VLAN sur le Switch

3. **Configuration des VLANs :**
</br>
   - On accéde au mode de configuration VLAN en conf-t.
   - Pour créer un VLAN il faut spécifier son numéro, pour créer notre vlan Management on utilise le vlan 240 et on lui donne un nom, de plus on lui attribue l'adresse ip suivante "172.28.96.1" en /24 : 
   </br>
    **`switch(config)#int vlan 240`**
    </br>
    **`switch(config)#ip address 172.28.96.1 255.255.255.0`**
     </br>
    **`switch(config-if)#no sh`**
    </br>
    **`switch(config-if)#ex`**
    </br>
    **`switch(config)#vlan 240`**
    </br>
    **`switch(config-vlan)#name Management`**
   - On peut repéter ce processus pour créer d'autres VLANs si nécessaire nottament pour la création de la DMZ.

4. **Attribution des Ports aux VLANs :**
   - On accéde au mode de configuration de l'interface du port qu'on souhaite attribuer à un VLAN pour notre part on va lui attribuer le port g0/1.
   </br>
   **`switch(config)#int g0/1`**
   </br>
   **`switch(config-if)#switchport mode access`**
   </br>
   **`switch(config-if)#switchport access vlan 240`**

## Activation du Routing :
- Le routage doit tout d'abord être activé sur le switch.
</br>
**`switch(config)#ip routing`**
</br>
## Mise en place du TFTP
- Pour pouvoir mettre en place le tftpd on doit d'abord ouvrir un serveur tftp.
- Pour ce faire on se rend sur MobaXterm, dans l'onglet "Servers" qui se trouve dans la barre du haut, le port est automatiquement configuré sur le port 69, il suffit juste de l'activer et sur notre switch qui se trouve sur le même réseau, on entre la commande suivante :
</br>
**`copy running-config tftp:`**
  
## Configuration du relai DHCP
- Le relai DHCP est fait pour indiquer ou se trouve le serveur DHCP
</br>
- Se placer dans l'interface du vlan 241 (vlan serveur) **`int vlan 241`** puis **`ip helper-adress 172.28.97.10`**
</br>
