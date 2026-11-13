# TODO - Mise en place et implémentation

Ce fichier guide l'équipe : d'abord l'environnement local, ensuite l'implémentation scénario par scénario.
Cocher les cases au fur et à mesure.

## Partie A - Configurer son environnement local (chacun)

- [ ] Installer **Git** : https://git-scm.com/download/win
- [ ] Installer **VS Code** ou Notepad++ pour écrire les configs.
- [ ] Installer **Cisco Packet Tracer** (compte gratuit Cisco Networking Academy requis).
- [ ] Ouvrir **draw.io** dans le navigateur (https://app.diagrams.net) pour les schémas.
- [ ] Cloner le dépôt de l'organisation :

```
git clone https://github.com/<ORG>/datalogix-routage.git
cd datalogix-routage
git config user.name "Ton Nom"
git config user.email "ton@email.com"
```

- [ ] Créer sa branche de travail avant toute modification :

```
git checkout -b scenario1-prenom
```

- [ ] Vérifier que Packet Tracer s'ouvre et qu'on peut placer un routeur 2911 et un switch 2960.

### Workflow Git de l'équipe

1. `git pull` avant de commencer.
2. Travailler dans son dossier de scénario.
3. `git add .` puis `git commit -m "scenario1: config R1"` puis `git push`.
4. Ouvrir une Pull Request, validée par l'Architecte et le Coordinateur.
5. Les fichiers `.pkt` ne se fusionnent pas : un seul membre les modifie à la fois.

## Partie B - Organisation de l'équipe (15 min, Chef de projet)

- [ ] Attribuer les 4 rôles et les noter dans `docs/roles.md`.
- [ ] À 3 membres : fusionner les rôles 1 et 4, ou 2 et 3.
- [ ] Faire un stand-up de 3 minutes au début de chaque scénario.

## Partie C - Scénario 1 : VLSM et OSPF

### Architecte
- [ ] Calculer le VLSM à partir de 192.168.10.0/24 pour LAN 1 (120 hôtes), LAN 2 (50), LAN 3 (25) et les 3 liaisons /30.
- [ ] Dessiner le triangle R1-R2-R3 avec interfaces et IP, exporter en PNG dans `docs/`.
- [ ] Écrire la justification : pourquoi OSPF plutôt que du routage statique en boucle fermée.

### Ingénieur Configuration
- [ ] Dans Packet Tracer, placer 3 routeurs, des switches et des PC, puis câbler le triangle.
- [ ] Configurer les IP des interfaces et les activer avec `no shutdown`.
- [ ] Activer OSPF :

```
router ospf 1
 network <reseau> <wildcard> area 0
 passive-interface <interface vers le LAN>
```

- [ ] Sauvegarder chaque config dans `scenario1-ospf-vlsm/configs/R1.txt`, `R2.txt`, `R3.txt`.

### Ingénieur Test / QA
- [ ] Vérifier `show ip ospf neighbor` et `show ip route`.
- [ ] Pinger chaque LAN depuis chaque autre LAN.
- [ ] Couper un lien du triangle et montrer que le ping continue.
- [ ] Sauvegarder les captures dans `captures/`.

## Partie D - Scénario 2 : zones, résumé et redistribution

- [ ] Architecte : définir l'ABR, l'ASBR (R1-CORE) et le résumé de l'Area 1.
- [ ] Config : relier R1-CORE à R4-DIST, configurer OSPF Area 0 et Area 1, EIGRP AS 100 côté filiale.
- [ ] Config : résumé sur l'ABR avec `area 1 range <prefixe> <masque>`.
- [ ] Config : redistribution sur l'ASBR :

```
router ospf 1
 redistribute eigrp 100 subnets
router eigrp 100
 redistribute ospf 1 metric 10000 100 255 1 1500
```

- [ ] QA : vérifier les routes `O E2` et `D EX`, puis un ping campus vers InnoTech.
- [ ] Sauvegarder configs et captures.

## Partie E - Scénario 3 : BGP, filtrage et PBR

- [ ] Architecte : fixer les AS, les IP des voisins, les préfixes à annoncer, la matrice de filtrage et la matrice PBR.
- [ ] Config : session eBGP avec l'AS 65500 (`neighbor <ip> remote-as 65500`).
- [ ] Config : prefix-list et route-map en sortie pour n'annoncer que les préfixes publics.
- [ ] Config : route-maps de redistribution avec tag (par exemple tag 100) pour éviter les boucles.
- [ ] Config : ACL étendue ports 80 et 443, route-map avec `set ip next-hop` vers le Lien 2, puis `ip policy route-map` sur l'interface d'ENTRÉE.
- [ ] Config : ajouter une clause `permit` finale dans chaque route-map.
- [ ] QA : `show ip bgp summary`, `show route-map`, traceroute via le Lien 2, test de panne.

## Partie F - Livrables finaux

- [ ] Rapport PDF dans `rapport/` (rôles, schémas, tableaux VLSM, captures, annexes de configs).
- [ ] Un `.txt` commenté par routeur dans chaque dossier `configs/`.
- [ ] Fichiers `.pkt` finaux dans `topologies/`.
- [ ] Archive zip unique prête à remettre.
- [ ] Soutenance : 5 min de présentation (choix et difficultés) et 5 min de démo avec panne injectée.
- [ ] Répétition de la démo : tout couper et remonter sans surprise.

## Si le temps manque

1. Priorité aux scénarios 1 et 2 complets avec captures.
2. Si le PBR bloque, montrer eBGP et prefix-list et expliquer le PBR à l'oral.
3. Un rapport sobre avec schémas, tableaux et captures suffit.
