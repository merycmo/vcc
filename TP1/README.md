# Compte Rendu — TP1 : Virtualisation KVM/QEMU, Clonage cloud-init et Formats Disques

## 1. Objectifs du TP
- Déployer et vérifier des instances Linux (`app01` et `app02`) personnalisées via `cloud-init` (`user-data` / `meta-data`) sous `libvirt` (`qemu:///system`).
- Vérifier l'unicité des identités système (`hostname`, `machine-id`, `/etc/tp-instance`, clés SSH).
- Créer, inspecter et comparer les formats d'images disques **RAW** et **QCOW2** (`qemu-img`, `ls -lh`, `du -sh`, `qemu-img convert`).

## 2. Vérification des instances (`app01` & `app02`)
- Connexion SSH par clé (`~/.ssh/tpl_kvm`) avec l'utilisateur `etudiant`.
- Validation de `app01` (`hostnamectl --static`, `/etc/machine-id`, `/etc/tp-instance`).
- Validation de `app02` et gestion du changement d'empreinte SSH (`ssh-keygen -R`) lors du partage d'IP DHCP.

## 3. Comparaison RAW vs QCOW2 (Exercice 11)
- **Taille virtuelle** : `5 GiB` pour `disque_test.raw` et `disque_test.qcow2`.
- **Occupation réelle (`du -sh`)** : `4.0K` pour `disque_test.raw` (fichier *sparse*) contre `196K` pour `disque_test.qcow2` (en-tête et métadonnées).
- **Conversion** : `qemu-img convert -f raw -O qcow2` produit une image `qcow2` de `196 KiB`.

## 4. Organisation des preuves et annexes
- `preuves/` : exports XML, empreintes `.sha256` et sorties de commandes (`.txt`).
- `fiches/` : fiches descriptives de l'image OS et du template.
- `tableaux/` : tableaux comparatifs `source-vs-clone.md` et `app01-vs-app02.md`.
- `cloud-init/` : fichiers `user-data` et `meta-data`.
- `formats-raw-qcow2.md` et `cas-usage.md` : analyses détaillées et études de cas.
