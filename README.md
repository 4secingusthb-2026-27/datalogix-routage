# DataLogix - Atelier d'Ingénierie en Routage Avancé

Dépôt de travail commun de l'équipe pour l'atelier de routage (niveau CCNP).

## 1. Le problème

L'entreprise fictive **DataLogix** grandit et son réseau doit évoluer en trois étapes :

1. Un petit réseau de siège en triangle (3 routeurs) à adresser et à faire communiquer.
2. Une filiale rachetée (InnoTech) qui utilise un autre protocole (EIGRP) que le siège (OSPF).
3. Une connexion à un fournisseur d'accès Internet (eBGP) et un trafic web qui prend un mauvais chemin.

## 2. L'objectif

Prouver que l'équipe sait **concevoir, configurer et valider** un réseau avec les routeurs Cisco :
adressage VLSM, OSPF, EIGRP, redistribution, BGP, filtrage par route-map et Policy-Based Routing.
L'équipe travaille avec 4 rôles : Chef de projet, Architecte, Ingénieur Configuration, Ingénieur Test/QA.

## 3. La solution demandée

| Scénario | Problème | Solution |
|---|---|---|
| 1 - Facile | 3 routeurs en triangle, 3 LAN | VLSM sur 192.168.10.0/24 + OSPF mono-zone |
| 2 - Intermédiaire | Siège OSPF et filiale EIGRP isolés | Area 1 + résumé de routes + redistribution bidirectionnelle |
| 3 - Difficile | Accès FAI et trafic web mal routé | eBGP (AS 65500) + prefix-list/route-map + PBR ports 80/443 |

## 4. Résultats attendus

- Tous les LAN se joignent de bout en bout (ping sans perte).
- Les tables de routage sont propres et les voisins OSPF/EIGRP sont établis.
- La session eBGP est active et seuls les préfixes publics de l'entreprise sont annoncés.
- Le trafic 80/443 passe par le Lien 2 (preuve par traceroute).
- La bascule fonctionne quand l'évaluateur coupe un lien.

## 5. Structure du dépôt

```
datalogix-routage/
  README.md                 ce fichier
  todo.md                   étapes de mise en place et d'implémentation
  docs/                     schémas, plan VLSM, décisions d'architecture
  topologies/               fichiers .pkt (Packet Tracer) ou labs GNS3/EVE-NG
  scenario1-ospf-vlsm/      scénario 1 et ses configs
  scenario2-redistribution/ scénario 2 et ses configs
  scenario3-bgp-pbr/        scénario 3 et ses configs
  captures/                 preuves de test (images)
  rapport/                  rapport PDF final
```

## 6. Règle d'or

Aucun script n'est injecté sur un routeur sans validation de l'Architecte et du Coordinateur.
Une configuration validée est d'abord poussée sur Git, puis collée dans le simulateur.
