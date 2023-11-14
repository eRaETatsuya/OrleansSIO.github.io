![SN210](stormshield_SN210.png) 

# Configuration d'un firewall stormshield (SN210)

Pour la configuration du firewall, nous l'avions d'abord réinitialisé en restant appuyer pendant plusieurs secondes sur le bouton qui se trouve à l'arrière du firewall.

1. Pour configurer le pare-feu depuis son interface graphique, il faut se câbler avec un câble rj45 sur le port IN(LAN) du pare-feu
2. Se rendre internet et taper dans l'URL l'interface du pare-feu qui est **`https://10.0.0.254/admin`**
3. Arriver sur la page de connexion les informations de connexion sont admin et admin et vous pouvez également mettre la langue française en déroulant les options.

# Configuration des interfaces réseau du firewall

1. Sur l'interface du stormshield, il faut se rentre dans **`configuration`** et dans **`interfaces`**. 
2. En haut à droite de l'interface stormshield, appuyer sur **écriture** pour avoir des droits de modification. Sélectionner l'interface **IN**, dans l'onglet configuration générale:
- Complétez les informations de la zone Plan d'adressage :
- Champ Adressage : sélectionnez Dynamique / Statique.
- Champ Adresse IPv4 : sélectionnez IP fixe (statique).
- Dans la grille : cliquez sur Ajouter et renseignez 192.168.45.254/24. (LAN)
- Cliquez sur Appliquer pour valider. 

Faire la même chose pour le OUT, et renseignez comme ip 172.28.105.1/24. (WAN)
