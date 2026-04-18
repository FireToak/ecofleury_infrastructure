# Contexte - EcoFleury

L'entreprise EcoFleury est une société spécialisée dans le commerce en ligne de fleurs et de bouquets issus de l'agriculture biologique et locale. Son modèle commercial repose sur des collections variant au gré des saisons. L'objectif actuel du département informatique est de fiabiliser les mises à jour de la plateforme web en instaurant une démarche DevOps stricte. La priorité est de mettre en place une intégration et un déploiement continus (CI/CD) permettant d'ajouter de nouvelles fonctionnalités sans interruption de service ni régression.

---

## Environnement technique

L'infrastructure repose sur une architecture segmentée pour séparer les environnements de développement, de test et de production. 

* **ecofleury_website :** Dépôt distant hébergeant le code source de l'application et configuré pour envoyer des webhooks lors des modifications du code.
* **ci-prod-fr-01:** Serveur d'orchestration central. Il intercepte les webhooks, exécute les tests automatisés et orchestre le déploiement vers les serveurs cibles.
* **web-dev-fr-01** Serveur de pré-production (recette). Il s'agit d'une copie exacte de l'environnement de production, utilisée pour valider le bon fonctionnement de l'application après le passage réussi du pipeline de CI.
* **web-prod-fr-01** Serveur hébergeant l'application accessible aux clients finaux. Le déploiement sur ce serveur est conditionné par une validation manuelle après les tests sur le serveur Staging.

**Règle de nommage :**

[Fonction]-[Environnement]-[Localisation]-[Numéro]

---

## GitFlow du contexte

Le cycle de développement adopte un modèle de branche standard (GitFlow simplifié) pour garantir un code stable en production :

1.  **Branche `main` :** Représente l'état stable de la production. Aucun développement n'y est fait directement.
2.  **Branche `develop` :** Environnement d'intégration. Elle centralise les nouvelles fonctionnalités validées.
3.  **Branches `feature/*` :** Créées depuis `develop` pour chaque nouvelle fonctionnalité développée.

**Validation CI/CD :** La création d'une *Pull Request* (PR) d'une branche `feature` vers `develop` déclenche automatiquement le pipeline Jenkins (tests unitaires). Si les tests réussissent, la PR peut être fusionnée, déclenchant un déploiement automatisé sur le serveur **web-dev-fr-01**. Le passage en production s'effectue par la suite via une fusion contrôlée de `develop` vers `main`.

---

## Road-map des fonctionnalités

Les fonctionnalités suivantes sont prévues dans le backlog. Elles sont conçues spécifiquement pour valider les différentes étapes de l'intégration continue.

| Fonctionnalité | Description technique | Enjeu pour les tests (Pipeline Jenkins) |
| :--- | :--- | :--- |
| **Calculateur d'empreinte carbone** | Algorithme calculant l'impact selon la distance client/producteur. | Vérification stricte via des **tests unitaires** mathématiques (logique métier). |
| **Gestionnaire de saisons** | Ajout d'une balise "saison" sur les bouquets existants. | Validation des requêtes et de la structure de la base de données. |
| **Bannière "Promo Flash"** | Apparition dynamique d'un bandeau de réduction sur l'accueil. | Vérification du rendu de l'interface et du non-blocage des éléments existants. |

---

## Fonctionnalités du site en production

L'application de base est volontairement minimaliste pour concentrer l'effort sur l'automatisation de l'infrastructure (CI/CD) plutôt que sur la complexité logicielle. 

**Technologies utilisées :**
* **Backend :** PHP (pur ou via un micro-framework très léger comme Slim/Lumen) pour faciliter l'écriture de tests avec PHPUnit.
* **Frontend :** HTML/CSS basique, sans framework Javascript lourd.
* **Base de données :** SQLite. L'utilisation d'un simple fichier de base de données évite la configuration d'un serveur MySQL complexe et facilite la réinitialisation des données lors des tests automatisés dans Jenkins.

**Base de données (Schéma initial) :**
* Table `produits` : `id`, `nom`, `description`, `prix`, `stock`.
* Table `commandes` : `id`, `produit_id`, `quantite`, `date`.

**Fonctionnalités actuelles :**
1.  **Catalogue :** Une page d'accueil affichant la liste des bouquets disponibles, lue depuis la base de données.
2.  **Détails :** Une page présentant la description d'un bouquet spécifique et son stock actuel.
3.  **Achat basique :** Un bouton de commande qui décrémente directement le stock dans la base de données (sans gestion complexe de panier ou de paiement en ligne).

Cette base simple permet de mettre en place rapidement des tests unitaires pertinents (par exemple : vérifier qu'une commande ne peut pas être passée si le stock est inférieur ou égal à zéro) qui seront exécutés par Jenkins à chaque modification du code.