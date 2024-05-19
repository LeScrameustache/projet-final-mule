# Projet final

## 1. Présentation

*Schéma*

Les trois fichiers .jar sont à importer dans AnyPoint Studio, ils ne peuvent pas être déployés directement.

Ports utilisés en dev (fichier dev.yaml):
- s-covidtracking-api : 8081
- p-covidusa-api : 8082
- s-usdepartment-api : 8083

## 2. Instructions pour le déploiement :

### 2.1. s-covidtracking-api
L'api récupère le données de l'api donnée dans l'énoncé.
Importer l'archive s-covidtracking-api.jar dans AnypointStudio, tester puis exporter le .jar et l'importer dans cloudhub2

**Endpoints :** 
- GET /api/getAll
- GET /api/getState/{state}

### 2.2. s-usdepartment-api
L'api écrit des données du body (en .json) dans un fichier au nom donné en query parameter.
L'api possède 3 configurations (dev.yaml, dev2.yaml et prod.yaml), pour déterminer où écrire le fichier. Voici quelques explications sur ces 3 configurations les modifications éventuelles à leur apporter:
- dev.yaml **(modification obligatoire)** : remplacer 'your-path' par l'emplacement local où vous suohaitez que le fichier soit enregistré.
- dev2.yaml et prod.yaml : les fichiers de sortie sont enregistrés dans un serveur linux accessible par ssh à l'adresse `scrameustache.ddns.net` [(détails sur la connexion ssh)](#connexion-au-serveur-via-ssh). Par défaut, les données sont stockées dans le dossier `/home/log-default`, les fichiers d'un même nom sont écrasés à chaque nouvelle requête. Vous pouvez modifier cette option en suivant [ces instructions](créer-son-espace-de-stockage).
Tester l'api.
Exporter l'archive et la déployer sur Cloudhub2.

**Endpoints :**
- POST /api/write?fileName=foo

### 2.3. p-covidusa-api
**Attention :** Pour effectuer le déploiement en prod de cette api, les deux étapes précédentes doivent impérativement avoir été accomplies.
Copier l'url de votre api s-covidtracking-api sur cloudhub et la coller dans le champ `system.retrieveData.host` du prod.yaml
Copier l'url de votre api s-usdepartment-api sur cloudhub et la coller dans le champ `system.logData.host` du prod.yaml
Tester l'api.
Exporter l'archive et la déployer sur Cloudhub2.

**Endpoints** 
- GET /national
- GET /national/average
- GET /national/{date}
- GET /state?state=ca,fl,ny

## Serveur linux
Vous vous connectez sur mon serveur, soyez gentils et mettez pas le sbeul plz =)

### Connexion au serveur via ssh
User : `mule` 
Mot de passe : `max`
Commande à taper dans la console (windows, linux ou mac) : `ssh mule@scrameustache.ddns.net`

*Remarque :* Il se peut que votre ordinateur vous demande si vous faites confiance au serveur (la connexion n'est pas sécurisée (port 22). Mettre "yes" pour établir la connexion ssh.

Vous pouvez maintenant accéder au dossier `logs-default` avec la commande `cd logs_Default` qui contient les fichiers créés par l'api s-usdepartment-api. 
Pour lire un fichier, vous pouvez utilier les commandes classiques (vim, nano, more, most...)

**Important :** Il s'agit d'un espace partagé, merci de ne pas modifier les fichiers dans les dossiers qui ne sont pas les votres.

### Créer son espace de stockage

Pour retrouver vos tets et vos sorties plus facilement, créer un dossier à votre nom : `mkdir logs_<votre nom>`.
Dans AnypointStudio, modifier les fichiers dev2.yaml et prod.yaml en remplaçant la valeur de `ssh.workingDirectory` (par défaut `logs_default`) par le nom du dossier que vous avez créé.




