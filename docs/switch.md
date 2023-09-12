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
   - Accédez au mode de configuration VLAN.
   - Créez un VLAN en spécifiant son numéro  et donnez-lui un nom significatif.
   - Répétez ce processus pour créer d'autres VLANs si nécessaire.

4. **Attribution des Ports aux VLANs :**
   - Accédez au mode de configuration de l'interface du port que vous souhaitez attribuer à un VLAN.
   - Associez le port au VLAN spécifié (par exemple, VLAN 10).

