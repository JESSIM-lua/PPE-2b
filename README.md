# 🚚 **Documentation de la Base de Données - Application de Gestion de Livraisons**

---

## 📊 **À quoi sert ce logiciel ?**

Ce logiciel permet la **gestion centralisée des livraisons entre des entreprises et des livreurs**. Il facilite la **création et le suivi des paniers de produits**, l'**assignation des livreurs aux entreprises**, et la **gestion des livraisons et de leur statut**. Grâce à cette base de données, il est possible de :

- **Créer des paniers contenant des produits.**  
- **Assigner des livreurs à des entreprises.**  
- **Suivre les livraisons et leurs statuts.**  
- **Gérer les commentaires liés aux livraisons.**

---

## 📑 **Structure de la Base de Données**

### 🛠️ **1. Table `Livreurs`**

| **Champ**    | **Type**   | **Contraintes**               | **Description**                     |
|--------------|------------|--------------------------------|-------------------------------------|
| `id`         | INT        | PRIMARY KEY, AUTO_INCREMENT    | Identifiant unique du livreur       |
| `mail`       | VARCHAR    | UNIQUE, NOT NULL              | Adresse e-mail du livreur           |
| `mdp`        | VARCHAR    | NOT NULL                      | Mot de passe du livreur             |
| `nom`        | VARCHAR    |                                | Nom de famille du livreur           |
| `prenom`     | VARCHAR    |                                | Prénom du livreur                   |
| `telephone`  | VARCHAR    | NULL                           | Numéro de téléphone du livreur      |

---

### 🛠️ **2. Table `Entreprises`**

| **Champ**    | **Type**   | **Contraintes**               | **Description**                     |
|--------------|------------|--------------------------------|-------------------------------------|
| `id`         | INT        | PRIMARY KEY, AUTO_INCREMENT    | Identifiant unique de l'entreprise  |
| `nom`        | VARCHAR    | NOT NULL                      | Nom de l'entreprise                 |
| `adresse`    | VARCHAR    | NULL                           | Adresse de l'entreprise             |

---

### 🛠️ **3. Table `Paniers`**

| **Champ**         | **Type**    | **Contraintes**             | **Description**                     |
|-------------------|-------------|----------------------------|-------------------------------------|
| `id`              | INT         | PRIMARY KEY, AUTO_INCREMENT | Identifiant unique du panier        |
| `entreprise_id`   | INT         | NOT NULL                    | Référence à l'entreprise            |
| `date_creation`   | DATE        | NOT NULL                    | Date de création du panier          |
| `montant_total`   | DECIMAL     | NULL                        | Montant total du panier             |

🔗 Clé étrangère :  
- `entreprise_id` référence `id` dans `Entreprises`.

---

### 🛠️ **4. Table `Produits`**

| **Champ**    | **Type**   | **Contraintes**               | **Description**                     |
|--------------|------------|--------------------------------|-------------------------------------|
| `id`         | INT        | PRIMARY KEY, AUTO_INCREMENT    | Identifiant unique du produit       |
| `libelle`    | VARCHAR    | NOT NULL                      | Nom ou description du produit       |

---

### 🛠️ **5. Table `Panier_Produits`**

| **Champ**     | **Type**   | **Contraintes**               | **Description**                     |
|---------------|------------|--------------------------------|-------------------------------------|
| `id`          | INT        | PRIMARY KEY, AUTO_INCREMENT    | Identifiant unique de la liaison    |
| `panier_id`   | INT        | NOT NULL                      | Référence au panier                 |
| `produit_id`  | INT        | NOT NULL                      | Référence au produit                |
| `quantite`    | INT        | NOT NULL                      | Quantité du produit dans le panier  |
| `statut`      | VARCHAR    | DEFAULT 'En stock'            | Statut du produit (par défaut : "En stock") |

🔗 Clés étrangères :  
- `panier_id` référence `id` dans `Paniers`.  
- `produit_id` référence `id` dans `Produits`.

---

### 🛠️ **6. Table `Assignations`**

| **Champ**       | **Type**   | **Contraintes**               | **Description**                     |
|-----------------|------------|--------------------------------|-------------------------------------|
| `id`            | INT        | PRIMARY KEY, AUTO_INCREMENT    | Identifiant unique de l'assignation |
| `livreur_id`    | INT        | NOT NULL                      | Référence au livreur                |
| `entreprise_id` | INT        | NOT NULL                      | Référence à l'entreprise            |

🔗 Clés étrangères :  
- `livreur_id` référence `id` dans `Livreurs`.  
- `entreprise_id` référence `id` dans `Entreprises`.

---

### 🛠️ **7. Table `Livraisons`**

| **Champ**         | **Type**    | **Contraintes**             | **Description**                     |
|-------------------|-------------|----------------------------|-------------------------------------|
| `id`              | INT         | PRIMARY KEY, AUTO_INCREMENT | Identifiant unique de la livraison  |
| `panier_id`       | INT         | NOT NULL                    | Référence au panier livré           |
| `livreur_id`      | INT         | NOT NULL                    | Référence au livreur                |
| `date_livraison`  | DATE        | NOT NULL                    | Date de la livraison                |
| `statut`          | VARCHAR     | DEFAULT 'En attente'        | Statut de la livraison              |
| `commentaire`     | TEXT        | NULL                        | Commentaire éventuel sur la livraison |

🔗 Clés étrangères :  
- `panier_id` référence `id` dans `Paniers`.  
- `livreur_id` référence `id` dans `Livreurs`.

---

## 🔗 **Relations Clés Étrangères**

1. **`Paniers.entreprise_id` → `Entreprises.id`**  
   Chaque panier est associé à une entreprise spécifique.

2. **`Panier_Produits.panier_id` → `Paniers.id`**  
   Chaque produit est lié à un panier.

3. **`Panier_Produits.produit_id` → `Produits.id`**  
   Chaque produit lié à un panier doit être référencé dans la table des produits.

4. **`Assignations.livreur_id` → `Livreurs.id`**  
   Chaque assignation associe un livreur à une entreprise.

5. **`Assignations.entreprise_id` → `Entreprises.id`**  
   Chaque assignation lie une entreprise à un livreur spécifique.

6. **`Livraisons.panier_id` → `Paniers.id`**  
   Chaque livraison est liée à un panier spécifique.

7. **`Livraisons.livreur_id` → `Livreurs.id`**  
   Chaque livraison est effectuée par un livreur spécifique.

---

## ✅ **Fonctionnalités Principales :**

1. **Gestion des livreurs** : Création, mise à jour et suppression des livreurs.  
2. **Gestion des entreprises** : Enregistrement des entreprises clientes.  
3. **Création de paniers** : Ajout de produits et calcul du montant total.  
4. **Assignation de paniers aux livreurs** : Suivi des livraisons et des statuts.  
5. **Historique des livraisons** : Suivi des livraisons avec commentaires.

