# Économétrie spatiale : dépenses publiques en Auvergne-Rhône-Alpes

Projet d'économétrie spatiale - Analyse des déterminants des dépenses communales (fonctionnement et investissement) en tenant compte des interactions spatiales entre communes voisines.

## Le projet

Étude portant sur **4 181 communes** de la région Auvergne-Rhône-Alpes, visant à identifier les déterminants socio-économiques, fonciers et fiscaux des dépenses communales par habitant, et à évaluer si les choix budgétaires d'une commune sont influencés par ceux de ses voisines.

### Variables étudiées

* **Variables dépendantes** : dépenses de fonctionnement (`lCFT_COM`) et d'investissement (`lDIT_COM`) par habitant, en logarithme
* **15 variables explicatives** : population, emploi, superficie, surfaces urbanisées, taux de chômage, personnes âgées isolées, logements sociaux/vacants/secondaires, part d'appartements, taux d'endettement, équipements scolaires, annuité de la dette, produit fiscal

### Démarche

1. **Construction de trois matrices de poids spatiaux**, avec des définitions de voisinage différentes : contiguïté (Queen), k-plus proches voisins (k=5), et distance avec seuil (12 km)
2. **Analyse descriptive spatiale** : cartographie, autocorrélation spatiale globale (I de Moran) et locale (LISA), ACP des variables explicatives
3. **Régression linéaire (MCO)** comme modèle de référence, avec diagnostics : hétéroscédasticité (test de Breusch-Pagan) et autocorrélation spatiale des résidus (test de Moran) — les deux hypothèses sont rejetées, ce qui invalide le MCO pour ces données
4. **Modèles économétriques spatiaux**, estimés successivement avec les trois matrices de poids et deux méthodes d'estimation (maximum de vraisemblance et moindres carrés en deux étapes / 2SLS) :
   * **Modèle SAR** (Spatial Autoregressive) : dépendance spatiale sur la variable expliquée
   * **Modèle de Durbin spatial** : ajoute les variables explicatives retardées spatialement (moyennes des communes voisines), permettant de décomposer les effets directs et indirects (impacts spatiaux)

## Résultats principaux

* Les **dépenses de fonctionnement** présentent une autocorrélation spatiale forte (I de Moran = 0,545), confirmée par des coefficients autorégressifs élevés dans les modèles SAR (ρ entre 0,53 et 0,89 selon la matrice et la méthode) : les dépenses d'une commune sont fortement liées à celles de ses voisines, notamment via les équipements scolaires et la population
* Les **dépenses d'investissement** montrent une autocorrélation spatiale plus faible (I de Moran = 0,231), le principal déterminant étant le taux d'endettement, avec des effets spatiaux plus limités
* La **matrice de distance avec seuil (12 km)** capture le mieux les interactions spatiales dans l'ensemble des modèles testés
* Le **modèle de Durbin** met en évidence des effets indirects souvent très importants (parfois supérieurs aux effets directs), confirmant une forte diffusion spatiale des comportements budgétaires entre communes proches
* La méthode d'estimation **2SLS (stsls)** est nettement plus rapide que le maximum de vraisemblance, mais tend à donner des coefficients autorégressifs plus élevés

## Structure du projet

* `Projet_économétrie_ARA.Rmd` : script R Markdown avec l'ensemble de l'analyse
* `Projet_économétrie_ARA.pdf` : rapport compilé
* `Evaluation économétrie spatiale.pdf` : énoncé/grille d'évaluation du projet
* `commune.shp`, `.dbf`, `.prj`, `.shx` : fonds de carte des communes françaises (format shapefile)

## Lancer l'analyse

1. Cloner le projet ou télécharger les fichiers
2. Installer les packages nécessaires

```r
install.packages(c("pacman", "ggplot2", "leaflet"))
pacman::p_load(here, sf, spdep, robustHD, mapsf, dplyr, spatialreg, GWmodel, tmap, lmtest, corrplot, factoextra)
```

3. S'assurer que le fichier de données `dat.RData` est présent dans le dossier (données communales, non fourni dans ce dépôt pour des raisons de taille/licence)
4. Ouvrir `Projet_économétrie_ARA.Rmd` dans RStudio et compiler (Knit)

## Auteur

Clara GAMBARDELLO
Projet Économétrie spatiale (2026)
