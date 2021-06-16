---
title: Utiliser R avec le SSP Cloud
description: Une courte démonstration pour lancer un projet R avec le datalab SSP Cloud
keywords: >-
  getting started, account, Subscription, Pay-As-You-Go, enterprise, catalog,
  upgrade account, IAM, access groups, invite users, notifications, email
  preferences, account settings, MFA, authetication, TOTP, U2F, FIDO U2F,
  security key
---

# Lancer un projet R avec le datalab

Une courte démonstration pour utiliser R avec le [datalab SSP Cloud](https://datalab.sspcloud.fr): travailler avec du code Git et des données hébergées sur [Mes Fichiers](https://datalab.sspcloud.fr/mes-fichiers) du datalab.

## Configuration de l'environnement

* Ouvrir le [catalogue de service](https://datalab.sspcloud.fr/my-lab/catalogue/inseefrlab-helm-charts-datascience)
* Lancer et ouvrir un service RStudio
* Se connecter avec les identifiants suivants:
  * user: `rstudio`
  * password: `Votre mot de passe pour vos services` 

    Vous pouvez le trouver sur votre [compte](https://datalab.sspcloud.fr/mon-compte)  ou le copier depuis directement [Mes services](https://datalab.sspcloud.fr/my-services) en cliquant sur la clef
* Dans votre RStudio, cloner et ouvrer ce projet:

  ```text
  git2r::clone("https://github.com/RLesur/sspcloud-demo.git", "sspcloud-demo")
  rstudioapi::openProject("sspcloud-demo")
  ```

* Installer les dépendances déclarées dans le fichier `DESCRIPTION`

## Utilisation du système de stockage

### Cheatsheet \(Antisèche\)

**Trouver le nom de son** _**bucket**_ **personnel**

Chaque utilisateur du SSP Cloud dispose d'un espace de stockage personnel sur le système de stockage du SSP Cloud. Ces espaces de stockage s'appellent des _buckets_.

Pour trouver le nom de son espace personnel de stockage, on peut se rendre sur la page [mes fichiers](https://datalab.sspcloud.fr/mes-fichiers). On trouve alors le nom de son **bucket personnel**.

**Télécharger un fichier**

```text
aws.s3::save_object(object, bucket, region = "")
```

**Importer un fichier**

```text
aws.s3::put_object(file, object, bucket, region = "")
```

### Exemple \#1 : Importer un rapport R Markdown

Le fichier `s3.Rmd` présente les commandes de base pour gérer ses fichiers cloud

* générer le rapport

  ```text
  # Modifier le bucket ci-dessous (renseignez votre bucket)
  bucket <- "f7sggu"

  rmarkdown::render("s3.Rmd", params = list(bucket = bucket), output_dir = "out")
  ```

* Importer le rapport

  ```text
  source("_upload.R")
  ```

### Exemple \#2 : Travailler avec des données stockées sur MinIO

Un exemple minimal est présent dans le fichier [datasaurus.R](https://github.com/RLesur/sspcloud-demo/blob/main/datasaurus.R) :

* les données du _datasaurus_ sont publiquement disponibles à cette adresse [https://minio.lab.sspcloud.fr/f7sggu/sspcloud-demo/data/datasaurus.csv](https://minio.lab.sspcloud.fr/f7sggu/sspcloud-demo/data/datasaurus.csv)
* on construit un graphique ggplot qu'on sauvegarde en `png` et qu'on uploade sur MinIO

Pour exécuter ce pipeline :

```text
source("datasaurus.R")
```



