![Catalyst 3560](catalyst.png )
# Guide d'Installation de SSH et Création de VLAN sur un Switch Cisco


## Installation de SSH sur le Switch

1. **Connexion au Switch :** On se connecte au switch Cisco via le câble console en utilisant un client de terminal.

2. **Configuration SSH :**
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

   - De plus on a généré une paire de clés SSH RSA pour l'authentification avec la commande **`switch(config)# crypto key generate rsa`** puis ensuite vous entrez **`2048`**.

## Création de VLAN sur le Switch

3. **Configuration des VLANs :**
</br>
   - On accède au mode de configuration VLAN en conf-t.
   - Pour créer un VLAN il faut spécifier son numéro, pour créer notre vlan Management on utilise le vlan 240 et on lui donne un nom significatif : 
   </br>
    **`switch(config)# int vlan 240`**
    </br>
    **`switch(config-if)# no sh`**
    </br>
    **`switch(config-if)# ex`**
    </br>
    **`switch(config)#vlan 240`**
    </br>
    **`switch(config-vlan)# name Management`**
   - On peut repéter ce processus pour créer d'autres VLANs si nécessaire.

4. **Attribution des Ports aux VLANs :**
   - On accéde au mode de configuration de l'interface du port qu'on souhaite attribuer à un VLAN pour notre part on va lui attribuer le port g0/1.
   </br>
   **`switch(config)# int g0/1`**
   </br>
   **`switch(config-if)# switchport mode access`**
   </br>
   **`switch(config-if)# switchport access vlan 240`**


