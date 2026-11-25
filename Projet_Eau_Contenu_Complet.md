# Projet DataViz — « Que contient l'eau de votre robinet ? »

> Dossier de référence complet du projet : sujet, données, méthode, formules Tableau, feuilles, tableaux de bord, résultats chiffrés, messages clés, limites et préparation de l'oral.
> À utiliser comme source unique pour la présentation PowerPoint, le poster et l'oral.

**Lien Tableau Public (à mettre dans la présentation et sur le poster) :**
https://public.tableau.com/app/profile/mohammed.benakka.ouayad/viz/Qualitdeleaudurobinet/VueFrance

(Le lien qui se termine par `/Macommune` ouvre directement le tableau de bord citoyen.)

**Groupe :** Mohammed BENAKKA OUAYAD, Hajar BITARI, Aya SINDEL — **Cours :** _[à compléter]_ — **Enseignant :** _[à compléter]_ — **Date :** _[à compléter]_

---

## Sommaire

1. Le sujet et la problématique
2. Les utilisateurs visés
3. Les données
4. Le traitement des données
5. La méthode : le score de priorité
6. Le classeur Tableau : les champs calculés
7. Le classeur Tableau : les 11 feuilles
8. Le tableau de bord « Vue France »
9. Le tableau de bord « Ma commune »
10. Les résultats chiffrés
11. Les messages clés et les recommandations
12. Les limites et les points de vigilance
13. Ce que le projet démontre (critères du cours)
14. Préparation de l'oral : le fil rouge et les questions probables
15. Glossaire
16. Checklist finale

---

## 1. Le sujet et la problématique

**Sujet :** la qualité de l'eau potable distribuée au robinet en France, de 2021 à 2026.

**Problématique :**
> Où l'eau du robinet dépasse-t-elle les limites sanitaires, et quels départements une Agence régionale de santé (ARS) doit-elle traiter en priorité ?

**Pourquoi ce sujet ?**
- Il concerne tout le monde : 100 % de la population boit l'eau du réseau public ou s'en sert.
- Il est d'actualité : les pesticides et leurs métabolites, et les PFAS (« polluants éternels »), sont régulièrement dans l'actualité.
- Les données sont officielles et publiques (ministère de la Santé, contrôle sanitaire des ARS).
- Il se prête à toutes les exigences du cours : cartes, mesures complexes, tableaux de bord, publication et aide à la décision.

**La question métier en une phrase :** si une ARS a un budget limité, où doit-elle intervenir en premier ?

---

## 2. Les utilisateurs visés

| Utilisateur | Son besoin | Ce qu'on lui donne |
|---|---|---|
| **Le décideur** (ARS, préfecture, ministère) | Vue d'ensemble, comparer les territoires, prioriser les actions | Tableau de bord **« Vue France »** : chiffres clés, carte, Top 10, tendances, carte de chaleur |
| **Le citoyen** | Savoir si l'eau de **sa** commune pose problème | Tableau de bord **« Ma commune »** : on tape sa commune, on voit ses résultats |

---

## 3. Les données

### 3.1 La source

- **Producteur :** ministère de la Santé, à partir des résultats du **contrôle sanitaire des eaux destinées à la consommation humaine**, réalisé par les ARS.
- **Diffusion :** data.gouv.fr (jeux de données « Résultats du contrôle sanitaire de l'eau distribuée commune par commune »).
- **Période :** 2021 à 2026.
- **Volume brut traité :** environ **1,6 million de prélèvements** et **plusieurs millions d'analyses** (un prélèvement = un échantillon d'eau ; on y mesure de nombreux paramètres, et chaque mesure est une analyse).

### 3.2 Les fichiers préparés pour Tableau

Les données brutes ont été transformées en **6 fichiers CSV** (dossier `projet-eau/donnees_tableau/`). Chacun regarde la même réalité à un niveau de détail différent.

| Fichier | Niveau (une ligne =) | Colonnes principales | Utilisé pour |
|---|---|---|---|
| `1_communes…` (version complète) | commune × famille × année | idem 1b | archive, non utilisé dans le classeur |
| `1b_communes_2025_leger.csv` | commune × famille de polluant (2025) | code_insee, commune, code_dept, departement, region, famille, population, nb_prelevements, nb_prelevements_depassement, ratio_max_limite, commune_touchee (0/1), latitude, longitude | Carte communes, Fiche commune, KPI population exposée |
| `2_departements_synthese.csv` | département × famille × année | code_dept, departement, region, famille, annee, nb_communes_controlees, nb_communes_touchees, nb_prelevements_communes, nb_prelevements_depassement_communes, population_controlee, population_exposee | Évolution par famille, Carte de chaleur |
| `3_prelevements_conformite.csv` | département × mois | code_dept, departement, region, annee, mois, date_mois, nb_prelevements, nb_non_conformes, nb_non_conformes_bacterio, nb_non_conformes_chimique | Évolution conformité, KPI prélèvements, KPI conformité |
| `4_substances.csv` | substance × année | substance, code_parametre, famille, limite_qualite, annee, nb_analyses, nb_depassements | Feuille Substances |
| `5_score_priorite_departements.csv` | département × année | code_dept, departement, region, annee, population_dept, risque_bacteries_pct, risque_nitrates_pct, risque_pesticides_pct, risque_pfas_pct, risque_autres_pct, couverture_pfas_pct, score_priorite, rang, latitude, longitude | Carte départements, Top 10 |

### 3.3 Les 5 familles de polluants

| Famille | Exemples | Pourquoi c'est un enjeu |
|---|---|---|
| **Bactéries** | *Escherichia coli*, entérocoques | Risque **immédiat** : gastro-entérites. Signale une contamination fécale. |
| **Nitrates** | nitrates (NO₃), limite 50 mg/L | Engrais agricoles ; risque surtout pour les nourrissons et les femmes enceintes. |
| **Pesticides** | chloridazone desphényl, chlorothalonil R417888, atrazine déséthyl | Résidus agricoles (molécules mères et **métabolites**) ; exposition chronique. |
| **PFAS** | « polluants éternels » (perfluorés) | Très persistants ; nouvelle limite européenne (directive 2020/2184) en vigueur depuis janvier 2026 ; mesurés en masse seulement depuis 2025. |
| **Autres substances** | turbidité, chlorure de vinyle monomère, métaux… | Paramètres physico-chimiques divers. |

### 3.4 Vocabulaire indispensable

- **Prélèvement :** un échantillon d'eau prélevé à un point du réseau.
- **Analyse :** la mesure d'un paramètre dans un prélèvement.
- **Limite de qualité :** valeur réglementaire à ne pas dépasser.
- **Dépassement / non-conformité :** une valeur au-dessus de la limite.
- **Commune touchée :** commune avec au moins un dépassement pour une famille donnée sur l'année.
- **Population exposée :** population des communes touchées.

---

## 4. Le traitement des données (préparation avant Tableau)

1. **Téléchargement** des fichiers officiels du contrôle sanitaire, année par année (2021–2026).
2. **Nettoyage :** codes communes et départements gardés en **texte** (pour conserver le « 01 », le « 2A »…), dates harmonisées, doublons retirés.
3. **Classement des paramètres** analysés dans les 5 familles.
4. **Calcul des dépassements** : comparaison de chaque résultat à sa limite de qualité.
5. **Agrégation** à plusieurs niveaux (commune, département, mois, substance) pour produire les 6 CSV.
6. **Calcul du score de priorité** par département (voir partie 5).
7. **Géocodage :** latitude et longitude des communes et des départements, pour les cartes.

**Pourquoi plusieurs fichiers au lieu d'un seul ?** Un fichier unique de plusieurs millions de lignes serait trop lourd pour Tableau Public. Les fichiers agrégés sont légers, et chaque feuille du classeur utilise **une seule source** : pas besoin de jointures.

---

## 5. La méthode : le score de priorité

Le **score de priorité** classe les départements du plus urgent au moins urgent. C'est une **moyenne pondérée** des risques par famille :

```
score = 0,30 × risque bactéries
      + 0,20 × risque nitrates
      + 0,20 × risque pesticides
      + 0,20 × risque PFAS
      + 0,10 × risque autres
```

- Chaque **risque** est un pourcentage (part des prélèvements ou de la population touchés pour cette famille dans le département).
- **Pourquoi 30 % pour les bactéries ?** Ce sont les seules qui rendent malade **tout de suite** ; une contamination bactérienne demande une action immédiate.
- **Pourquoi seulement 10 % pour « autres » ?** Groupe très hétérogène, de la turbidité à des métaux comme le plomb et l'arsenic. C'est un choix discutable.
- **Pourquoi ces poids sont discutables ?** C'est un **choix de décideur**, pas une vérité scientifique. Amélioration possible : un **paramètre** Tableau pour changer les poids (à confirmer dans le classeur).

**Le rang** : 1 = département le plus prioritaire.

---

## 6. Le classeur Tableau : les champs calculés

Les fichiers donnent surtout des **comptes**. Un compte seul ne permet pas de comparer (un grand département a forcément plus de dépassements), donc on calcule des **taux**.

### Source `2_departements_synthese`

| Champ | Formule | Rôle |
|---|---|---|
| **Taux de dépassement** | `SUM([nb_prelevements_depassement_communes]) / SUM([nb_prelevements_communes])` | Part des prélèvements en dépassement, par famille / région / année |
| **Part de la population exposée** | `SUM([population_exposee]) / SUM([population_controlee])` | Part de la population concernée |

### Source `3_prelevements_conformite`

| Champ | Formule | Rôle |
|---|---|---|
| **Taux de non-conformité** | `SUM([nb_non_conformes]) / SUM([nb_prelevements])` | Indicateur global de qualité |
| **Taux de conformité** | `1 - [Taux de non-conformité]` | Le KPI « 95,49 % » |
| **Variation sur un an** | `(SUM([nb_non_conformes]) / SUM([nb_prelevements])) - LOOKUP(SUM([nb_non_conformes]) / SUM([nb_prelevements]), -1)` | **Calcul de table** : différence avec l'année précédente (amélioration ou dégradation) |

### Source `5_score_priorite_departements`

| Champ | Formule | Rôle |
|---|---|---|
| **Score recalculé** | `0.30*ZN([risque_bacteries_pct]) + 0.20*ZN([risque_nitrates_pct]) + 0.20*ZN([risque_pesticides_pct]) + 0.20*ZN([risque_pfas_pct]) + 0.10*ZN([risque_autres_pct])` | Montre que le score est calculable dans Tableau. `ZN` remplace les vides par 0. |
| **Rang du département** | `RANK(SUM([score_priorite]))` | **Calcul de table** : classement |
| **Population de la région** | `{ FIXED [region], [annee] : SUM([population_dept]) }` | **Expression LOD FIXED** : total régional quel que soit le niveau d'affichage |

### Source `1b_communes_2025_leger`

| Champ | Formule | Rôle |
|---|---|---|
| **Population de la commune** | `{ FIXED [code_insee] : MAX([population]) }` | **LOD FIXED** : chaque commune apparaît 5 fois (une ligne par famille) ; on prend sa population **une seule fois** |
| **Population exposée** | `IF [commune_touchee] = 1 THEN [population] END` | Population des communes touchées (utilisée avec un filtre sur une famille) |

**Formats :** les taux sont au format **pourcentage**.

**Types de champs corrigés :**
- `annee` converti en **dimension** (sinon Tableau l'additionne) ;
- `code_dept` et `code_insee` en **texte** (sinon « 01 » devient « 1 ») ;
- `commune_touchee` en **dimension** (pour colorer 0 / 1) ;
- `latitude` et `longitude` avec le **rôle géographique** correspondant.

---

## 7. Le classeur Tableau : les 11 feuilles

| # | Feuille | Source | Construction | Question à laquelle elle répond |
|---|---|---|---|---|
| 1 | **Carte départements** | 5 | Longitude/Latitude ; Département en Détail ; Score priorité en **Taille** et en **Couleur** (palette rouge séquentielle) ; filtre Année = 2025 affiché ; infobulle : risques pesticides, PFAS, bactéries | **Où** sont les départements à risque ? |
| 2 | **Top 10** | 5 | Département en lignes, Score en colonnes ; filtre Année = 2025 **en contexte** ; filtre **Top 10 par score** ; tri décroissant ; barres rouges avec valeurs | **Quels** départements traiter en premier ? |
| 3 | **Évolution conformité** | 3 | Année (discrète) en colonnes, Taux de non-conformité en lignes ; barres ; Variation sur un an en infobulle | La qualité **s'améliore-t-elle** ? |
| 4 | **Évolution par famille** | 2 | Année en colonnes, Taux de dépassement en lignes, Famille en couleur ; lignes ; **PFAS exclus** (trop peu mesurés avant 2025) | **Quel polluant** pose problème, et comment ça évolue ? |
| 5 | **Carte de chaleur** | 2 | Région en lignes, Famille en colonnes, Taux de dépassement en couleur et en texte ; carrés ; filtre 2025 | **Quelle région** a **quel** problème ? |
| 6 | **Substances** | 4 | Substance en lignes, Nb dépassements en colonnes, Famille en couleur ; filtre 2025 (contexte) ; Top 10 ; tri décroissant | **Quelles molécules** dépassent le plus ? |
| 7 | **Carte communes** | 1b | Un point par commune (Code INSEE en Détail) ; Commune touchée en couleur (0 = gris clair, 1 = rouge) ; filtre Famille à valeur unique (Pesticides par défaut) ; petits points ; infobulle : commune, population, prélèvements, dépassements, ratio max / limite | **Quelles communes** sont touchées ? |
| 8 | **Fiche commune** | 1b | Famille en lignes ; Nb prélèvements et Nb dépassements en colonnes ; filtre Commune en saisie de texte | Que se passe-t-il **dans ma commune** ? |
| 9 | **KPI prélèvements** | 3 | SUM(Nb prélèvements), 2025, grande police | Combien de contrôles ? |
| 10 | **KPI conformité** | 3 | Taux de conformité, 2025, en % | Quelle part d'eau conforme ? |
| 11 | **KPI population exposée** | 1b | SUM(Population exposée), filtre Famille = Pesticides | Combien de personnes concernées ? |

### Techniques Tableau utilisées (à citer à l'oral)

- **Cartes** : symboles proportionnels (départements) et nuage de points (≈ 35 000 communes).
- **Champs calculés** : taux, moyenne pondérée, `ZN`, `IF … THEN … END`.
- **Calculs de table** : `LOOKUP` (variation annuelle), `RANK` (rang).
- **Expressions LOD** : `{ FIXED … }` (population sans double comptage, population régionale).
- **Filtres de contexte + Top N** : le Top 10 est calculé **après** le filtre Année (sinon le classement porterait sur toutes les années).
- **Filtres partagés** : le filtre Année s'applique à toutes les feuilles de la même source.
- **Action de filtre** sur le tableau de bord : cliquer un département filtre les autres graphiques.
- **Filtre en saisie de texte** pour le citoyen.
- **Publication** sur Tableau Public.

---

## 8. Le tableau de bord « Vue France » (pour le décideur)

**Titre :** *Qualité de l'eau du robinet : où agir en priorité ?*
**Taille :** 1366 × 768 (format écran / vidéoprojecteur).

**Organisation (de haut en bas) :**

```
┌──────────────────────────────────────────────────────────────────┐
│  Qualité de l'eau du robinet : où agir en priorité ?             │
├──────────────────┬──────────────────────┬────────────────────────┤
│ KPI prélèvements │ KPI conformité       │ KPI population exposée │
│ 295 134          │ 95,49 %              │ 15 762 665             │
├──────────────────┴──────────────┬───────┴──────┬─────────────────┤
│ Carte départements              │ Top 10       │ Filtre Année    │
│ (taille + couleur = score)      │ (barres)     │ + légendes      │
├─────────────────────────────────┼──────────────┴─────────────────┤
│ Évolution par famille (lignes)  │ Carte de chaleur (régions ×    │
│                                 │ familles)                      │
└─────────────────────────────────┴────────────────────────────────┘
```

**Interactions :**
- **Clic sur un département de la carte** → les autres graphiques se filtrent sur ce département (action de filtre « Utiliser comme filtre »).
- **Filtre Année** → appliqué à toutes les feuilles de la source « score de priorité ».

**Lecture en 10 secondes :** 3 chiffres → où (carte) → qui en premier (Top 10) → quel polluant (courbes) → quelle région (chaleur).

---

## 9. Le tableau de bord « Ma commune » (pour le citoyen)

**Organisation :**

```
┌──────────────────────────────────────────────────────────────────┐
│  Tapez votre commune : [__________________________]              │
├───────────────────────────────┬──────────────────────┬───────────┤
│ Fiche commune                 │ Carte communes       │ Famille   │
│ prélèvements et dépassements  │ (rouge = touchée)    │ (choix)   │
│ par famille                   │                      │ Légende   │
└───────────────────────────────┴──────────────────────┴───────────┘
```

**Utilisation :** on tape le nom de sa commune → la fiche et la carte n'affichent plus que cette commune. Le filtre **Famille** permet de passer des pesticides aux bactéries, nitrates, PFAS ou autres.

**Filtre vide = toute la France** (« inclure toutes les valeurs si vide »).

---

## 10. Les résultats chiffrés

> Les valeurs marquées « ≈ » sont lues sur les graphiques Tableau (arrondies) ; les autres viennent des calculs.

### 10.1 Les chiffres clés

| Indicateur | Valeur | Source / remarque |
|---|---|---|
| Prélèvements traités (2021–2026) | **≈ 1,6 million** | données brutes |
| Prélèvements en 2025 | **295 134** | KPI, source 3 |
| Taux de conformité 2025 | **95,49 %** | KPI, source 3 |
| Conformité chaque année | **plus de 94 %** | source 3 |
| Communes touchées par les pesticides (2025) | **5 954** | source 1b |
| Habitants concernés par les pesticides (2025) | **15,8 millions** (15 762 665) | KPI, source 1b |
| Communes au-dessus de la nouvelle limite européenne PFAS | **172** | source 1b / 2 |

### 10.2 Évolution du taux de non-conformité (France entière)

| Année | Non-conformité (≈) | Conformité (≈) |
|---|---|---|
| 2021 | 5,5 % | 94,5 % |
| 2022 | 5,6 % | 94,4 % |
| 2023 | 5,6 % | 94,4 % |
| 2024 | 5,8 % | 94,2 % |
| 2025 | **4,5 %** | **95,5 %** |
| 2026 | 4,5 % | 95,5 % *(année en cours : données possiblement partielles)* |

**Lecture :** hausse légère jusqu'en 2024, puis baisse en 2025. Environ trois quarts de cette baisse viennent du métabolite R471811, classé non pertinent par l'Anses en avril 2024 : sans les 2 727 prélèvements non conformes à cause de lui seul, 2024 serait à 4,84 %.

### 10.3 Évolution du taux de dépassement par famille (≈)

| Famille | 2021 | 2022 | 2023 | 2024 | 2025 | 2026 |
|---|---|---|---|---|---|---|
| **Pesticides** | 15,8 % | 16,9 % | 18,0 % | 18,5 % | 13,1 % | 17,5 % |
| Autres substances | 3,9 % | 3,4 % | 2,8 % | 2,7 % | 2,7 % | 2,9 % |
| Bactéries | 1,8 % | 1,8 % | 2,0 % | 1,9 % | 1,7 % | 1,7 % |
| Nitrates | 1,3 % | 1,1 % | 1,1 % | 1,4 % | 1,3 % | 0,8 % |
| PFAS | *exclus du graphique : presque pas mesurés avant 2025* | | | | | |

**Lecture :** les **pesticides** sont, de très loin, la famille qui dépasse le plus, chaque année.

### 10.4 Le Top 10 des départements prioritaires (2025)

| Rang | Département | Région | Score |
|---|---|---|---|
| 1 | **Somme** | Hauts-de-France | **11,58** |
| 2 | **Marne** | Grand Est | 9,44 |
| 3 | **Aisne** | Hauts-de-France | 9,24 |
| 4 | Oise | Hauts-de-France | 8,76 |
| 5 | Pas-de-Calais | Hauts-de-France | 8,35 |
| 6 | Calvados | Normandie | 6,71 |
| 7 | Nord | Hauts-de-France | 5,77 |
| 8 | Eure | Normandie | 5,22 |
| 9 | Guyane | Guyane | 3,87 |
| 10 | Seine-Maritime | Normandie | 3,30 |

**Lecture :** 5 départements sur 10 sont dans les **Hauts-de-France**, 3 en **Normandie**. La **Guyane** est la seule exception, avec un profil différent (« autres substances » et bactéries). Le score 2025 va de 0,01 à 11,58.

> Chiffres du tableau ci-dessous recalculés avec un comptage distinct des prélèvements.

### 10.5 Carte de chaleur : taux de dépassement par région et par famille (2025)

| Région | Autres | Bactéries | Nitrates | **Pesticides** | PFAS |
|---|---|---|---|---|---|
| Hauts-de-France | 2,0 % | 0,2 % | 6,5 % | 52,2 % | 0,8 % |
| Normandie | 0,4 % | 0,3 % | 0,4 % | 27,3 % | 1,5 % |
| Grand Est | 2,1 % | 2,2 % | 4,3 % | 21,6 % | 6,8 % |
| Île-de-France | 0,3 % | 0,0 % | 0,4 % | 16,2 % | 0,6 % |
| Centre-Val de Loire | 2,2 % | 0,4 % | 2,8 % | 14,4 % | 0,3 % |
| Bourgogne-Franche-Comté | 5,6 % | 1,7 % | 1,6 % | 5,5 % | 1,1 % |
| Guyane | 25,4 % | 5,6 % | 0,0 % | 3,2 % | n.m. |
| Occitanie | 4,4 % | 2,5 % | 0,2 % | 2,5 % | 3,1 % |
| Nouvelle-Aquitaine | 3,1 % | 0,7 % | 0,0 % | 1,5 % | 0,3 % |
| Bretagne | 0,4 % | 0,1 % | 0,0 % | 0,5 % | 0,3 % |
| Provence-Alpes-Côte d'Azur | 1,9 % | 2,8 % | 0,0 % | 0,4 % | 2,0 % |
| Auvergne-Rhône-Alpes | 2,4 % | 3,4 % | 0,1 % | 0,4 % | 2,3 % |
| Mayotte | 7,9 % | 2,1 % | 0,1 % | 0,0 % | n.m. |
| Martinique | 4,1 % | 1,5 % | 0,0 % | 0,0 % | n.m. |
| Guadeloupe | 9,9 % | 0,7 % | 0,0 % | 0,0 % | n.m. |
| Pays de la Loire | 0,6 % | 0,0 % | 0,1 % | 0,0 % | 0,0 % |
| Corse | 8,8 % | 10,1 % | 0,1 % | 0,0 % | n.m. |
| La Réunion | 2,4 % | 2,5 % | 0,0 % | 0,0 % | 0,0 % |

**Lecture :**
- **Pesticides** : un problème du **nord de la France** (Hauts-de-France un peu plus d'un prélèvement analysé sur 2 en dépassement, Normandie, Grand Est, Centre, Île-de-France).
- **Bactéries** : un problème de **Corse et d'outre-mer** (réseaux plus petits, climat, traitement).
- **PFAS** : premier signal en **Grand Est** (6,8 %), **Occitanie** (3,1 %) et **Auvergne-Rhône-Alpes** (2,3 %).
- **Outre-mer** : profil « autres substances » (turbidité, etc.), surtout en Guyane.

### 10.6 Les 10 substances qui dépassent le plus (2025, ≈ nombre de dépassements)

| Rang | Substance | Famille | Dépassements (≈) |
|---|---|---|---|
| 1 | **Chloridazone desphényl** | Pesticides (métabolite) | ≈ 4 500 |
| 2 | *Escherichia coli* /100 mL | Bactéries | ≈ 3 100 |
| 3 | Entérocoques /100 mL | Bactéries | ≈ 3 000 |
| 4 | Chloridazone méthyl desphényl | Pesticides (métabolite) | ≈ 2 400 |
| 5 | Total des pesticides analysés | Pesticides | ≈ 2 200 |
| 6 | Nitrates (en NO₃) | Nitrates | ≈ 1 600 |
| 7 | Chlorothalonil R417888 | Pesticides (métabolite) | ≈ 1 500 |
| 8 | Turbidité néphélométrique | Autres | ≈ 550 |
| 9 | Atrazine déséthyl | Pesticides (métabolite) | ≈ 500 |
| 10 | Chlorure de vinyle monomère | Autres | ≈ 400 |

**Lecture :** les dépassements de pesticides viennent surtout de **métabolites** (produits de dégradation) de molécules souvent **interdites depuis longtemps** :
- **chloridazone** : herbicide utilisé surtout sur la betterave jusqu'à fin 2020 ;
- **chlorothalonil** : fongicide utilisable jusqu'en mai 2020 ;
- **atrazine** : herbicide du maïs, interdit en 2003.

Ils persistent des années dans les nappes : c'est une **pollution héritée du passé**.

---

## 11. Les messages clés et les recommandations

### Les 5 messages à retenir

1. **L'eau du robinet est globalement de bonne qualité** : plus de 94 % des prélèvements sont conformes chaque année (95,5 % en 2025).
2. **Les pesticides sont le problème n°1**, loin devant les bactéries et les nitrates : 5 954 communes et 15,8 millions d'habitants concernés en 2025.
3. **Le problème est concentré géographiquement** dans le nord de la France (Hauts-de-France, Normandie, Grand Est).
4. **C'est en grande partie une pollution héritée** : des métabolites de pesticides interdits depuis des années.
5. **Les PFAS sont le prochain enjeu** : 172 communes au-dessus de la nouvelle limite européenne, avec des mesures encore récentes.

### Recommandations pour une ARS

| Priorité | Action | Où |
|---|---|---|
| 1 | Traiter les métabolites de pesticides (filtration charbon actif, interconnexion de réseaux, dilution) | Somme, Marne, Aisne, Oise, Pas-de-Calais |
| 2 | Protéger les captages (aires d'alimentation, réduction des intrants agricoles) | Hauts-de-France, Normandie, Grand Est |
| 3 | Renforcer la surveillance des PFAS et informer la population | Grand Est, Auvergne-Rhône-Alpes, 172 communes |
| 4 | Améliorer le traitement bactériologique des petits réseaux | Corse, Guyane, Mayotte, Guadeloupe |
| 5 | Informer les citoyens (outil « Ma commune ») | Partout |

---

## 12. Les limites et les points de vigilance

- **Score de priorité = choix de pondération.** D'autres poids donneraient un autre classement.
- **PFAS :** très peu mesurés avant 2025. Les comparer dans le temps n'a pas de sens, d'où leur exclusion de la courbe d'évolution.
- **2026 :** année en cours, données probablement **incomplètes**.
- **Dépassement ≠ danger immédiat.** Une limite de qualité est une valeur de précaution ; un dépassement de pesticide n'entraîne pas forcément une restriction de consommation.
- **Lignes « Null »** : elles venaient de Saint-Barthélemy et Saint-Martin, désormais regroupés avec la Guadeloupe dans le fichier 2 (même ARS).
- **Limites modifiées :** ESA métolachlore (2023) et R471811 (2025) n'ont plus de limite dans nos données ; les années ne sont pas strictement comparables.
- **Carte :** les départements et régions d'outre-mer élargissent la vue ; il faut zoomer sur la métropole.
- **Commune touchée** = au moins **un** dépassement dans l'année. Une commune avec un seul dépassement ponctuel compte autant qu'une commune avec des dépassements permanents.
- **Population exposée** = population de toute la commune touchée, même si une partie seulement du réseau est concernée. C'est un **majorant**.

---

## 13. Ce que le projet démontre (critères du cours)

| Exigence | Où dans le projet |
|---|---|
| Problématique métier | Priorisation des interventions d'une ARS (partie 1) |
| Préparation des données | 1,6 M de prélèvements → 6 CSV agrégés (parties 3–4) |
| Cartes | Carte départements (symboles proportionnels), Carte communes (points) |
| Mesures complexes | Taux, score pondéré, `LOOKUP`, `RANK`, LOD `FIXED`, filtres de contexte + Top N (partie 6) |
| Tableaux de bord | « Vue France » (décideur) et « Ma commune » (citoyen) |
| Interactivité | Action de filtre, filtres partagés, saisie de texte, filtre famille |
| Publication | Tableau Public (lien en tête de document) |
| Aide à la décision | Top 10, recommandations (partie 11) |

---

## 14. Préparation de l'oral

### 14.1 Le fil rouge (environ 10 minutes)

| Temps | Partie | Ce qu'on dit |
|---|---|---|
| 0:00 | Accroche | « Est-ce que vous savez ce qu'il y a dans l'eau que vous avez bue ce matin ? » |
| 0:30 | Problématique | Où l'eau dépasse-t-elle les limites ? Où une ARS doit-elle agir en premier ? |
| 1:30 | Données | Source officielle, 1,6 M de prélèvements, 5 familles, 6 fichiers |
| 3:00 | Méthode | Taux plutôt que comptes ; score de priorité pondéré ; pourquoi 30 % pour les bactéries |
| 4:30 | Démo « Vue France » | Les 3 KPI → la carte → le Top 10 → clic sur la Somme → les courbes → la carte de chaleur |
| 7:00 | Démo « Ma commune » | Taper une commune connue du jury → lire la fiche |
| 8:00 | Résultats et recommandations | Les 5 messages, les actions prioritaires |
| 9:00 | Limites | Pondération, PFAS récents, 2026 partiel |
| 9:30 | Conclusion | « L'eau est bonne à 95 %, mais 15,8 millions de personnes vivent dans une commune touchée par les pesticides, surtout dans le Nord. C'est là qu'il faut agir. » |

### 14.2 Questions probables du jury (et réponses)

**« Pourquoi ces poids dans le score ? »**
Les bactéries ont un effet immédiat sur la santé, d'où 30 %. Les autres familles ont des effets chroniques, d'où 20 %. « Autres » est une famille hétérogène, d'où 10 %, mais c'est discutable car elle contient aussi des métaux.

**« Pourquoi avoir mis le filtre Année en contexte ? »**
Sans contexte, Tableau calcule le Top 10 sur toutes les années, puis filtre 2025 : le classement serait faux. Le filtre de contexte s'applique **avant** le Top N.

**« À quoi sert le FIXED ? »**
Dans le fichier communes, chaque commune a 5 lignes, une par famille. Sans `FIXED`, sa population serait additionnée 5 fois. `{ FIXED [code_insee] : MAX([population]) }` la prend une seule fois par commune.

**« Pourquoi les PFAS ne sont pas sur la courbe ? »**
Ils n'étaient presque pas mesurés avant 2025. La courbe montrerait une fausse évolution.

**« Un dépassement, c'est dangereux ? »**
Pas forcément. Les limites sont des seuils de précaution. Les ARS évaluent ensuite le risque et peuvent restreindre la consommation si nécessaire.

**« Pourquoi les pesticides sont surtout dans le Nord ? »**
La chloridazone était surtout utilisée sur la betterave (ARS Hauts-de-France), mais nos données ne prouvent pas la cause. Les dépassements viennent surtout de métabolites d'herbicides et de fongicides (chloridazone, chlorothalonil) qui persistent dans les nappes même après l'interdiction des molécules.

**« Quelle différence entre taux de non-conformité et taux de dépassement ? »**
Le taux de non-conformité (source 3) porte sur **tous** les prélèvements d'un département et d'un mois. Le taux de dépassement (source 2) est calculé **par famille** de polluant : il dit quelle famille pose problème.

**« Pourquoi Tableau Public ? »**
C'est gratuit, ça se partage par un lien, et les données sont publiques, donc il n'y a pas de problème de confidentialité.

---

## 15. Glossaire

| Terme | Définition |
|---|---|
| **ARS** | Agence régionale de santé : elle organise le contrôle sanitaire de l'eau |
| **Contrôle sanitaire** | Programme officiel de prélèvements et d'analyses de l'eau distribuée |
| **Limite de qualité** | Seuil réglementaire à ne pas dépasser |
| **Métabolite** | Produit de dégradation d'un pesticide dans l'environnement |
| **PFAS** | Substances per- et polyfluoroalkylées, dites « polluants éternels » |
| **KPI** | Indicateur clé de performance (chiffre clé) |
| **Champ calculé** | Nouvelle colonne créée par une formule dans Tableau |
| **Calcul de table** | Calcul fait sur les résultats affichés (`RANK`, `LOOKUP`) |
| **Expression LOD** | Calcul à un niveau de détail fixé (`FIXED`), indépendant de l'affichage |
| **Filtre de contexte** | Filtre appliqué en premier, avant les Top N et les autres filtres |
| **Action de filtre** | Clic sur un graphique qui filtre les autres graphiques d'un tableau de bord |
| **Dimension / Mesure** | Dimension = catégorie (région, année) ; mesure = nombre qu'on agrège (somme, moyenne) |

---

## 16. Checklist finale

- [ ] Classeur publié sur Tableau Public et **masqué du profil** (« Show Viz on Profile » désactivé)
- [ ] Lien testé dans une fenêtre de navigation privée
- [ ] Les deux tableaux de bord testés (clic sur un département, saisie d'une commune)
- [ ] Captures d'écran : Vue France (complet), Ma commune (avec une commune tapée), Top 10, Carte de chaleur, Évolution par famille
- [ ] PowerPoint : noms du groupe (slide 1), captures (slide 13), lien Tableau (slide 16), chiffres vérifiés avec ce document
- [ ] Poster 2 pages imprimé
- [ ] Oral répété au moins une fois en minutant (10 min)
- [ ] Réponses aux questions du jury relues
- [ ] *(Bonus)* Paramètre « Poids pesticides » ajouté dans Tableau
