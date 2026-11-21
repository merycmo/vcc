# Fiche technique — Template (`template-v1`)

## 1. Identification du template
- **Identifiant du template** : `template-v1`
- **Référence d'instance observée** : `ubuntu-template-v1` (visible dans `/etc/tp-instance` sur `app01`)[cite: 5]
- **Système d'exploitation** : Ubuntu 24.04.5 LTS (GNU/Linux 6.8.0-147-generic, `x86_64`)[cite: 2, 4, 5, 8]
- **Hyperviseur** : KVM/QEMU géré via `libvirt` (`qemu:///system`)

## 2. Caractéristiques du disque & intégrité
- **Fichier du template** : `[nom_du_fichier_template.qcow2 ou .img]`
- **Format de disque** : `[qcow2 / raw]`
- **Taille virtuelle déclarée** : `[ex. 10 GiB / 20 GiB]`
- **Occupation réelle sur l'hôte (`du -sh`)** : `[ex. 2.4G]`
- **Empreinte d'intégrité (`SHA-256`)** : `[coller le hash depuis preuves/template.sha256]`

## 3. Préparation et personnalisation `cloud-init`
- **Mécanisme de personnalisation** : `cloud-init` (`user-data` et `meta-data`) appliqué lors du démarrage des instances.
- **Accès distant** : Clé publique injectée pour l'utilisateur `etudiant`, permettant la connexion SSH avec `~/.ssh/tpl_kvm`[cite: 1, 4, 5, 8].
- **Identités configurées au déploiement** :
  - `hostname` distinct (`app01`, `app02`)[cite: 1, 2, 4, 5, 8].
  - `/etc/machine-id` unique par instance[cite: 2, 4, 5].
  - Fichier de traçabilité `/etc/tp-instance` renseignant l'instance et la référence du template[cite: 5].

## 4. Remarques opérationnelles et preuves
- **Réseau / DHCP** : Sur le réseau virtuel `192.168.122.0/24`, la réutilisation d'une adresse IP (ex. `192.168.122.254`) entre deux instances successives entraîne un changement d'empreinte SSH côté client, nécessitant `ssh-keygen -R <IP>`[cite: 1, 4, 5, 6].
- **Preuves associées** :
  - `../preuves/capture-config.xml`
  - `../preuves/template.sha256`
  - `../cloud-init/user-data` et `../cloud-init/meta-data`