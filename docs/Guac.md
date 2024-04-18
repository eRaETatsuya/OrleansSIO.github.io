# Installation et configuration d'Apache Guacamole sur Debian 11

## Étape 1 : Installation des dépendances

Sur notre machine Debian, nous commençons par installer les dépendances nécessaires avec les commandes suivantes :


                      sudo apt-get update
                      sudo apt-get install build-essential libcairo2-dev libjpeg62-turbo-dev libpng-dev libtool-bin uuid-dev libossp-uuid-dev libavcodec-dev libavformat-dev libavutil-dev libswscale-dev freerdp2-dev libpango1.0-dev libssh2-1-dev libtelnet-dev libvncserver-dev libwebsockets-dev libpulse-dev libssl-dev libvorbis-dev libwebp-dev

## Étape 2 : Téléchargement et installation de guacamole-server

Nous nous positionnons dans le répertoire /tmp et téléchargeons l'archive tar.gz de Guacamole Server :

        cd /tmp
        wget https://downloads.apache.org/guacamole/1.5.2/source/guacamole-server-1.5.2.tar.gz
        # ou 
        wget https://dlcdn.apache.org/guacamole/1.5.2/source/guacamole-server-1.5.2.tar.gz


Une fois le téléchargement terminé, nous décomprimons l'archive et nous nous positionnons dans le répertoire obtenu :

        tar -xzf guacamole-server-1.5.2.tar.gz
        cd guacamole-server-1.5.2/

Nous configurons et compilons le code source de guacamole-server :

          sudo ./configure --with-init-dir=/etc/init.d
          sudo make
          sudo make install

Nous mettons à jour les liens entre guacamole-server et les librairies :

              sudo ldconfig

## Étape 3 : Configuration et démarrage du service guacd

Nous démarrons le service guacd et le configurons pour un démarrage automatique :

                  sudo systemctl daemon-reload
                  sudo systemctl start guacd
                  sudo systemctl enable guacd

Nous vérifions le statut du service :

                  sudo systemctl status guacd

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
## Étape 6 : Configuration de la base de données MariaDB

Nous installons MariaDB Server :

                    sudo apt-get install mariadb-serv
                    
Nous sécurisons l'installation de MariaDB :

                    sudo mysql_secure_installation
Nous nous connectons à MariaDB pour créer une base de données et un utilisateur dédié à Guacamole :

      mysql -u root -p


      CREATE DATABASE guacadb;
      CREATE USER 'admin'@'localhost' IDENTIFIED BY 'P@ssword!';
      GRANT SELECT,INSERT,UPDATE,DELETE ON guacadb.* TO 'guaca_nachos'@'localhost';
      FLUSH PRIVILEGES;
      EXIT;

## Étape 7 : Configuration de l'authentification MySQL pour Guacamole

Nous téléchargeons et installons l'extension MySQL pour Guacamole :

              cd /tmp
              wget https://downloads.apache.org/guacamole/1.5.2/binary/guacamole-auth-jdbc-1.5.2.tar.gz
              tar -xzf guacamole-auth-jdbc-1.5.2.tar.gz
              sudo mv guacamole-auth-jdbc-1.5.2/mysql/guacamole-auth-jdbc-mysql-1.5.2.jar /etc/guacamole/extensions/

Nous téléchargeons et installons le connecteur MySQL :

          cd /tmp
          wget https://dev.mysql.com/get/Downloads/Connector-J/mysql-connector-j-8.0.33.tar.gz
          tar -xzf mysql-connector-j-8.0.33.tar.gz
          sudo cp mysql-connector-j-8.0.33/mysql-connector-j-8.0.33.jar /etc/guacamole/lib/

Nous importons la structure de la base de données de Guacamole dans la base de données guacadb :

        cd guacamole-auth-jdbc-1.5.2/mysql/schema/
        cat *.sql | mysql -u root -p guacadb

Nous créons et éditons le fichier guacamole.properties :

      sudo nano /etc/guacamole/guacamole.properties

Nous insérons les lignes suivantes :

            # MySQL
            mysql-hostname: 127.0.0.1
            mysql-port: 3306
            mysql-database: guacadb
            mysql-username: admin
            mysql-password: P@ssword!

Nous éditons le fichier guacd.conf :            
            
          sudo nano /etc/guacamole/guacd.conf

Nous insérons le code suivant :

      [server]
      bind_host = 0.0.0.0
      bind_port = 4822

Nous redémarrons les services liés à Guacamole :

        sudo systemctl restart tomcat9 guacd mariadb


On peut maintenant se connecter à notre interface web en entrant ``*172.28.97.30:8080/guacamole*`` ou ``*guac.orleans.sportludique.fr:8080/guacamole*``

![Connexion-a-Apache-Guacamole](https://github.com/eRaETatsuya/OrleansSIO.github.io/assets/144692551/38956b58-21a4-455c-bfb0-d9ac045d489c)

