# Que contient l'eau de votre robinet ?

Projet du séminaire BI et Data Viz (Ismail Rebai) : analyse de la qualité de l'eau potable en France de 2021 à 2026 avec Tableau.

**Problématique :** où l'eau du robinet dépasse-t-elle les limites sanitaires, et quels départements une Agence régionale de santé doit-elle traiter en priorité ?

**Classeur Tableau Public :** https://public.tableau.com/app/profile/mohammed.benakka.ouayad/viz/Qualitdeleaudurobinet/VueFrance

## Contenu du dépôt

| Fichier ou dossier | Contenu |
|---|---|
| `rapport/` | Rapport du projet (source LaTeX, PDF et figures) |
| `Presentation_Eau_du_robinet_v2.pptx` | Présentation pour l'oral, version finale (avec notes) |
| `Presentation_Eau_du_robinet.pptx` | Première version de la présentation |
| `Poster_Eau_du_robinet.pdf` | Poster de synthèse imprimable (source : `Poster_Eau_du_robinet.html`) |
| `donnees_tableau/` | Fichiers CSV préparés et chargés dans Tableau |
| `captures/` | Captures d'écran des feuilles et tableaux de bord Tableau |
| `Projet_Eau_Contenu_Complet.md` | Dossier de référence : méthode, formules, résultats, préparation de l'oral |
| `GUIDE_TABLEAU.md` | Guide pour reconstruire le classeur Tableau pas à pas |

## Données

Source : ministère de la Santé, « Résultats du contrôle sanitaire de l'eau distribuée commune par commune », data.gouv.fr (Licence Ouverte). Population et coordonnées des communes : Insee, via geo.api.gouv.fr.

Les fichiers bruts (environ 1,7 Go) ne sont pas dans le dépôt. Ils se téléchargent sur data.gouv.fr. Le fichier `1_communes_qualite_eau.csv` (78 Mo) n'y est pas non plus : il n'est pas utilisé dans le classeur, qui s'appuie sur `1b_communes_2025_leger.csv`.

## Compiler le rapport

```bash
cd rapport
tectonic Rapport_Eau_du_robinet.tex
```

Le rapport peut aussi être ouvert dans Overleaf en important le fichier `.tex` et le dossier `figures/`.

## Groupe

Mohammed BENAKKA OUAYAD, Hajar BITARI, Aya SINDEL
