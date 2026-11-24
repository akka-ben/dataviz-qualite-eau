# Guide Tableau pas à pas : « Que contient l'eau de votre robinet ? »

Ce guide te permet de reconstruire dans **Tableau Public** (dans le navigateur) toutes les visualisations du projet, puis de les publier.
Temps estimé : 1 h 30 à 2 h.

---

## 0. Les fichiers à utiliser

Tous les fichiers sont dans le dossier `projet-eau/donnees_tableau/`.

| Fichier | Contenu | Utilisé pour |
|---|---|---|
| `5_score_priorite_departements.csv` | Score de priorité par département et par année, avec la latitude et la longitude | Carte « Vue France », top 10 |
| `2_departements_synthese.csv` | Par département, année et famille de polluants : prélèvements, dépassements, population exposée | Courbes, carte de chaleur |
| `3_prelevements_conformite.csv` | Conformité officielle des prélèvements, par mois et par département | Évolution globale, indicateurs clés |
| `4_substances.csv` | Nombre de dépassements par substance et par année | Classement des substances |
| `1b_communes_2025_leger.csv` | Résultats 2025 par commune, avec la latitude et la longitude | Carte des communes, « Ma commune » |
| `1_communes_qualite_eau.csv` | La même chose pour 2021-2026 (81 Mo) | Seulement si le chargement fonctionne, sinon garder la version légère |

**Les 5 familles de polluants :** Pesticides, Bactéries, Nitrates, PFAS, Autres substances.

**Définitions (à garder en tête) :**
- *Prélèvement avec dépassement* : au moins une analyse au-dessus de la limite sanitaire.
- *Commune touchée* : au moins un prélèvement avec dépassement dans l'année.
- *Score de priorité* = 30 % bactéries + 20 % nitrates + 20 % pesticides + 20 % PFAS + 10 % autres, chaque terme étant la part de la population du département concernée, pondérée par le taux de dépassement de sa commune.

---

## 1. Charger les données

1. Sur public.tableau.com, clique sur **Créer > Web Authoring** (ou « Créer une Viz »).
2. Dans **Se connecter à des données**, choisis **Fichiers** et charge `5_score_priorite_departements.csv`.
3. Va sur la première feuille (**Feuille 1** en bas).
4. Pour ajouter les autres fichiers comme sources séparées : menu **Données > Nouvelle source de données**, puis charge chaque fichier un par un. Pas besoin de les joindre entre eux : chaque feuille utilise une seule source.

**Vérifie les types de champs** (l'icône à gauche du nom de chaque colonne) :
- `annee` : si Tableau l'affiche comme une mesure (#), fais clic droit > **Convertir en dimension**.
- `code_dept`, `code_insee` : ce sont des chaînes de caractères (Abc). Si ce n'est pas le cas, change le type, sinon le « 01 » devient « 1 ».
- `latitude` et `longitude` : clic droit > **Rôle géographique** > **Latitude** (puis **Longitude**).

---

## 2. Les champs calculés (les « mesures complexes »)

Pour créer un champ : clic droit dans le panneau **Données** > **Créer un champ calculé**. Copie-colle les formules ci-dessous.

### Source `2_departements_synthese`
```
// Taux de dépassement
SUM([nb_prelevements_depassement_communes]) / SUM([nb_prelevements_communes])
```
```
// Part de la population exposée
SUM([population_exposee]) / SUM([population_controlee])
```
Mets ces deux champs au format pourcentage : clic droit > **Propriétés par défaut** > **Format numérique** > **Pourcentage**.

### Source `3_prelevements_conformite`
```
// Taux de non-conformité
SUM([nb_non_conformes]) / SUM([nb_prelevements])
```
```
// Variation sur un an (calcul de table)
(SUM([nb_non_conformes]) / SUM([nb_prelevements]))
 - LOOKUP(SUM([nb_non_conformes]) / SUM([nb_prelevements]), -1)
```

### Source `5_score_priorite_departements`
```
// Score recalculé (montre le calcul pondéré dans Tableau)
0.30 * ZN([risque_bacteries_pct]) + 0.20 * ZN([risque_nitrates_pct])
+ 0.20 * ZN([risque_pesticides_pct]) + 0.20 * ZN([risque_pfas_pct])
+ 0.10 * ZN([risque_autres_pct])
```
```
// Rang du département (calcul de table)
RANK(SUM([score_priorite]))
```
```
// Population de la région (expression de niveau de détail FIXED)
{ FIXED [region], [annee] : SUM([population_dept]) }
```

### Source `1b_communes_2025_leger`
```
// Population de la commune (FIXED : évite de la compter 5 fois, une par famille)
{ FIXED [code_insee] : MAX([population]) }
```
```
// Population exposée
IF [commune_touchee] = 1 THEN [population] END
```

**Bonus (très apprécié) : un paramètre pour les poids.** Crée un paramètre « Poids pesticides » (nombre décimal de 0 à 1, valeur 0,2), remplace `0.20` par ce paramètre dans le score recalculé, puis clic droit sur le paramètre > **Afficher le paramètre**. Le décideur peut alors tester ses propres priorités.

---

## 3. Les feuilles à construire

Renomme chaque feuille (double-clic sur l'onglet) avec le nom indiqué en gras.

### Feuille **« Carte départements »**, source `5_score…`
1. Double-clique sur **Longitude**, puis sur **Latitude** : un fond de carte apparaît.
2. Glisse `departement` sur **Détail**.
3. Glisse `score_priorite` sur **Taille** et sur **Couleur**. Choisis une palette rouge séquentielle (Couleur > Modifier les couleurs > Rouge).
4. Glisse `annee` dans **Filtres**, coche 2025, puis clic droit sur le filtre > **Afficher le filtre**.
5. Glisse `risque_pesticides_pct`, `risque_pfas_pct` et `risque_bacteries_pct` sur **Infobulle**.

> Si Tableau reconnaît les départements : clic droit sur `departement` > **Rôle géographique** > **Comté** (selon la version, ce rôle couvre les départements français). Double-clique ensuite sur `departement` pour obtenir une carte remplie (polygones). Garde la version en points si ça ne marche pas.

### Feuille **« Top 10 »**, source `5_score…`
1. `departement` sur **Lignes**, `score_priorite` sur **Colonnes**.
2. Filtre `annee` = 2025, puis tri décroissant (bouton de tri dans la barre d'outils).
3. Glisse `rang` dans **Filtres** et garde les rangs ≤ 10. Tu peux aussi faire clic droit sur `departement` > Filtre > Premiers > 10 par `score_priorite`.
4. Couleur rouge, et active l'affichage des valeurs (bouton **T** dans la barre d'outils).

### Feuille **« Évolution conformité »**, source `3_prelevements…`
1. `annee` sur **Colonnes**, `Taux de non-conformité` sur **Lignes**.
2. Clic droit sur `annee` > **Discret**. Choisis le type **Barres**.
3. Ajoute une deuxième version avec le champ `Variation sur un an` en **Infobulle**. Clic droit sur le champ > **Calculer avec** > **Tableau (horizontal)**.

### Feuille **« Évolution par famille »**, source `2_departements…`
1. `annee` sur **Colonnes**, `Taux de dépassement` sur **Lignes**, `famille` sur **Couleur**.
2. Type **Ligne**. Filtre `famille` : décoche PFAS, trop peu mesurés avant 2025.

### Feuille **« Carte de chaleur »**, source `2_departements…`
1. `region` sur **Lignes**, `famille` sur **Colonnes**, `Taux de dépassement` sur **Couleur**.
2. Type **Carré**. Filtre `annee` = 2025. Ajoute `Taux de dépassement` sur **Texte**.

### Feuille **« Substances »**, source `4_substances`
1. `substance` sur **Lignes**, `nb_depassements` sur **Colonnes**, `famille` sur **Couleur**.
2. Filtre `annee` = 2025, tri décroissant, filtre Premiers > 10.

### Feuille **« Carte communes »**, source `1b_communes…`
1. Double-clique sur **Longitude** puis **Latitude**, et glisse `code_insee` sur **Détail**.
2. Filtre `famille` (affiche le filtre, choix unique), par défaut **Pesticides**.
3. `commune_touchee` sur **Couleur** : clic droit > **Dimension**, puis 0 = gris clair et 1 = rouge.
4. Réduis la taille des points (Taille au minimum).
5. Glisse `commune`, `population`, `nb_prelevements`, `nb_prelevements_depassement` et `ratio_max_limite` sur **Infobulle**.

### Feuilles indicateurs (KPI), une par chiffre
- **« KPI prélèvements »** : `SUM(nb_prelevements)` sur **Texte**, source 3, filtre 2025. Agrandis la police.
- **« KPI conformité »** : `1 - Taux de non-conformité`, au format pourcentage.
- **« KPI population exposée »** : `SUM(Population exposée)`, source 1b, filtre famille = Pesticides.

### Feuille **« Fiche commune »**, source `1b_communes…`
1. `famille` sur **Lignes**, `nb_prelevements` et `nb_prelevements_depassement` sur **Colonnes**.
2. Glisse `commune` dans **Filtres** (Tous), puis affiche le filtre en mode **Saisie de texte** ou **Liste déroulante**.

---

## 4. Les tableaux de bord

### Tableau de bord **« Vue France »** (pour le décideur)
1. **Nouveau tableau de bord** (icône en bas). Taille : **Automatique**, ou 1366 × 768.
2. En haut, mets les 3 feuilles KPI côte à côte.
3. Au milieu à gauche **Carte départements**, à droite **Top 10**.
4. En bas **Évolution par famille** et **Carte de chaleur**.
5. Ajoute un titre : « Qualité de l'eau du robinet : où agir en priorité ? ».
6. **Interaction :** sur la carte, clique sur l'icône entonnoir (« Utiliser comme filtre ») : cliquer un département filtre les autres graphiques.
7. Applique le filtre `annee` à toutes les feuilles : clic droit sur le filtre > **Appliquer aux feuilles de calcul** > **Toutes celles utilisant cette source**.

### Tableau de bord **« Ma commune »** (pour le citoyen)
1. En haut, le filtre `commune` en saisie de texte (« Tapez votre commune »).
2. À gauche **Fiche commune**, à droite **Carte communes**.
3. Applique le filtre `commune` aux deux feuilles.

---

## 5. Publier

1. Menu **Fichier > Enregistrer** (ou **Publier**) sur Tableau Public. Donne le titre « Que contient l'eau de votre robinet ? ».
2. Sur la page du classeur, vérifie que la visibilité est **publique**, puis copie le lien.
3. Colle ce lien dans la dernière slide du PowerPoint, à la place de « [à compléter] ».

---

## 6. Finaliser le PowerPoint (`Presentation_Eau_du_robinet.pptx`)

- **Slide 1 :** remplace `[Nom 1]`… `[Nom 4]` par les noms du groupe.
- **Slide 13 :** fais une capture d'écran de chaque tableau de bord (Cmd + Maj + 4 sur Mac) et colle-la dans le cadre en pointillés, puis supprime le cadre.
- **Slides 8 et 11 (facultatif) :** remplace les cartes par tes captures de la feuille « Carte communes » (famille Pesticides, puis PFAS).
- **Slide 16 :** colle le lien Tableau Public.
- Les notes du présentateur (sous chaque slide) contiennent ce qu'il faut dire à l'oral.

---

## 7. Si quelque chose bloque

| Problème | Solution |
|---|---|
| Le fichier de 81 Mo ne se charge pas | Utilise `1b_communes_2025_leger.csv` (15 Mo) |
| `annee` est additionnée (8 090 au lieu de 2025) | Clic droit > Convertir en dimension |
| La carte n'affiche rien | Vérifie que latitude et longitude ont bien leur rôle géographique |
| Les codes « 01 » deviennent « 1 » | Change le type de `code_dept` en Chaîne |
| Les pourcentages affichent 0,05 | Format numérique > Pourcentage |
