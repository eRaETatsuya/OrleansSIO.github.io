## ACL appliquée sur le réseau d'Orléans

Tout d'abord, configurez la règle ACL qui autorise spécifiquement le trafic SSH entrant :

**`SW-ORL(config)# access-list 100 permit tcp any any eq 22`**
</br>
Cette commande permet le trafic TCP entrant sur le port 22 (SSH) depuis n'importe quelle source.

Ensuite, configurez la règle ACL qui bloque tout autre trafic IP entrant :


**`SW-ORL(config)# access-list 100 deny ip any any`**
</br>
Cette commande bloque tout autre type de trafic IP entrant depuis n'importe quelle source.

Une fois que les règles ACL sont configurées, accédez à la configuration de l'interface VLAN 240 :

**`SW-ORL(config)# int vlan 240`**
</br>
Vous êtes maintenant dans la configuration de l'interface VLAN 240.

Appliquez la liste d'accès (ACL) que vous avez créée (numéro 100) à l'interface VLAN 240 en direction de l'entrée (in) :


**`SW-ORL(config-if)# ip access-group 100 in`**
</br>
Cela signifie que les règles de la liste d'accès seront appliquées au trafic entrant sur l'interface VLAN 240.

Enfin, quittez la configuration de l'interface VLAN 240 :

**`SW-ORL(config-if)# exit`**

