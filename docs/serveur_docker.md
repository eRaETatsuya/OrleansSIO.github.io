# Création du serveur Docker 

## Étape 1: Création du Serveur Docker sur Nutanix

### 1.1 Prérequis

Avant de commencer, il faut créer la machine virtuelle sur Nutanix, avec un OS Debian.

### 1.2 Installation de Docker

Pour installer Docker, on utilise la documentation officiel.
Sur la machine, recopier ces lignes.

```bash

sudo apt-get update
sudo apt-get install ca-certificates curl gnupg
sudo install -m 0755 -d /etc/apt/keyrings
curl -fsSL https://download.docker.com/linux/debian/gpg | sudo gpg --dearmor -o /etc/apt/keyrings/docker.gpg
sudo chmod a+r /etc/apt/keyrings/docker.gpg

echo \
  "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.gpg] https://download.docker.com/linux/debian \
  $(. /etc/os-release && echo "$VERSION_CODENAME") stable" | \
  sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
sudo apt-get update

sudo apt-get install docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin

```
Pour vérifier que l'installation s'est bien passée :
```bash
sudo docker run hello-world
```

## Étape 2: Création d'un conteneur Node.js

### 2.1 Prérequis

Avant de commencer, il faut créer un nouveau dossier qui va contenir notre application Node.js.

```bash
mkdir test-app
cd test-app
```

### 2.2 Création de l'application Node.js

Création d'un fichier app.js qui doit contenir ce code:

```javascript
// app.js
const http = require('http');

const server = http.createServer((req, res) => {
  res.statusCode = 200;
  res.setHeader('Content-Type', 'text/plain');
  res.end('Hello, Docker!\n');
});

const port = process.env.PORT || 3000;

server.listen(port, () => {
  console.log(`Server running on http://192.168.45.251:${port}`);
});
```

Ensuite créer un fichier Dockerfile qui se situera dans le même répertoire, il doit contenir cela :

```docker
# Use the official Node.js image
FROM node:14

# Set the working directory
WORKDIR /testnodejsdocker/

# Install app dependencies
RUN npm install

# Bundle app source
COPY . .

# Expose the port
EXPOSE 3000

# Command to run the application
CMD ["node", "app.js"]
```

### 2.3 Création de l'image Docker

Pour créer l'image Docker, il faut utiliser la commande suivante :

```bash
sudo docker build -t nodejs .
```

### 2.4 Lancement du conteneur

Pour lancer le conteneur sur le port 80, il faut utiliser la commande suivante :

```bash
sudo docker run -d -p 80:3000 nodejs
```

### 2.5 Vérification de l'application

Pour vérifier que l'application fonctionne, dirigez-vous sur internet et copier **`http://192.168.45.251`**
Comme l'enregistrement DNS CNAME a déjà été fait sur le [**serveur DNS**](/serveur_DNS/) on peut aussi écrire **`http://node.orleans.sportludique.fr`** pour y accéder

# Étape 3: Installation de Portnainer 

## 3.1 Création du conteneur Portainer

Pour créer le conteneur Portainer, il faut utiliser la commande suivante :

```bash
docker run -d -p 9000:9000 --name=portainer --restart=always -v /var/run/docker.sock:/var/run/docker.sock portainer/portainer-ce
```

## 3.2 Vérification de Portainer

Pour vérifier que Portainer fonctionne, dirigez-vous sur internet et copier **`http://192.168.45.251:9000`**



