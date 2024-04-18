# Installation et configuration d'Apache Guacamole sur Debian 11

## Étape 1 : Installation des dépendances

Sur notre machine Debian, nous commençons par installer les dépendances nécessaires avec les commandes suivantes :


**`sudo apt-get update`**
**`sudo apt-get install build-essential libcairo2-dev libjpeg62-turbo-dev libpng-dev libtool-bin uuid-dev libossp-uuid-dev libavcodec-dev libavformat-dev libavutil-dev libswscale-dev freerdp2-dev libpango1.0-dev libssh2-1-dev libtelnet-dev libvncserver-dev libwebsockets-dev libpulse-dev libssl-dev libvorbis-dev libwebp-dev`**

## Étape 2 : Téléchargement et installation de guacamole-server

Nous nous positionnons dans le répertoire /tmp et téléchargeons l'archive tar.gz de Guacamole Server :

**`cd /tmp`**
**`wget https://downloads.apache.org/guacamole/1.5.2/source/guacamole-server-1.5.2.tar.gz`**
**`# ou `**
**`wget https://dlcdn.apache.org/guacamole/1.5.2/source/guacamole-server-1.5.2.tar.gz`**
**``**

Une fois le téléchargement terminé, nous décomprimons l'archive et nous nous positionnons dans le répertoire obtenu :

**`tar -xzf guacamole-server-1.5.2.tar.gz`**
**`cd guacamole-server-1.5.2/`**

Nous configurons et compilons le code source de guacamole-server :

**`sudo ./configure --with-init-dir=/etc/init.d`**
**`sudo make`**
**`sudo make install`**

Nous mettons à jour les liens entre guacamole-server et les librairies :

**`sudo ldconfig`**

## Étape 3 : Configuration et démarrage du service guacd

Nous démarrons le service guacd et le configurons pour un démarrage automatique :

**`sudo systemctl daemon-reload`**
**`sudo systemctl start guacd`**
**`sudo systemctl enable guacd`**

Nous vérifions le statut du service :

**`sudo systemctl status guacd`**

## Étape 4 : Configuration du répertoire de Guacamole

Nous créons l'arborescence pour la configuration d'Apache Guacamole :

                  sudo mkdir -p /etc/guacamole/{extensions,lib}

## Étape 5 : Installation de Tomcat9 et de la Web App de Guacamole

Nous installons Tomcat9 :

                    sudo apt-get install tomcat9 tomcat9-admin tomcat9-common tomcat9-user

Nous téléchargeons la Web App d'Apache Guacamole et la déplaçons dans le répertoire de Web App de Tomcat9 :

                    cd /tmp
                    wget https://downloads.apache.org/guacamole/1.5.2/binary/guacamole-1.5.2.war
                    sudo mv guacamole-1.5.2.war /var/lib/tomcat9/webapps/guacamole.war

Nous redémarrons les services Tomcat9 et guacd :

                    sudo systemctl restart tomcat9 guacd


