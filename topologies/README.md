# topologies - Fichiers du simulateur

## Ce qui se fait ici

On stocke les fichiers source du simulateur pour reproduire le réseau immédiatement :

- `scenario1.pkt`, `scenario2.pkt`, `scenario3.pkt` pour Cisco Packet Tracer ;
- ou une archive de lab pour GNS3 / EVE-NG.

## Attention

Les fichiers `.pkt` sont **binaires** : Git ne sait pas les fusionner.
Un seul membre modifie un fichier à la fois. Prévenir l'équipe avant de pousser une nouvelle version.
Convention de nommage : `scenarioX_vN.pkt` (par exemple `scenario1_v2.pkt`).
