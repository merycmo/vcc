# Comparatif des instances — `app01` vs `app02`

Ce tableau met en évidence que `app01` et `app02` constituent bien deux instances distinctes déployées via `cloud-init`, malgré l'utilisation du même modèle (`ubuntu-template-v1`), de la même clé SSH (`~/.ssh/tpl_kvm`) et de la réutilisation de l'adresse IP `192.168.122.254` sur le réseau virtuel[cite: 1, 2, 4, 5, 8]. Complétez les champs entre crochets `[...]` pour `app02` avec vos relevés finaux.

| Critère de comparaison | Instance `app01` | Instance `app02` | Preuve / Observation |
| :--- | :--- | :--- | :--- |
| **Hostname (`hostnamectl --static`)** | `app01`[cite: 2, 5] | `app02`[cite: 1] | Noms d'hôtes distincts configurés via `cloud-init`[cite: 1, 2, 5]. |
| **`/etc/machine-id`** | `eac5d175ba3865bfc00c1a47316e13ca`[cite: 2, 5] | `[machine-id de app02 à compléter]` | Identifiant machine unique par VM. |
| **Contenu de `/etc/tp-instance`** | `Instance : app01`<br>`Reference: ubuntu-template-v1`[cite: 5] | `Instance : app02`<br>`Reference: ubuntu-template-v1` `[à vérifier]`[cite: 5] | Confirme la traçabilité de l'instance et de la version du template[cite: 5]. |
| **Système d'exploitation** | Ubuntu 24.04.5 LTS (Linux 6.8.0-147-generic, `x86_64`)[cite: 2, 5] | Ubuntu 24.04.5 LTS (Linux 6.8.0-147-generic, `x86_64`)[cite: 2, 5] | Base système identique issue du template[cite: 5]. |
| **Adresse IP (`enp1s0`)** | `192.168.122.254`[cite: 2, 5] | `192.168.122.254`[cite: 1, 4] | Même IP attribuée successivement par DHCP ; provoque un changement d'empreinte SSH (`ssh-keygen -R`)[cite: 1, 2, 4, 5]. |
| **Authentification SSH** | Clé `~/.ssh/tpl_kvm` (`etudiant`)[cite: 2, 5] | Clé `~/.ssh/tpl_kvm` (`etudiant`)[cite: 1, 4] | Accès fonctionnel par clé sans mot de passe une fois l'hôte prêt[cite: 1, 2, 4, 5]. |

## Conclusion de la validation
Malgré un environnement réseau partagé (`192.168.122.254`) et une base commune (`ubuntu-template-v1`), `app01` et `app02` disposent d'identités propres (`hostname`, `machine-id`, `/etc/tp-instance`) et d'empreintes SSH distinctes[cite: 1, 2, 4, 5].