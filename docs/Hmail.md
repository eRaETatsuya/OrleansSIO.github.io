# Configuration du Serveur HMail sur Windows 10

1. **Téléchargement et Installation de HMail**
   - On télécharge la dernière version stable depuis [le site officiel](https://www.hmailserver.com/).
   - On suit les instructions d'installation.

2. **Configuration Initiale de HMail**
   - On exécute l'assistant de configuration.
   - On définit le nom de domaine et on ajoute des domaines, des utilisateurs et des boîtes aux lettres.

3. **Paramètres de Sécurité**
   - On configure les certificats SSL et on active le chiffrement TLS/SSL.

4. **Paramètres SMTP/POP3/IMAP**
   - On configure les ports et les autorisations pour SMTP et IMAP.

# Modifications dans le Serveur DNS

1. **Enregistrements MX (Mail Exchange)**
   - On ajoute un enregistrement MX pour le domaine. 

![image](https://github.com/eRaETatsuya/OrleansSIO.github.io/assets/144692551/d49a5839-5d37-4cbf-b223-57677c129ff5)
</br>
![image](https://github.com/eRaETatsuya/OrleansSIO.github.io/assets/144692551/1307b741-390f-4924-9f3d-63b0da1d0339)

2. **Redirection de Ports sur le Switch et le Pare-feu**

  - On redirige les ports 25 (SMTP) et 143 (IMAP) vers l'adresse IP du serveur HMail.
  - On configure les règles de redirection de ports sur le pare-feu.
3. **Test**
    - On vérifie la connectivité en envoyant/recevant des e-mails depuis l'extérieur.
