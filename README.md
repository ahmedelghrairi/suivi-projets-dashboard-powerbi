# Pilotage d'un portefeuille de projets — tableau de bord Power BI

Sanitoral, fabricant international de soins bucco-dentaires, pilote 104 projets IT et Marketing dans le monde sans disposer d'une vue d'ensemble fiable. Sophie, cheffe de projet au PMO, a besoin d'un tableau de bord pour suivre l'avancement, repérer les retards, et donner à trois niveaux de direction (général, régional, pays) une lecture adaptée à leur périmètre, sans multiplier les rapports.

Étude de cas complète, avec démarche et recommandations : [voir sur mon portfolio](https://ahmedelghrairi.github.io/projets/suivi-projets.html)

## Contenu du dépôt

- `dashboard_suivi_projets.pbix` : le tableau de bord complet, à ouvrir avec Power BI Desktop. Contient aussi, dans ses propres onglets, le Product Strategy Canvas, les étapes de préparation des données et le modèle relationnel documenté, comme demandé par la mission.
- `donnees_sanitoral.xlsx`, `dictionnaire_donnees.xlsx` : les données sources et leur documentation
- `captures/` : le Product Strategy Canvas, les étapes de nettoyage Power Query, le modèle relationnel, et les quatre pages principales du dashboard, pour consulter le résultat sans ouvrir Power BI

## Les données

Sept tables extraites du logiciel de gestion de projets de Sanitoral, couvrant 104 projets menés entre 2018 et 2022 dans 52 pays : le planning prévisionnel (coût, durée, livrables par phase), les réalisations effectives sur ces trois mêmes axes, le type de chaque projet (IT ou Marketing), et la localisation géographique avec région et statut du pays. Les projets IT suivent 6 phases (A à F), les projets Marketing 4 phases (1 à 4), deux cycles indépendants qui ne se succèdent jamais.

## La démarche

**Product Strategy Canvas avant tableau de bord.** Neuf user stories réparties sur les trois profils de directeurs, formalisées et validées par la cheffe de projet avant toute construction dans Power BI, pour ne pas se lancer dans l'outil avant d'avoir vérifié le besoin.

**Nettoyage entièrement dans Power Query.** Sophie a demandé une mise à jour hebdomadaire des données : le nettoyage devait donc être automatisable, pas fait à la main. Chaque table passe par les mêmes étapes rejouables, promotion des en-têtes, typage, suppression des lignes vides. Une colonne calculée `Id_Phase`, concaténant l'identifiant du projet et le nom de la phase, sert de clé unique pour relier les tables de réalisations au planning : sans elle, impossible de savoir si "Phase 1" appartient au projet 12 ou au projet 87.

**Modèle en étoile autour de la table de planning.** `Projects_plans` centralise les mesures DAX d'écart et d'alerte, reliée aux trois tables de réalisation sur `Id_Phase`, et à la géographie et au type de projet sur l'identifiant projet. Le seuil d'alerte, 15 % d'écart entre prévu et réalisé sur un indicateur, est une mesure DAX unique réutilisée partout, pas recalculée page par page.

**Trois rôles, un seul rapport.** Plutôt que dupliquer les pages pour chaque niveau de direction, des rôles de sécurité filtrent les mêmes visuels selon qui les consulte : le directeur général voit tout, un directeur régional ne voit que sa région, un directeur pays que son pays. Un seul tableau de bord à maintenir, une lecture différente pour chacun.

## Quelques résultats

- Sur 104 projets et 520 phases, 79,8 % des projets accusent un retard, 74 % dépassent leur budget, et 45,2 % sont en alerte sur leurs livrables.
- L'écart de coût le plus frappant n'est pas régional mais lié au type de projet : 100 % des projets IT (52 sur 52) sont en alerte coût, contre 48 % des projets Marketing (25 sur 52).
- Le coût total dérive de 7,3 % sur l'ensemble du portefeuille (56,1 M€ prévus contre 60,2 M€ réels), un chiffre modéré qui masque des écarts extrêmes projet par projet, certaines phases dépassant 400 % de leur budget.
- Les livrables sont globalement sous-produits par rapport au plan (-10,5 %), avec l'Europe de l'Ouest la plus touchée (-14,2 %) et l'Amérique du Nord et latine la mieux tenue (-4,6 %).
- Axe stratégique retenu : adapter le pilotage selon le type de projet plutôt que d'appliquer un contrôle uniforme, puisque le risque budgétaire ne se répartit pas du tout de la même façon entre IT et Marketing.

## Outils

Power BI Desktop, Power Query, DAX.

Ahmed El Ghrairi, 2026.
