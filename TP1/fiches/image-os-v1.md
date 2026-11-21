# Fiche technique — Image OS de base (`image-os-v1`)

## 1. Identification générale
- **Identifiant de l'image** : `image-os-v1`
- **Système d'exploitation** : Ubuntu 24.04.5 LTS (GNU/Linux 6.8.0-147-generic, architecture `x86_64`)
- **Hyperviseur / Gestionnaire** : KVM/QEMU via `libvirt` (`qemu:///system`)
- **Rôle dans le TP** : Image OS de référence utilisée comme base de travail avant préparation du template (`template-v1`) et déploiement des instances (`app01`, `app02`).

## 2. Caractéristiques du disque
- **Fichier source** : `[nom_du_fichier_image.qcow2 ou .img]`
- **Format de disque** : `[qcow2 / raw]`
- **Taille virtuelle déclarée** : `[ex. 10 GiB / 20 GiB]`
- **Occupation réelle sur l'hôte (`du -sh`)** : `[ex. 2.4G]`
- **Empreinte d'intégrité (`SHA-256`)** : `[coller le hash depuis preuves/capture.sha256]`

## 3. État et configuration initiale
- **Utilisateur / Accès** : Compte `etudiant` configuré avec authentification par clé SSH (`~/.ssh/tpl_kvm`).
- **Réseau** : Interface virtuelle (`vnet` / `enp1s0`) rattachée au réseau `libvirt` (`192.168.122.0/24`) avec attribution d'adresse via DHCP.
- **Personnalisation `cloud-init`** : Socle OS initial avant finalisation du template (`ubuntu-template-v1`) et instanciation des machines cibles.

## 4. Observations et preuves associées
- Configuration XML de référence : `../preuves/source.xml`
- Empreinte de contrôle : `../preuves/capture.sha256`
- Sorties de commandes associées : `../preuves/sorties/`
