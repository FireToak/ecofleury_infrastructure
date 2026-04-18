# EcoFleury - Infrastructure as Code (IaC)

> ⚠️ Ce dépôt est lié au contexte d'entreprise détaillé dans le fichier [contexte.md](./contexte.md).

Ce dépôt contient l'automatisation de l'infrastructure pour le projet **EcoFleury**. Il provisionne, via Vagrant, l'environnement nécessaire à la mise en place d'un pipeline d'intégration et de déploiement continus (CI/CD) avec Jenkins, en respectant les standards de cybersécurité et de nommage.

---

## 🏗️ Architecture de l'Infrastructure

Les machines virtuelles déployées simulent une architecture d'entreprise segmentée :

* **`ci-prod-fr-01`** : Serveur d'orchestration Jenkins (Maître).
* **`web-dev-fr-01`** : Serveur web de Staging (Pré-production / Recette).
* **`web-prod-fr-01`** : Serveur web de Production.

---

## 📂 Organisation du dépôt

```text
.ecofleury_infrastructure
├── contexte.md      # Détails du cas d'étude métier et technique
├── README.md        # Documentation principale du dépôt
├── secret.md        # (Attention : les secrets ne doivent jamais être commités en production réel)
└── vagrant/         # Dossier contenant l'automatisation de l'infrastructure
    └── Vagrantfile  # Fichier de configuration pour le provisionnement des VMs
```

---

## 🚀 Utilisation et Déploiement

Prérequis : Avoir installé [Vagrant](https://www.google.com/search?q=https://developer.hashicorp.com/vagrant/downloads) et un hyperviseur (ex: VirtualBox).

1. **Cloner le dépôt sur votre machine**

```bash
git clone https://github.com/FireToak/ecofleury_infrastructure.git
```

  * **`git clone`** : Télécharge une copie locale complète du dépôt distant.

2. **Accéder au dossier de provisionnement**

```bash
cd vagrant
```

  * **`cd`** (Change Directory) : Permet de naviguer dans le dossier cible contenant le fichier `Vagrantfile`.

3. **Lancer le provisionnement de l'infrastructure**

```bash
vagrant up
```

  * **`vagrant up`** : Lit le fichier `Vagrantfile`, télécharge les images systèmes nécessaires, crée les machines virtuelles, configure le réseau et exécute les scripts d'installation.

---

## 📚 Ressources et Documentation

  * [Documentation sur Vagrant - Stephane Robert](https://blog.stephane-robert.info/docs/infra-as-code/provisionnement/vagrant/)
  * [Documentation installation de JDK 21 (Prérequis Jenkins)](https://www.tecmint.com/install-java-on-debian-12/)
  * [Documentation installation de Jenkins](https://www.jenkins.io/doc/book/installing/linux/)

---

## Mainteneurs

**Louis MEDO** | [Linkedin](https://www.linkedin.com/in/louismedo/) | [Portfolio](https://louis.loutik.fr/) | [GitHub](https://github.com/FireToak)

---

<div align="center">
  <br/>
  <small><i>Dernière mise à jour : 12 avril 2026</i></small>
</div>
