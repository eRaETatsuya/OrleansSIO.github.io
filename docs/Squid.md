# Installation Squid/SquidGuard

## 1. Constatation

### 1.1 Besoin

Pour installer le proxy nous avons besoins de créer une machine virtuelle Debian sur Nutanix
    

### 1.2 Objectifs

À partir d’un navigateur se connecter à [http://dsi.ut-capitole.fr/hidden/test](http://dsi.ut-capitole.fr/hidden/test) (le test ne marcherait pas bien en HTTPS et il faut que l'ip du proxy soit recensé dans le navigateur Firefox) On constate alors l’information ”User-Agent” qui indique le nom et la version du navigateur et le nom et la version de l’OS. Cette information, utile, va nous donner l’occasion de voir ce qui peut être fait par un relai applicatif.

## 2. Squid

### 2.1 Installation

L’installation du relai proxy squid est assez simple (nous supposons utiliser squid3, mais dans la toute dernière Debian, c’est squid 4, qui est dénommé ”squid” tout court).

```bash
apt-get install squid
```

Après l'installation, un `squid.conf` s'est crée, remplit de commentaire, il faut supprimer l'inutile :

```bash
cd /etc/squid3
# On cree une copie sous le nom squid.conf.distrib
cp squid.conf squid.conf.distrib
# On enleve les commentaires, puis on ne prend que les lignes avec 2 caracteres
cat squid.conf|grep -Pv '^#'|grep -P '..' > squid.conf.new
cp squid.conf.new squid.conf
```
On peut alors lancer le squid
```bash
service squid start
```

Dans le cas où l’on utilise la machine virtuelle, il sera nécessaire d’autoriser le réseau, où se trouve la machine
cliente (la machine physique), à utiliser le squid local. Dans le fichier squid.conf on va donc ajouter une définition
d’ACL, puis utiliser cette définition. Il sera important de bien choisir l’ordre dans lequel on va placer les acl et
leur utilisation.

```bash
acl localclient src 172.16.0.0/12 #Cette ACL est déjà créée mais dans l'installation de squid l'ACL a pour nom localnet moi je l'ai renommée localclient (en laissant le src)
...
http_access allow localclient #Il faut créer cette ACL
```

### 2.2 Vérification

Pour vérifier que le fichier est conforme et sans erreur:

```bash
squid -k check
```

### 2.3 Utilisation

Se diriger vers Firefox, puis dans les paramètres, chercher proxy, puis recenser l'ip et le port du proxy:
Le notre c'est 172.28.98.253:3128 et cliquer sur `Utiliser également ce proxy pour HTTPS`

### 3 Installation de SquidGuard

Squid ne peut bloquer que des URLs une par une, ce qui est long, donc on va installer SquidGuard

```bash
apt-get install squidguard
```
Une fois ceci fait, on signale à squid d’utiliser squidGuard. Pour cela, on modifie /etc/squid3/squid.conf
```bash
url_rewrite_program /usr/bin/squidGuard
```

### 3.2 Configuration du logiciel

La configuration doit se faire avant l’installation des bases de données. Le fichier de configuration est, en général,
/etc/squidGuard/squidGuard.conf

### 3.3 Les bases de filtrage
On rapatrie une base de filtrage et on l’installe

```bash
cd /var/lib/squidguard/db
wget http://dsi.ut-capitole.fr/blacklists/download/blacklists.tar.gz
# On decompacte, un repertoire blacklists est alors cree
tar zxvf blacklists.tar.gz
# On cree un lien symbolique pour garder la notion de dest
ln -s blacklists dest
# Pour accelerer le traitement (mais en reduisant considerablement le nombre de domaines filtres)
cd dest/adult
cp domains domains.originel
echo "playboy.com" >domains
echo "sex.com" >>domains
echo "lesitequevousvoulezajouter.com" >> domains
chown proxy:proxy domains
```

La structure des fichiers textes contenant la liste est créée. Squidguard peut fonctionner tel quel (il recompilera les bases en m´emoire au démarrage), mais cela ralentira considérablement tout démarrage ou redémarrage.
Il faut donc créer (compiler) les bases de donn´ees avant le lancement.
Il faudra prendre la précaution d’arrêter le squid avant, si on veut que la compilation soit rapide.

```bash
mv database db
service squid stop
/usr/bin/squidGuard -P -d -b -C all
chown -R proxy:proxy /var/lib/squidguard/db
service squid start
```
Cette commande va compiler les bases de donn´ees qui sont utilis´ees dans le squidGuard.conf. Comme la
commande a ´et´e faite par root, les fichiers domains.db et urls.db lui appartiennent, il faut changer le propri´etaire.
On peut alors relancer le squid