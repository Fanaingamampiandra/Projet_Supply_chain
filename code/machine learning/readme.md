# Analyse de la Base de Données d'Achats

Ce document décrit la structure et les analyses potentielles de la base de données d'achats fournie (`data.csv`).
Le script Python `projet.ipynb` accompagne ce README et met en œuvre certaines des analyses décrites.

## 1. Description de la Base de Données

La base de données contient des informations détaillées sur les transactions d'achat d'une entreprise, incluant des détails sur les produits, les fournisseurs, les quantités, les prix, et les délais de livraison.

**Nom du fichier :** `data.csv`
**Format :** CSV (Comma Separated Values)
**Encodage supposé :** UTF-8

## 2. Structure des Données (Colonnes)

| Nom de la Colonne          | Type de Donnée (Python) | Description                                                                   |
| -------------------------- | ------------------------ | ----------------------------------------------------------------------------- |
| `id_achat`               | object (string)          | Identifiant unique de la transaction d'achat.                                 |
| `date_achat`             | datetime64[ns]           | Date à laquelle l'achat a été effectué.                                   |
| `id_produit`             | object (string)          | Identifiant unique du produit acheté.                                        |
| `quantité`              | int64                    | Nombre d'unités du produit achetées.                                        |
| `id_fournisseur`         | object (string)          | Identifiant unique du fournisseur.                                            |
| `prix_unitaire`          | float64                  | Prix d'achat unitaire du produit pour cette transaction.                      |
| `délai_livraison_jours` | int64                    | Délai de livraison réel observé pour cet achat (en jours).                 |
| `montant_total`          | float64                  | Montant total de la transaction d'achat (`quantité` * `prix_unitaire`).  |
| `mois`                   | int64                    | Mois de la transaction (extrait de `date_achat`).                           |
| `année`                 | int64                    | Année de la transaction (extrait de `date_achat`).                         |
| `jour_semaine`           | int64                    | Jour de la semaine de la transaction (0=Lundi, 6=Dimanche).                   |
| `catégorie`             | object (string)          | Catégorie à laquelle appartient le produit.                                 |
| `marque`                 | object (string)          | Marque du produit.                                                            |
| `prix`                   | float64                  | Prix catalogue/de vente du produit (différent du `prix_unitaire` d'achat). |
| `stock_minimum`          | int64                    | Niveau de stock minimum requis pour ce produit.                               |
| `nom_fournisseur`        | object (string)          | Nom du fournisseur.                                                           |
| `ville`                  | object (string)          | Ville du fournisseur.                                                         |
| `pays`                   | object (string)          | Pays du fournisseur.                                                          |
| `fiabilité`             | float64                  | Indicateur de fiabilité du fournisseur (probablement entre 0 et 1).          |
| `délai_moyen_jours`     | int64                    | Délai de livraison moyen habituellement pratiqué par ce fournisseur.        |
| `niveau_stock`           | int64                    | Niveau de stock actuel du produit au moment de l'analyse/transaction.         |
| `entrepot`               | object (string)          | Entrepôt de destination ou de gestion de la commande.                        |

## 3. Analyses Proposées et Mises en Œuvre

Le script Python effectue les analyses suivantes :

### 3.1. Analyse Exploratoire des Données (EDA)

* **Statistiques descriptives** : Résumé des colonnes numériques (moyenne, médiane, min, max, quantiles).
* **Types de données et valeurs manquantes** : Vérification de la cohérence des données.
* **Distributions** :
  * Histogramme des montants totaux des achats.
  * Histogramme des délais de livraison observés.
  * Diagramme à barres pour la fréquence des catégories de produits.
  * Diagramme à barres pour le top 10 des fournisseurs par volume d'achats.
  * Diagramme à barres du montant total des achats par mois.
* **Visualisations de relations** :
  * Scatter plot de la fiabilité vs. le délai moyen de livraison spécifié par les fournisseurs.

### 3.2. Classification des Fournisseurs

L'objectif est de regrouper les fournisseurs en segments basés sur leur performance et leurs caractéristiques.

* **Agrégation des données** : Calcul de métriques clés par fournisseur (fiabilité moyenne, délai moyen spécifié, nombre total d'achats, montant total des achats, quantité totale achetée, prix unitaire moyen payé, délai de livraison moyen observé).
* **Segmentation simple** : Une classification basique est proposée en utilisant les quantiles de `fiabilité` et `délai_moyen_livraison_specifique` pour créer des segments (ex: "Haute Fiabilité - Délai Court").
* **Pistes pour classification avancée (optionnel)** :
  * Utilisation d'algorithmes de clustering non supervisé comme K-Means sur des caractéristiques normalisées des fournisseurs (fiabilité, délai moyen, volume d'affaires, prix moyens).
  * L'analyse des clusters résultants permettrait d'identifier des profils types de fournisseurs.

### 4. Instructions pour exécuter le code Python

1. **Prérequis** :

   * Python 3.x
   * Bibliothèques Python : `pandas`, `numpy`, `matplotlib`, `seaborn`, `scikit-learn`.
     Vous pouvez les installer avec pip :

   ```bash
   pip install pandas numpy matplotlib seaborn scikit-learn
   ```
2. **Fichier de données** :

   * Assurez-vous que le fichier `data.csv` se trouve dans le même répertoire que le script Python, ou modifiez le chemin d'accès dans la fonction `pd.read_csv()`.
3. **Exécution** :

   * Lancez le script depuis votre terminal :

   ```bash
   python nom_du_script.py
   ```

   (Remplacez `nom_du_script.py` par le nom réel de votre fichier Python).
4. **Résultats** :

   * Le script affichera des informations, des statistiques et des graphiques directement dans la console ou dans des fenêtres de visualisation.
   * Les performances du modèle de prédiction seront également affichées.

## 5. Pistes d'Amélioration et Analyses Futures

* **Affiner la classification des fournisseurs** : Utiliser des techniques de clustering plus avancées, tester différents nombres de clusters, et analyser en profondeur les caractéristiques de chaque segment.
* **Optimiser le modèle de prédiction** : Tester différents algorithmes de régression, effectuer un réglage fin des hyperparamètres, et explorer d'autres features (ex: interactions entre variables, saisonnalité plus fine).
* **Analyse des produits** : Identifier les produits les plus commandés, ceux avec les marges les plus élevées (si les données le permettent), ceux ayant les délais de livraison les plus longs.
* **Analyse des coûts** : Étudier l'évolution des prix unitaires, comparer les prix entre fournisseurs pour des produits similaires.
* **Gestion des stocks** : Analyser la relation entre `niveau_stock`, `stock_minimum` et les décisions d'achat.
* **Détection d'anomalies** : Identifier des transactions ou des comportements de fournisseurs atypiques.
