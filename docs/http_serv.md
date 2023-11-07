# Étapes d'Installation

## Préparation du Système :
Avant de commencer, j'ai vérifié que mon système Linux était à jour en utilisant la commande suivante :

</br>

**`sudo apt update`**
**`sudo apt upgrade`**

</br>

## Installation d'Apache :
Pour installer Apache, j'ai exécuté la commande suivante :

**`sudo apt install apache2`**

## Vérification d'Apache :
Après l'installation, j'ai vérifié qu'Apache fonctionnait correctement en accédant à l'adresse IP du serveur depuis un navigateur web. J'ai vu la page par défaut d'Apache, ce qui signifie que l'installation était réussie.

# Mise en place du HTTPS

## Création de la nouvelle CA :

On a généré une clé privée pour la nouvelle CA :
</br>
**`openssl genrsa -out nouvelle_ca.key 4096`**
</br>
Ensuite, on a créé un certificat pour cette CA :
</br>
**`openssl req -x509 -new -nodes -key nouvelle_ca.key -sha256 -days 365 -out nouvelle_ca.crt`**
</br>
On a rempli les informations demandées pour le certificat.

Génération de la clé privée pour le certificat du serveur :
</br>
**`openssl genrsa -out server.key 2048`**
</br>
Création d'une demande de signature de certificat (CSR) pour le serveur :

On a généré un CSR pour le serveur avec la clé privée créée précédemment :
</br>
**`openssl req -new -key server.key -out server.csr`**
</br>
On a fourni les informations nécessaires pour le certificat.

Signature du certificat du serveur avec la nouvelle CA :

On a signé le CSR du serveur avec la nouvelle CA pour émettre un certificat :
</br>
**`openssl x509 -req -in server.csr -CA nouvelle_ca.crt -CAkey nouvelle_ca.key -CAcreateserial -out server.crt -days 365 -sha256`**
</br>

  On modifie le fichier .conf de notre site d'Orleans qui se trouve dans cette racine /etc/apache2/sites-available  
  <VirtualHost *:80>
      ServerName Orleans.sportludique.fr
      ServerAlias www.orleans.sportludique.fr
  
      Redirect permanent / https://192.168.45.253
  </VirtualHost>
  
  <VirtualHost *:443>
      ServerName Orleans.sportludique.fr
      ServerAlias www.orleans.sportludique.fr
      DocumentRoot /var/www/html
  
      SSLEngine on
      SSLCertificateFile /certificats/server.crt
      SSLCertificateKeyFile /mykey/server.key
  
  </VirtualHost>
