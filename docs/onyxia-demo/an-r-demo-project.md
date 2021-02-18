---
description: An R demo project for datalab.sspcloud.fr.
---

# An R demo project

An R demo project for [datalab.sspcloud.fr](https://datalab.sspcloud.fr/).

## Environment setup

* create an RStudio service [![](https://camo.githubusercontent.com/a6aab12af08bd4aaa6cba643681ab06877f0b1a6120f48b6d880af6d0b5bc1d4/68747470733a2f2f696d672e736869656c64732e696f2f62616467652f535350436c6f75642d5253747564696f2d253233373661626464)](https://datalab.sspcloud.fr/my-lab/catalogue/inseefrlab-helm-charts-datascience/rstudio/deploiement)
* log in with the following credentials:
  * user: `rstudio`
  * password: your services password \(you can find it on this page [https://datalab.sspcloud.fr/mon-compte](https://datalab.sspcloud.fr/mon-compte)\)
* in RStudio, clone and open this project

  ```text
  git2r::clone("https://github.com/RLesur/sspcloud-demo.git", "sspcloud-demo")
  rstudioapi::openProject("sspcloud-demo")
  ```

* install the dependencies declared in the `DESCRIPTION` file  _you should declare your dependencies in the `DESCRIPTION` file!_

## Utilisation du système de stockage

### Cheatsheet

**Trouver le nom de son** _**bucket**_ **personnel**

Chaque utilisateur du SSP Cloud dispose d'un espace de stockage personnel sur le système de stockage du SSP Cloud. Ces espaces de stockage s'appellent des _buckets_.

Pour trouver le nom de son espace personnel de stockage, on peut se rendre sur la page [https://datalab.sspcloud.fr/mes-fichiers](https://datalab.sspcloud.fr/mes-fichiers). On trouve alors le nom de son **bucket personnel**.

**Télécharger un fichier**

```text
aws.s3::save_object(object, bucket, region = "")
```

**Uploader un fichier**

```text
aws.s3::put_object(file, object, bucket, region = "")
```

### Exemple \#1 : uploader un rapport R Markdown

Le fichier `s3.Rmd` présente les commandes de base pour gérer ses fichiers cloud

* générer le rapport

  ```text
  # Modifier le bucket ci-dessous (renseignez votre bucket)
  bucket <- "f7sggu"

  rmarkdown::render("s3.Rmd", params = list(bucket = bucket), output_dir = "out")
  ```

* uploader le rapport

### Exemple \#2 : Travailler avec des données stockées sur MinIO

Un exemple minimal est présent dans le fichier [datasaurus.R](https://github.com/RLesur/sspcloud-demo/blob/main/datasaurus.R) :

* les données du _datasaurus_ sont publiquement disponibles à cettre adresse [https://minio.lab.sspcloud.fr/f7sggu/sspcloud-demo/data/datasaurus.csv](https://minio.lab.sspcloud.fr/f7sggu/sspcloud-demo/data/datasaurus.csv)
* on construit un graphique ggplot qu'on sauvegarde en `png` et qu'on uploade sur MinIO

Pour exécuter ce pipeline :

