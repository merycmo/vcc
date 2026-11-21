# Comparaison des formats de disques virtuels : RAW vs QCOW2

Ce document synthétise les résultats expérimentaux obtenus lors de l'Exercice 11 sous KVM/QEMU (`qemu-img`, `ls -lh`, `du -sh` et `qemu-img convert`) sur des images disques de capacité virtuelle `5 GiB` (`5368709120 bytes`)[cite: 5, 9].

## 1. Résultats expérimentaux (`qemu-img`, `ls -lh`, `du -sh`)

### Création et inspection (`qemu-img create` & `qemu-img info`)
- **`disque_test.raw`** :
  - Format : `raw`[cite: 5]
  - Taille virtuelle (`virtual size`) : `5 GiB (5368709120 bytes)`[cite: 5]
  - Taille réelle allouée (`disk size`) : `4 KiB`[cite: 5]
- **`disque_test.qcow2`** :
  - Format : `qcow2` (`compat: 1.1`, `cluster_size: 65536`, `compression_type: zlib`, `lazy_refcounts: false`, `refcount_bits: 16`)[cite: 5]
  - Taille virtuelle (`virtual size`) : `5 GiB (5368709120 bytes)`[cite: 5]
  - Taille réelle allouée (`disk size`) : `196 KiB` (`file length: 192 KiB (197120 bytes)`)[cite: 5]
- **`disque_converti.qcow2`** (généré via `qemu-img convert -f raw -O qcow2 disque_test.raw disque_converti.qcow2`)[cite: 5] :
  - Format : `qcow2` (`compat: 1.1`, `cluster_size: 65536`)[cite: 5]
  - Taille virtuelle (`virtual size`) : `5 GiB (5368709120 bytes)`[cite: 5]
  - Taille réelle allouée (`disk size`) : `196 KiB`[cite: 5]

### Taille apparente vs occupation physique (`ls -lh` vs `du -sh`)
| Fichier | Taille apparente (`ls -lh`) | Occupation réelle (`du -sh`) | Analyse |
| :--- | :--- | :--- | :--- |
| `disque_test.raw` | `5.0G`[cite: 5] | `4.0K`[cite: 5] | Fichier creux (*sparse file*) : `5 GiB` déclarés, mais seuls `4 KiB` de blocs physiques sont alloués[cite: 5]. |
| `disque_test.qcow2` | `193K`[cite: 5] | `196K`[cite: 5] | Allocation dynamique (*thin provisioning*) : l'en-tête et les structures de métadonnées occupent ~`196 KiB` au départ[cite: 5]. |
| `disque_converti.qcow2` | — | `196 KiB`[cite: 5] | La conversion ignore les blocs non alloués du fichier RAW et produit une image QCOW2 propre de `196 KiB`[cite: 5]. |

## 2. Analyse comparative

| Critère | RAW | QCOW2 |
| :--- | :--- | :--- |
| **Occupation disque initiale** | Très faible (`4 KiB`) grâce au *sparse file* sur le système de fichiers hôte[cite: 5]. | Faible (`196 KiB`) incluant l'en-tête et les tables de métadonnées (`cluster_size: 65536`)[cite: 5]. |
| **Performances E/S** | Optimales (accès direct sans couche de métadonnées supplémentaire). | Légèrement inférieures en raison de la gestion des clusters et tables d'allocation. |
| **Fonctionnalités avancées** | Aucune (pas de *snapshots* internes, compression ou *backing files*). | *Snapshots* natifs, allocation fine, *backing files* (clonage léger) et options de compression/chiffrement. |
| **Choix recommandé** | Charges sensibles aux performances E/S maximales ou stockage géré par d'autres couches (LVM/SAN). | Environnements de développement/lab, clonage rapide via *backing files* et *snapshots* de machines virtuelles. |