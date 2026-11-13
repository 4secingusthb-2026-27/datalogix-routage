# Scénario 2 - Hiérarchie OSPF, agrégation et redistribution (Intermédiaire)

## Ce qui se fait ici

DataLogix rachète la filiale InnoTech qui utilise **EIGRP AS 100**, alors que le campus utilise **OSPF**.
Les deux réseaux ne se comprennent pas : on les relie avec une **redistribution contrôlée**
et on allège OSPF avec une zone dédiée (Area 1) et un **résumé de routes**.

## Rôles des routeurs

- **ABR** : routeur entre l'Area 0 et l'Area 1, c'est lui qui résume les routes.
- **ASBR** : R1-CORE, routeur entre OSPF et EIGRP, c'est lui qui redistribue.
- **R4-DIST** : passerelle de la filiale, côté EIGRP.

## Résultat à obtenir

- OSPF multi-zones fonctionnel (Area 0 et Area 1).
- Un seul préfixe résumé de l'Area 1 visible dans l'Area 0.
- Routes EIGRP visibles dans OSPF (`O E2`) et routes OSPF visibles dans EIGRP (`D EX`).
- Ping de bout en bout entre le campus et InnoTech.

## Pièges à éviter

- Oublier le mot-clé `subnets` dans `redistribute eigrp ... subnets`.
- Oublier les 5 composantes de métrique dans `redistribute ospf ... metric bw delay rel load mtu`.

## Contenu du dossier

- `configs/` : une configuration `.txt` par routeur.
