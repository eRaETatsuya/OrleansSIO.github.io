# Compte Rendu - Configuration Nginx et Reverse Proxy

## Objectif
Configurer Nginx en tant que serveur web et reverse proxy sur un serveur Debian 12. Rediriger le trafic HTTP vers HTTPS, mettre en place un reverse proxy pour plusieurs sites, et configurer SSL.

## Étapes Accomplies

### 1. Installation de Nginx
```bash```
**``sudo apt update``**
**``sudo apt install nginx``**

2. Configuration de Base
Réglage du pare-feu pour autoriser le trafic HTTP/HTTPS.
Démarrage du service Nginx.

3. # Reverse Proxy avec Nginx
Configuration d'un fichier de site .conf dans le dossier /etc/nginx/nginx.conf pour le reverse proxy.
Utilisation de proxy_pass pour rediriger le trafic vers une application interne.
# Redirection HTTP vers HTTPS
Configuration des blocs de serveurs pour rediriger le trafic HTTP vers HTTPS.
Utilisation de return 301 pour effectuer la redirection.
# Configuration SSL/TLS
Création d'un fichier PEM combinant certificat et clé privée.
Configuration du serveur Nginx pour utiliser le fichier PEM.
# Gestion de Plusieurs Sites
Configuration de plusieurs blocs de serveurs pour différents domaines.
Utilisation de proxy_pass pour rediriger le trafic vers des serveurs internes distincts.

        server {
          listen 80;
          server_name orleans.sportludique.fr;
          return 301 https://orleans.sportludique.fr$request_uri;
        }

        server {
             listen 80;
             server_name blog.orleans.sportludique.fr;
             return 301 https://blog.orleans.sportludique.fr$request_uri;
        }

        server {

                listen 443 ssl;

                server_name orleans.sportludique.fr;


                ssl_certificate /orleans-sportludique/certificat_et_cle.pem;

                ssl_certificate_key /orleans-sportludique/certificat_et_cle.pem;



                ssl_protocols TLSv1.2 TLSv1.3;

                ssl_prefer_server_ciphers off;





                location / {

                            proxy_pass https://192.168.45.253;

                            proxy_set_header Host $host;

                            proxy_set_header X-Real-IP $remote_addr;

                            proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;

                            proxy_set_header X-Forwarded-Proto $scheme;

                }

         }



         server {

                 listen 443 ssl;

                 server_name blog.orleans.sportludique.fr;


                ssl_certificate /orleans-sportludique/certificat_et_cle.pem;
                ssl_certificate_key /orleans-sportludique/certificat_et_cle.pem;
                ssl_protocols TLSv1.2 TLSv1.3;
                ssl_prefer_server_ciphers off;



               # Autres directives de configuration SSL/TLS...



                 location / {

                             proxy_pass https://192.168.45.253;

                             proxy_set_header Host $host;

                             proxy_set_header X-Real-IP $remote_addr;

                             proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;

                             proxy_set_header X-Forwarded-Proto $scheme;

                 }

         }
         server {

                 listen 80;

                 server_name node.orleans.sportludique.fr;


               # Autres directives de configuration SSL/TLS...



                 location / {

                             proxy_pass http://192.168.45.251:80;

                             proxy_set_header Host $host;

                             proxy_set_header X-Real-IP $remote_addr;

                             proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;

                             proxy_set_header X-Forwarded-Proto $scheme;

                 }

         }
8. Vérification et Tests
Utilisation de commandes comme sudo nginx -t pour vérifier la configuration.
Tests d'accès aux sites depuis un navigateur pour s'assurer que les redirections et le reverse proxy fonctionnent correctement.
9. Problèmes Rencontrés
Gestion des erreurs liées à la suppression d'une entrée statique NAT sur un routeur Cisco.
Vérification des traductions NAT, connexions actives, et suppression des sessions pour résoudre des problèmes de suppression d'entrée NAT.
Conclusion
La configuration de Nginx en tant que serveur web et reverse proxy a été réalisée avec succès. Les différentes étapes ont permis de mettre en place des redirections, de configurer SSL/TLS, et de gérer plusieurs sites sur un même serveur.


La configuration finale permet à Nginx de servir plusieurs sites web de manière sécurisée avec des redirections appropriées.