# Scénario 1 - Routage dynamique intra-domaine et VLSM (Facile)

## Ce qui se fait ici

Trois routeurs (R1-Siège, R2-Bureaux, R3-Serveurs) sont reliés en triangle.
Il faut adresser trois LAN avec VLSM puis faire apprendre les routes automatiquement avec **OSPFv2 Area 0**.

## Besoins

| Réseau | Hôtes |
|---|---|
| LAN 1 | 120 |
| LAN 2 | 50 |
| LAN 3 | 25 |
| 3 liaisons point à point | /30 chacune |

Bloc de départ : `192.168.10.0/24`.

## Résultat à obtenir

- Les 3 voisins OSPF sont établis.
- Les 3 LAN se joignent entre eux.
- Si un lien du triangle est coupé, le trafic passe par l'autre chemin.
- Aucun paquet Hello OSPF ne sort vers les PC (passive-interface).

## Contenu du dossier

- `configs/R1.txt`, `configs/R2.txt`, `configs/R3.txt` : running-config commentées.

## Commandes de vérification

```
show ip ospf neighbor
show ip route ospf
ping <ip d'un autre LAN>
```
