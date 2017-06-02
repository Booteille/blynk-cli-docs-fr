Documentation de Blynk CLI
=======================

# Description
Cet utilitaire fourni un moyen de gérer votre serveur Blynk à partir de votre terminal.

Cet outil est en construction et entièrement expérimental. Il n'est pas conseillé de l'essayer en production.

# Installation
## Pré-requis
Pour que Blynk CLI puisse fonctionner, il faudra que votre configuration respecte les pré-requis suivants :

* Java 8
* NodeJS >= 4.6.1
* npm (ou similaire)

## Installation
Installez Blynk CLI à partir de NPM avec l'option globale activée `-g` :
```console
npm install -g blynk-cli
```

# Utilisation
## Afficher l'aide
Une fois Blynk CLI installé, vous pouvez afficher la liste des commandes existantes avec la commande `--help` :
```console
$ blynk-cli
# OR
$ blynk-cli help
# OR
$ blynk-cli --help
```

Vous pouvez aussi afficher la liste des sous-commandes disponibles pour une commande spécifique en faisant suivre votre commande de `--help` :
```console
$ blynk-cli server --help
```

## Install Blynk Server
Avec Blynk CLI, installer Blynk est rapide et facile :
```console
$ blynk-cli server install
[INFO] Downloading Blynk server v0.24.5
[INFO] Creating default configuration
[OK] Installation complete
```

## Mettre à jour le serveur Blynk
Vous pouvez aussi mettre facilement à jour votre serveur :
```console
$ blynk-cli server update
[INFO] Update v0.24.6 available. Downloading...
[OK] Update complete
[OK] Backup done! You can find it in /home/booteille/.blynkcli/backup/auto-update/b1045268-2f32-4e24-85a3-fb740266d417
```

## Démarrer/Arrêter/Redémarrer le serveur
```console
blynk-cli server start

blynk-cli server status # Affiche l'état du serveur

blynk-cli server stop

blynk-cli server restart
```

## Créer une sauvegarde des données
Vous pouvez créer une sauvegarde de votre dossier data :
```console
$ blynkcli backup create BACKUP
[OK] Backup done! You can find it in /home/booteille/.blynkcli/backup/BACKUP/cee3acd3-1190-4501-bfc1-ba10423c1a07
```

La sauvegarde générée est située dans le dossier choisi dans votre configuration de Blynk CLI.
Par défaut, ce dossier est `<HOME>/.blynkcli/backup`.

Comme vous pouvez le voir dans l'exemple ci-dessus, si l'on souhaite donner le nom "BACKUP" à notre sauvegarde, la sauvegarde générée aura pour nom réel quelque chose comme `BACKUP/cee3acd3-1190-4501-bfc1-ba10423c1a07`.
Chaque sauvegarde possède un identifiant unique en plus du nom que nous lui fournissons. Ce permet de pouvoir créer plusieurs sauvegarde sous le même nom.

Le fichier `<HOME>/.blynkcli/backup/backups.lock` contient des informations spécifiques à chaque sauvegarde.
Voici un exemple des informations générées pour la sauvegarde `BACKUP/cee3acd3-1190-4501-bfc1-ba10423c1a07` :
```console
$ cat /home/booteille/.blynkcli/backup/backups.lock
[
  {
    "name": "BACKUP",
    "uuid": "cee3acd3-1190-4501-bfc1-ba10423c1a07",
    "date": "Thu Jun 01 2017 15:46:50 GMT+0200 (CEST)",
    "server_version": "v0.24.6"
  }
]
```

## Restore from a backup
Vous pouvez ensuite restaurer votre sauvegarde :
```console
$ blynkcli backup restore BACKUP
[OK] Restored from backup /home/booteille/.blynkcli/backup/BACKUP/cee3acd3-1190-4501-bfc1-ba10423c1a07
```

Maintenant, admettons que nous avons créé deux sauvegardes ayant le même nom, si nous souhaitons faire une restauration :
```console
$ blynkcli backup restore BACKUP
[WARN] There are 2 backup found with corresponding names:
BACKUP/812de666-6d21-4f36-8877-cc1f775dab73 Thu Jun 01 2017 15:58:44 GMT+0200 (CEST)
BACKUP/cee3acd3-1190-4501-bfc1-ba10423c1a07 Thu Jun 01 2017 15:46:50 GMT+0200 (CEST)
[ERR]  Please, retry with one of these backups
```

L'erreur indique ici que nous devons définir un nom plus précis.

Ainsi, nous souhaitons restaurer la sauvegarde `BACKUP/812de666-6d21-4f36-8877-cc1f775dab73` :
```console
$ blynkcli backup restore BACKUP/812
[OK] Restored from backup /home/booteille/.blynkcli/backup/BACKUP/812de666-6d21-4f36-8877-cc1f775dab73
```

Vous pouvez constater qu'il n'y a pas eu besoin de taper l'identifiant entier mais simplement les premiers caractères. (Ici, écrire `BACKUP/8` aurait même suffit car il n'y a pas d'autre sauvegarde nommée BACKUP ayant un identifiant commençant par un 8)

Vous pouvez aussi restaurer votre sauvegarde directement à partir de l'identifiant (uuid) :
```console
$ blynkcli backup restore /cee
[OK] Restored from backup /home/booteille/.blynkcli/backup/BACKUP/cee3acd3-1190-4501-bfc1-ba10423c1a07
```

## Ajouter un utilisateur
Vous pouvez ajouter un utilisateur en ligne de commandes :
```console
$ blynkcli user add
? Email:  booteille@booteille.com
? Password:  [hidden]
? Confirm your password:  [hidden]
? Is super admin?  true
[OK] User booteille@booteille.com added
```

## Modifier les propriétés d'un utilisateur
Voici un exemple pour modifier l'énergie totale d'un utilisateur :
```console
$ blynkcli user set booteille@booteille.com energy 15000
[OK] Property energy set to 15000
[WARN] You must restart the server to apply the effect
```
Pour afficher une propriété :
```console
$ blynkcli user get booteille@booteille.com energy
energy: 15000
```

## Cloner les projets d'un utilisateur à un autre
```console
blynkcli user clone-projects booteille@booteille.com admin@blynk.cc
[OK] admin@blynk.cc projects cloned from booteille@booteille.com
[WARN] You must restart the server to apply the effect
[OK] Backup done! You can find it in /home/sephir/.blynkcli/backup/auto-cloneProfile/07221e9e-3f09-46dc-914a-
```

## Modifier le mot de passe d'un utilisateur
```console
blynkcli user password booteille@booteille.com
? Password:  [hidden]
? Confirm your password:  [hidden]
[WARN] You must restart the server to apply the effect
```
