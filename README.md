# Compte Rendu — TP1 : Virtualisation KVM/QEMU, Clonage cloud-init et Formats Disques

## 1. Objectifs du TP
- Déployer et vérifier des instances Linux (`app01` et `app02`) personnalisées via `cloud-init` (`user-data` / `meta-data`) sous `libvirt` (`qemu:///system`).
- Vérifier l'unicité des identités système (`hostname`, `machine-id`, `/etc/tp-instance`, clés SSH).
- Créer, inspecter et comparer les formats d'images disques **RAW** et **QCOW2** (`qemu-img`, `ls -lh`, `du -sh`, `qemu-img convert`).

## 2. Vérification des instances (`app01` & `app02`)
- Connexion SSH par clé (`~/.ssh/tpl_kvm`) avec l'utilisateur `etudiant`[cite: 1].
- Validation de `app01` (`hostnamectl --static`, `/etc/machine-id`, `/etc/tp-instance`)[cite: 1].
- Validation de `app02` et gestion du changement d'empreinte SSH (`ssh-keygen -R`) lors du partage d'IP DHCP (`192.168.122.254`)[cite: 1].

## 3. Comparaison RAW vs QCOW2 (Exercice 11)
- **Taille virtuelle** : `5 GiB` pour `disque_test.raw` et `disque_test.qcow2`[cite: 2].
- **Occupation réelle (`du -sh`)** : `4.0K` pour `disque_test.raw` (fichier *sparse*) contre `196K` pour `disque_test.qcow2` (en-tête et métadonnées)[cite: 2].
- **Conversion** : `qemu-img convert -f raw -O qcow2` produit une image `qcow2` de `196 KiB`[cite: 2].

## 4. Captures d'écran et preuves visuelles
Les captures d'écran sont classées dans `preuves/captures/` :
- `preuves/captures/01-ssh-app02.png` : connexion SSH et vérification de l'instance `app02`[cite: 1].
- `preuves/captures/02-identite-app01.png` : vérification des identités de `app01` (`hostname`, `machine-id`, `/etc/tp-instance`)[cite: 1].
- `preuves/captures/03-formats-raw-qcow2.png` : création, inspection (`qemu-img info`), comparaison (`ls -lh` / `du -sh`) et conversion (`qemu-img convert`) des disques RAW et QCOW2[cite: 2].

![Comparaison RAW vs QCOW2](preuves/captures/03-formats-raw-qcow2.png)

## 5. Organisation du dossier `TP1`
- `preuves/` : exports XML, empreintes `.sha256`, sorties de commandes (`.txt`) et `captures/`.
- `fiches/` : fiches descriptives (`image-os-v1.md` et `template-v1.md`).
- `tableaux/` : tableaux comparatifs (`source-vs-clone.md` et `app01-vs-app02.md`).
- `cloud-init/` : fichiers `user-data` et `meta-data`.
- `formats-raw-qcow2.md` et `cas-usage.md` : analyses détaillées, 8 études de cas et conclusion personnelle.