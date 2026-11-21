# Comparatif d'identités — Source / Template vs Instance clonée

Ce tableau compare les attributs d'identité entre l'image/template source et une instance déployée via `cloud-init` sous KVM/QEMU (`libvirt` / `qemu:///system`). Complétez les champs entre crochets `[...]` si vous disposez des valeurs exactes de la source.

| Critère d'identité | Source / Template (`ubuntu-template-v1`) | Instance clonée (`app01` / `app02`) | Impact et vérification |
| :--- | :--- | :--- | :--- |
| **Hostname (`hostnamectl --static`)** | `[hostname de la source / template]` | `app01` (`eac5d175ba3865bfc00c1a47316e13ca`)[cite: 2, 3, 5] / `app02` `[à compléter]`[cite: 1] | Doit être unique sur le réseau pour éviter les conflits d'identification. |
| **`/etc/machine-id`** | `[machine-id d'origine ou réinitialisé]` | `eac5d175ba3865bfc00c1a47316e13ca` sur `app01`[cite: 2, 3, 5] | Identifiant machine systemd unique par instance. |
| **Marqueur `/etc/tp-instance`** | `[absent ou valeur de référence]` | `Instance : app01` / `Reference: ubuntu-template-v1`[cite: 5] | Confirme l'application de la personnalisation `cloud-init` (`user-data` / `meta-data`). |
| **Adresse MAC (`vnet` / interface)** | `[MAC de la VM/template source]` | `[MAC attribuée à l'instance clonée]` | Distincte sur l'interface virtuelle `libvirt` pour le bail DHCP. |
| **Adresse IP (`enp1s0`)** | `[IP source ou DHCP]` | `192.168.122.254` (DHCP `192.168.122.0/24`)[cite: 2, 3, 5, 6] | Attribuée dynamiquement ; la réutilisation d'une IP entraîne un changement d'empreinte SSH[cite: 2, 4, 5]. |
| **Clés / Empreintes SSH** | `[empreinte SSH de la source]` | Clé autorisée `~/.ssh/tpl_kvm` (`etudiant`) ; nouvelle empreinte hôte générée au démarrage[cite: 1, 2, 3, 4, 5, 6] | Nécessite `ssh-keygen -R <IP>` côté client si la même IP est reprise par une autre VM[cite: 2, 4, 5]. |

## Points clés observés
- Le déploiement via `cloud-init` permet d'instancier des machines à partir d'un modèle commun tout en attribuant une identité propre (`hostname`, `/etc/machine-id`, `/etc/tp-instance`)[cite: 1, 2, 5].
- Lorsqu'une adresse IP est réattribuée sur le réseau virtuel (`192.168.122.254`), le client SSH signale un changement d'identité d'hôte (`WARNING: REMOTE HOST IDENTIFICATION HAS CHANGED!`)[cite: 2, 4, 5].