# 🛒 Segmentation des clients d’un site e-commerce (Olist)

## 🎯 Objectif du projet
Segmenter les clients d’Olist, une marketplace brésilienne, selon leur comportement d’achat afin d’optimiser les actions marketing et la fidélisation.

L’objectif principal est d’obtenir des **segments clients exploitables** permettant de :
- Identifier les profils à forte valeur.
- Réengager les clients inactifs.
- Optimiser les dépenses marketing.

---

## 🧠 Contexte
Olist connecte des vendeurs indépendants à des plateformes e-commerce comme **Amazon**, **Mercado Libre** et **Magalu**.  
Les données mises à disposition (issues d’un dataset public) contiennent des informations détaillées sur :
- les **clients**,  
- les **commandes**,  
- les **produits**,  
- les **paiements**,  
- et les **avis clients**.

---

## 🗂️ Données utilisées
| Table | Description |
|--------|--------------|
| `customers_df` | Informations clients |
| `orders_df` | Commandes et statuts |
| `order_items_df` | Produits achetés |
| `payments_df` | Montants et modes de paiement |
| `reviews_df` | Avis et notes clients |

---

## 🧮 Étapes du projet

### 1️⃣ Exploration & SQL (BigQuery)
Analyse initiale via SQL pour :
- Identifier les **commandes livrées en retard**  
- Trouver les **vendeurs performants (>100K BRL)**  
- Détecter les **nouveaux vendeurs actifs**  
- Repérer les **zones à faible satisfaction client**

➡️ *Requêtes SQL disponibles dans* [`Rota-Nodari_Antoine_1_script_022025.pdf`](./Rota-Nodari_Antoine_1_script_022025.pdf)

---

### 2️⃣ Construction du modèle RFM(R)
Création d’un modèle **RFM étendu** intégrant un indicateur de satisfaction :
- **Recency** → jours depuis la dernière commande  
- **Frequency** → nombre total de commandes  
- **Monetary** → montant total dépensé  
- **Review** → score moyen des avis clients  

➡️ Calculs et agrégations réalisés dans le notebook [`notebook_exploration_essai_022025.ipynb`](./Rota-Nodari_Antoine_2_notebook_exploration_essai_022025.ipynb)

---

### 3️⃣ Feature Engineering
- Imputation des valeurs manquantes (review)
- Transformation logarithmique pour limiter les valeurs extrêmes
- Normalisation avec `StandardScaler`

---

### 4️⃣ Clustering (K-Means & DBSCAN)
- Sélection du nombre de clusters par la **méthode du coude**  
- Validation avec le **coefficient de silhouette**  
- Visualisation 2D via **ACP (PCA)**  
- Comparaison K-Means vs DBSCAN  

➡️ K-Means (k=4) retenu pour sa meilleure lisibilité et actionnabilité.

---

### 5️⃣ Analyse des segments
| Cluster | Profil client | Insights | Recommandations |
|----------|----------------|-----------|----------------|
| 0 | Clients insatisfaits et peu actifs | Peu d’achats, faibles reviews | Réengagement & support |
| 1 | Clients fidèles et engagés | Achats fréquents, bons avis | Fidélisation premium |
| 2 | Nouveaux clients satisfaits | Petits paniers, bonnes notes | Cross-selling |
| 3 | Clients dormants mais satisfaits | Inactifs, bons avis | Relance par promotions |

---

### 6️⃣ Maintenance du modèle
Étude de la **stabilité temporelle** du clustering :
- Fenêtre : 3 mois glissants  
- Pas de 2,5 semaines  
- Mesure de stabilité : **Adjusted Rand Index (ARI)**  

Résultats :
- Modèle **sans review** stable (ARI ≈ 0.8)  
- Modèle **avec review** instable → reviews exclues de la version finale.

➡️ Détails dans le notebook [`notebook_maintenance_022025.ipynb`](./Rota-Nodari_Antoine_3_notebook_maintenance_022025.ipynb)

---

## 📊 Résultats principaux
- 4 segments clients identifiés, exploitables par les équipes marketing.  
- Inclusion du score d’avis testée mais instable dans le temps.  
- Recommandation : recalibrage du modèle tous les **3-4 mois**.

---

## 🧰 Outils et technologies
- **Langages** : Python, SQL  
- **Librairies** : pandas, numpy, matplotlib, scikit-learn  
- **Base de données** : Google BigQuery  
- **Outils** : Jupyter Notebook, GSlides  

---

## 📎 Livrables
- 📘 [Notebook d’exploration](./Rota-Nodari_Antoine_2_notebook_exploration_essai_022025.ipynb)  
- 📗 [Notebook de maintenance](./Rota-Nodari_Antoine_3_notebook_maintenance_022025.ipynb)  
- 📄 [Présentation finale](./Rota-Nodari_Antoine_4_presentation_022025.pdf)  
- 📜 [Script SQL complet](./Rota-Nodari_Antoine_1_script_022025.pdf)

---

## 💡 Enseignements clés
- L’ajout de la variable **Review** enrichit la compréhension client mais compromet la stabilité du modèle.  
- L’approche RFM reste un levier puissant pour **segmenter efficacement** une base client e-commerce.  
- Importance du **suivi temporel** des clusters pour un déploiement durable.

