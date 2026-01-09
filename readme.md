# Moteur de Recherche Sémantique pour Articles Scientifiques

Ce projet implémente un **moteur de recherche sémantique avancé** destiné à la recommandation d'articles scientifiques. Il résout un problème de **Citation Matching** : étant donné un article (requête), retrouver les articles qu'il cite parmi une liste de candidats, en distinguant les liens pertinents du bruit.

Le projet compare et combine des approches classiques (fréquentielles), neuronales (Transformers) et topologiques (Graphes).

## 🎯 Objectifs

* **Recherche d'Information (IR) :** Construire un système capable de classer des documents scientifiques par pertinence sémantique.
* **Comparaison d'approches :** Évaluer le gap de performance entre les méthodes creuses (Sparse), les représentations denses (Embeddings) et les approches structurelles (Graphes).
* **Analyse de contenu :** Comparer l'impact de l'utilisation des titres seuls par rapport à l'utilisation combinée des titres et des résumés.

## 🗂️ Dataset

Le projet s'appuie sur un corpus bibliographique réel :
* **Corpus :** 25 657 articles scientifiques contenant titres, résumés et métadonnées (auteurs).
* **Requêtes :** 1 000 articles sources utilisés comme requêtes.
* **Vérité Terrain :** 20 950 paires annotées (score 0 ou 1) pour la validation et l'évaluation des modèles.

## ⚙️ Méthodologie

Le pipeline du projet suit une complexité croissante à travers trois axes majeurs :

### 1. Approche "Sparse" (Baseline)
Utilisation de méthodes statistiques classiques pour établir une ligne de base basée sur la fréquence des mots.
* **Bag-of-Words (BoW) :** Encodage simple via `CountVectorizer` pour construire une matrice documents-termes.
* **TF-IDF :** Pondération des termes pour réduire l'importance des mots trop fréquents et valoriser les mots discriminants.
* **Résultat :** Capture efficace des mots-clés exacts mais difficulté à saisir la synonymie.

### 2. Approche "Dense" (Transformers)
Utilisation de modèles d'état de l'art pour projeter les textes dans un espace vectoriel sémantique (Embeddings).
* **Modèle :** `all-MiniLM-L6-v2` via la bibliothèque `sentence-transformers`.
* **Stratégies de contenu :**
    * *Titres seuls :* Encodage rapide, focalisé sur l'idée principale.
    * *Enrichi (Titre + [SEP] + Résumé) :* Maximise l'information contextuelle pour une meilleure précision.
* **Similarité :** Utilisation de la similarité cosinus pour le classement des documents.



### 3. Approche Topologique (Graphes)
Exploitation de la structure relationnelle entre les documents, les mots et les auteurs.
* **Structure du Graphe :** Construction d'un graphe hétérogène incluant des nœuds pour les documents, les mots (filtrés par fréquence) et les auteurs.
* **Node Embeddings :** Utilisation de **Random Walks** (marches aléatoires) et de l'algorithme **Word2Vec** (Skip-gram) pour apprendre des représentations vectorielles basées sur la topologie du réseau.
* **Avantage :** Capture des relations de co-citation et d'expertise via les auteurs communs.

## 🛠️ Stack Technique

* **Langage :** Python 3.10
* **NLP & Embeddings :** `sentence-transformers`, `scikit-learn`, `gensim`
* **Graphes :** `networkx`
* **Manipulation de données :** `pandas`, `numpy`
* **Visualisation :** `matplotlib`

## 🚀 Installation et Exécution

1. **Cloner le dépôt :**
   ```bash
   git clone https://github.com/aitkherimed1228/SearchEngine
   ```