# Scénario 3 - BGP inter-domaine, filtrage et PBR (Difficile)

## Ce qui se fait ici

DataLogix se connecte à un FAI (**AS 65500**) en **eBGP** et dispose de deux liens WAN vers son centre de données :
Lien 1 (10.0.13.0/30, fibre) et Lien 2 (10.0.24.0/30, secours).
Trois choses à réaliser :

1. Une session eBGP qui n'annonce que les préfixes publics de l'entreprise (prefix-list + route-map).
2. Du **Policy-Based Routing** pour forcer le trafic web (ports 80 et 443) à passer par le Lien 2.
3. Du **route tagging** (par exemple tag 100 pour EIGRP) pour empêcher les boucles de redistribution.

## Résultat à obtenir

- `show ip bgp summary` montre la session établie.
- Seuls les préfixes autorisés sont annoncés au FAI.
- Un traceroute du trafic web prouve le passage par le Lien 2.
- Pas de boucle de routage, et la bascule fonctionne en cas de panne.

## Pièges à éviter

- Une route-map se termine par un `deny` implicite : prévoir une clause `permit` finale.
- `ip policy route-map` s'applique sur l'interface d'**entrée** du trafic, pas de sortie.

## Contenu du dossier

- `configs/` : une configuration `.txt` par routeur.
