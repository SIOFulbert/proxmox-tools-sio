# proxmox-tools-sio

## Description
Package proxmox-tools-sio contenant des scripts permettant de faciliter l'utilisation des cluster proxmox de BTS SIO de Centre-Val-de-Loir.

- **cluster-maintenance :** Active ou désactive le mode maintenance pour tous les noeuds du cluster
- **sync-personal-pool :** création de pools individuels pour tous les membres d'un groupe
- **sync-groupes-pool :** création de pools communs pour tous les groupes "proxmox-\*"
- **delete-removed-pool :** suppression définitive des pools nommés "removed-\*"

Les scripts sont placé dans le répertoire /usr/sbin/ .

Dépend du package proxmox-ve.


## Création du .deb
Réalisez un clone du dépôt :

`git clone https://github.com/SIOFulbert/proxmox-tools-sio.git`

Renommez le dossier **proxmox-tools-sio** en **proxmox-tools-sio_1.1-1_all** et utilisez la commande dpkg-deb pour créer le .deb :

`dpkg-deb --build proxmox-tools-sio_1.1-1_all`

## Installation
Installez le paquet normalement avec dpkg :

`dpkg -i proxmox-tools-sio_1.1-1_all`

Le package est généré dans le répertoire courant.


## Utilisation
### cluster-maintenance enable|disable
Ce script permet l'entrée en mode maintenance de tout les noeuds du cluster avec le paramètre "enable". Cela est utile pour permettre l'extinction totale du cluster.
La sortie du mode maintenance de tous les noeuds est réalisé avec le paramètre "disable".


### sync-personal-pool [groupe]
Ce script permet la création de pools personnels pour les nouveaux membres du groupe spécifié et commençant par « proxmox- » (le préfixe « proxmox- » n'est pas à spécifier dans le pamrètre). Il marque également les pools des membres qui n'existent plus dans le groupe comme supprimés.

Par défaut le script crée des pools commençant par « etudiant- » et se base sur le groupe « proxmox-etudiant ». Il renomme les pools étudiants obsolètes avec le préfixe "removed-".
Vous pouvez modifier les variables du script pour changer son comportement :
- REALM : Nom du royaume proxmox (Active Directory) dont est issu le groupe étudiant (défaut "adsio")
- GROUP : Nom du groupe étudiant dans l'Active Directory (défaut "proxmox-etudiant")
- ROLE : Nom du rôle proxmox a appliquer sur les pools (defaut "etudiant")
- TMPFILE : Fichier temporaire utilisé par le script pour les étudiants (defaut "/tmp/etudiants")
- PREFIX : Préfixe à aplliquer sur les pool étudiants créés (defaut "etudiant-")
- RMPREFIX : Préfixe à appliquer sur les anciens pools (defaut "removed-")


### sync-groupes-pool
Ce script permet la création de pools de groupes pour les nouveaux groupes proxmox importés. Il marque également les pools des groupes qui n'existent plus.

Par défaut le script crée des pools commençant par « groupe- » et se base sur les groupes commençant par « proxmox- ». Il renomme les de groupes obsolètes avec le préfixe "removed-".
Vous pouvez modifier les variables du script pour changer son comportement :
- REALM : Nom du royaume proxmox (Active Directory) dont est issu le groupe étudiant (défaut "adsio")
- ADGROUP : Préfixe des groupe de l'Active Directory (défaut "proxmox-")
- ROLE : Nom du rôle proxmox a appliquer sur les pools (defaut "etudiant")
- TMPFILE : Fichier temporaire utilisé par le script pour les groupes (defaut "/tmp/groupes")
- PREFIX : Préfixe à aplliquer sur les pool de groupe créés (defaut "groupe-")
- RMPREFIX : Préfixe à appliquer sur les anciens pools (defaut "removed-")


### delete-removed-pool
Ce script permet de supprimer les pools marqué comme supprimés ainsi que les VMs associés.

Par défaut ce script se base sur le nom des pools commençant par "removed-".
Vous pouvez modifier la variable "RMPREFIX" du script pour modifier le préfixe des pools à supprimer.
