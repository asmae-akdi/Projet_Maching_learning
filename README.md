# Mini-projet Machine Learning : prédiction du décrochage scolaire

Jeu de données : UCI n°697, *Predict Students' Dropout and Academic Success*
(4 424 étudiants, 36 variables, cible à 3 classes : `Dropout`, `Enrolled`, `Graduate`).

## Répartition du travail

| Membre | Travail | Dossier / notebook |
|---|---|---|
| P1 | Exploration, prétraitement, arbre de décision écrit à la main, sélection des variables | `P1/projet_ml.ipynb` |
| P2 | Régression logistique vs SVM (F1, matrices de confusion, courbes ROC) | `P2/` |
| P3 | K-means (silhouette, centroïdes, profils d'étudiants) | `P3/` |

Chacun travaille dans **son propre dossier** et son propre notebook (les notebooks `.ipynb`
se fusionnent très mal quand deux personnes modifient le même fichier). Le notebook final
sera assemblé à la fin.

## Organisation du dépôt

```
.
├── README.md
├── requirements.txt
├── data.csv                 <- jeu de données d'origine (séparateur « ; »)
├── data_split/              <- données déjà découpées par P1 (train / test)
│   ├── X_train.csv  X_test.csv  y_train.csv  y_test.csv
│   ├── config.json          <- graine, classes, listes de variables par type
│   └── selected_features.json   <- variables retenues par P1 (k = 4)
├── figures/                 <- images pour le rapport
├── P1/projet_ml.ipynb       <- code commenté de P1
├── P2/   P3/                <- un dossier par membre
```

## Démarrer

```bash
pip install -r requirements.txt
```

Dans votre notebook, chargez les données déjà découpées (chemins `../data_split/...` si le
notebook est dans `P2/` ou `P3/`), puis **copiez depuis `P1/projet_ml.ipynb` les fonctions
`fit_preprocessor` et `transform_preprocessor`** (section Prétraitement, version finale) :

```python
prep = fit_preprocessor(X_train, features, config, min_count=20)  # apprend sur le TRAIN seulement
Xtr  = transform_preprocessor(X_train, prep)
Xte  = transform_preprocessor(X_test,  prep)
```

## Règles communes (à respecter par tous)

1. **Graine unique : 42** (`random_state=42`) partout.
2. **Même découpage train/test pour tout le monde** : on utilise `data_split/`, on ne redécoupe pas.
3. **Le jeu de test ne sert qu'à l'évaluation finale**, une seule fois par modèle. Les réglages
   (hyperparamètres, nombre de clusters…) se choisissent par validation croisée sur le train.
4. **Rien ne s'apprend sur le test** (moyennes, catégories, sélection de variables…).
5. **Ordre des classes** : `Dropout`, `Enrolled`, `Graduate`.
6. **Scénario principal** : prédiction en fin de 2e semestre (toutes les variables disponibles).
7. Juger sur le **F1 macro** et les résultats par classe, pas seulement sur la précision globale.

## Variables retenues (P1)

`Curricular units 2nd sem (approved)`, `Tuition fees up to date`, `Course`,
`Curricular units 1st sem (evaluations)`.

Attention (K-means) : `Course` est une variable catégorielle à 17 modalités ; après encodage,
elle fournit 17 des 20 colonnes, donc elle domine la distance euclidienne si on ne fait rien.

## Données

Jeu de données sous licence CC BY 4.0 : Realinho, V., Vieira Martins, M., Machado, J., &
Baptista, L. (2021). *Predict Students' Dropout and Academic Success* [Dataset]. UCI Machine
Learning Repository. https://doi.org/10.24432/C5MC89
