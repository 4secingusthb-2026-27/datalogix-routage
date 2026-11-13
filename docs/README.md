# docs - Conception et documentation

## Ce qui se fait ici

C'est le dossier de l'**Architecte réseau**. On y dépose tout ce qui est produit **avant** de configurer un routeur :

- le plan d'adressage VLSM de chaque scénario (sous-réseau, masque, plage utile, broadcast) ;
- les schémas topologiques (niveau logique et physique), exportés depuis draw.io en PNG ;
- la justification du choix des protocoles (pourquoi OSPF, pourquoi redistribution, etc.) ;
- la matrice de filtrage BGP et la matrice PBR du scénario 3 ;
- la fiche d'attribution des 4 rôles de l'équipe.

## Fichiers conseillés

- `roles.md`
- `vlsm-scenario1.md`
- `schema-scenario1.png`, `schema-scenario2.png`, `schema-scenario3.png`
- `decisions-protocoles.md`
