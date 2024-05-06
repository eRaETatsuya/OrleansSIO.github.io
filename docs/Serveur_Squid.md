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
