# Études de cas et conclusion personnelle — KVM/QEMU, `cloud-init` et formats disques

Ce document présente huit scénarios d'utilisation concrets issus des manipulations réalisées durant le TP1 (`libvirt` / `qemu:///system`, `cloud-init`, `app01` / `app02`, et comparaison `RAW` vs `QCOW2`), suivis d'une conclusion personnelle.

## 1. Études de cas

### Cas 1 : Déploiement reproductible de nœuds applicatifs (`app01`, `app02`) via `cloud-init`
- **Contexte** : Provisionner rapidement plusieurs machines virtuelles Linux (`app01`, `app02`) à partir d'un même modèle (`ubuntu-template-v1`).
- **Approche** : Utilisation de `cloud-init` (`user-data` et `meta-data`) pour injecter la clé SSH de l'utilisateur `etudiant`, définir un `hostname` distinct et inscrire `/etc/tp-instance`.
- **Bénéfice** : Gain de temps, reproductibilité et garantie d'identités distinctes (`hostname`, `/etc/machine-id`, empreintes SSH) dès le premier démarrage.

### Cas 2 : Évitement des collisions d'identifiants système et réseau
- **Contexte** : Cloner une machine virtuelle sans réinitialiser ses identifiants internes ou partager une même adresse IP DHCP (`192.168.122.254`) entre deux instances successives.
- **Approche** : Vérification systématique de `hostnamectl --static`, `/etc/machine-id` et gestion des empreintes SSH côté client (`ssh-keygen -R`) lors de la réutilisation d'une IP.
- **Bénéfice** : Prévention des conflits d'identité et sécurisation des connexions SSH.

### Cas 3 : Optimiser l'espace disque en laboratoire et développement avec `QCOW2`
- **Contexte** : Héberger plusieurs machines virtuelles sur un poste de travail ou un serveur disposant d'un espace disque limité.
- **Approche** : Privilégier le format `qcow2` (allocation dynamique / *thin provisioning*, `196 KiB` à la création pour `5 GiB` virtuels).
- **Bénéfice** : Réduction immédiate de l'empreinte physique sur l'hôte tant que les blocs ne sont pas écrits.

### Cas 4 : Maximiser les performances E/S pour une charge intensive avec `RAW`
- **Contexte** : Déployer une machine virtuelle hébergeant une base de données ou un service sensible à la latence disque.
- **Approche** : Utiliser le format `raw` (accès direct sans surcouche de métadonnées `QCOW2`, `4 KiB` alloués initialement sur fichier *sparse* pour `5 GiB` virtuels).
- **Bénéfice** : Débit et latence optimisés au niveau de la couche disque virtuelle.

### Cas 5 : Sécuriser les mises à jour et tests applicatifs par *snapshots*
- **Contexte** : Appliquer une mise à jour système ou tester une nouvelle configuration avant validation.
- **Approche** : S'appuyer sur un format `qcow2` permettant de prendre un instantané (*snapshot*) avant modification et de revenir à l'état sain en cas d'incident.
- **Bénéfice** : Retour arrière rapide et sans risque pour l'environnement de test.

### Cas 6 : Migration et conversion de formats d'images (`qemu-img convert`)
- **Contexte** : Faire évoluer une image disque d'un format vers un autre (par exemple convertir une image `raw` vers `qcow2` pour activer des fonctionnalités ou compacter l'allocation).
- **Approche** : Exécution de `qemu-img convert -f raw -O qcow2` et contrôle avec `qemu-img info` (`196 KiB` obtenus).
- **Bénéfice** : Transition maîtrisée sans réinstaller le système d'exploitation.

### Cas 7 : Traçabilité et audit d'intégrité des images et templates
- **Contexte** : Garantir qu'une image OS (`image-os-v1`) ou un template (`template-v1`) n'a pas été altéré entre deux étapes ou deux partages.
- **Approche** : Archivage des configurations XML (`source.xml`, `capture-config.xml`), des sorties de commandes et des empreintes `SHA-256` (`capture.sha256`, `template.sha256`).
- **Bénéfice** : Meilleure traçabilité, vérification d'intégrité et facilité de revue technique.

### Cas 8 : Préparation d'environnements multi-nœuds pour l'ingénierie des données et le cloud
- **Contexte** : Mettre en place des machines virtuelles isolées pour expérimenter des services réseau, des conteneurs ou des architectures distribuées en local.
- **Approche** : Combiner `libvirt` / `KVM`, `cloud-init` et des disques `QCOW2` pour instancier rapidement des nœuds reproductibles.
- **Bénéfice** : Environnement local flexible, économique en ressources et proche des pratiques d'infrastructure cloud.

## 2. Conclusion personnelle

Ce TP m'a permis de passer d'une approche théorique de la virtualisation à une maîtrise concrète des mécanismes d'instanciation et de stockage sous KVM/QEMU et `libvirt`. J'ai particulièrement apprécié de constater l'importance des détails d'identité (`machine-id`, `hostname`, empreintes SSH lors de la réattribution d'une IP DHCP) et la différence concrète entre la taille apparente (`ls -lh`) et l'occupation réelle (`du -sh`) des formats `RAW` et `QCOW2`.

Structurer ce travail sous forme de preuves, de fiches techniques et de tableaux comparatifs dans Git m'a également aidée à adopter une démarche rigoureuse et reproductible, très utile pour mes projets en Big Data, ingénierie des données et cloud computing.