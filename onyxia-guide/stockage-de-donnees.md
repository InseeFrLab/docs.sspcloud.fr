---
description: Stocker des données et les utiliser dans des services sur le Datalab
---

# Stockage de données

## Principes

La solution de stockage de fichiers associée au Datalab est [MinIO](https://min.io), un système de stockage d'objets basé sur le cloud, compatible avec l'API S3 d'Amazon. Concrètement, cela a plusieurs avantages :&#x20;

* les fichiers stockés sont accessibles facilement et à n'importe quel endroit : un fichier est accessible directement via une simple URL, qui peut être partagée ;
* il est possible d'accéder aux fichiers stockés directement dans les services de _data science_ (R, Python...) proposés sur le Datalab, sans avoir besoin de copier les fichiers localement au préalable, ce qui améliore fortement la reproductibilité des analyses.

## Gérer ses données

### Importer des données

La page [Mes fichiers](https://datalab.sspcloud.fr/mes-fichiers/) du Datalab prend la forme d'un explorateur de fichiers présentant les différents _buckets_ (dépôts) auxquels l’utilisateur a accès.&#x20;

Chaque utilisateur dispose par défaut d'un _bucket_ personnel pour stocker ses fichiers. Au sein de ce _bucket_, deux options sont possibles :

* "**créer un répertoire**" : crée un répertoire dans le _bucket_/répertoire courant, de manière hiérarchique, comme dans un système de fichiers traditionnel ;
* "_**uploader**_** un fichier**" : _upload_ un ou plusieurs fichiers dans le répertoire courant.

{% hint style="info" %}
L'interface graphique du stockage de données sur le Datalab est encore en cours de construction. Elle peut à ce titre présenter des problèmes de réactivité. Pour des opérations fréquentes sur le stockage de fichiers, il peut être préférable d'interagir avec MinIO via le terminal.
{% endhint %}

### Partager des données

En cliquant sur un fichier dans son _bucket_ personnel, on accède à sa page de caractéristiques. Sur celle-ci, il est notamment possible de **changer le statut de diffusion du fichier**. Changer le statut du fichier de "privé" à "public" permet d'obtenir un **lien de diffusion**, qui peut alors être transmis pour téléchargement du fichier. Le statut "public" ne donne aux autres utilisateurs que des droits en lecture, la modification ou la suppression de fichiers personnels par d'autres utilisateurs est impossible.

Pour simplifier la mise à disposition en lecture de plusieurs fichiers — dans le cadre d'une formation par exemple — il est possible de créer un **dossier "diffusion"** dans son _bucket_ personnel. Par défaut, tous les fichiers présents dans ce dossier ont un statut de diffusion public.

{% hint style="info" %}
Dans le cadre de projets collaboratifs, il peut être intéressant pour les différents participants d'avoir accès à un espace de stockage commun. Il est possible pour cet usage de créer des _buckets_ partagés sur MinIO. N'hésitez pas à nous contacter via les canaux précisés sur la page "[Première utilisation](premiere-utilisation.md)" si vous souhaitez porter des projets _open-data_ sur le Datalab.
{% endhint %}

{% hint style="warning" %}
Conformément aux [conditions d'utilisation](https://www.sspcloud.fr/tos\_fr.md), seuls des données de type _open data_ ou ne présentant aucune sensibilité peuvent être stockées sur le Datalab. Le fait qu'un fichier ait un statut de diffusion "privé" ne suffit pas à garantir une parfaite confidentialité.
{% endhint %}

## Utiliser des données dans un service

Les identifiants d'accès nécessaires pour accéder à des données sur MinIO sont pré-configurés dans les différents services du Datalab, accessibles sous la forme de [variables d'environnement](gestion-des-secrets.md). Ainsi, l'import et l'export de fichiers à partir des services est grandement facilité.

{% hint style="warning" %}
L'accès au stockage MinIO est possible via un _token_ (jeton d'accès) personnel, valide 7 jours, et automatiquement régénéré à échéances régulières sur le SSP Cloud. Lorsqu'un token a expiré, les services créés avant la date d'expiration (avec le précédent token) ne peuvent plus accéder au stockage ; le service concerné apparaît alors marqué en rouge dans la page [Mes Services](https://datalab.sspcloud.fr/my-services). Il est donc primordial de renouveler régulièrement les services en cours d'exécution, en prenant soin de sauvegarder au préalable son code et ses données.
{% endhint %}

### Avec R Studio

La section de la documentation [utilitR ](https://www.book.utilitr.org)dédiée à l'[usage de RStudio sur l'environnement SSP Cloud ](https://www.book.utilitr.org/sspcloud.html#utiliser-des-donn%C3%A9es-stock%C3%A9es-sur-le-syst%C3%A8me-de-stockage-s3)présente en détails comment lister les fichiers d'un bucket, importer et exporter des données à l'aide de la librairie `aws.s3` de R.

### En Python (avec Jupyter ou VSCode)

En Python, l'interaction avec un système de fichiers compatible S3 est rendu possible par deux librairies :&#x20;

* [Boto3](https://boto3.amazonaws.com/v1/documentation/api/latest/index.html), une librairie créée et maintenue par Amazon ;
* [S3Fs](https://s3fs.readthedocs.io/en/latest/), une librairie qui permet d'interagir avec les fichiers stockés à l'instar d'un _filesystem_ classique.

Pour cette raison et parce que S3Fs est utilisée par défaut par la librairie [pandas ](https://pandas.pydata.org)pour gérer les connections S3, nous allons présenter la gestion du stockage sur MinIO via Python à travers cette librairie.

```python
import os

import s3fs
import pandas
```

```python
# Create filesystem object
S3_ENDPOINT_URL = "https://" + os.environ["AWS_S3_ENDPOINT"]
fs = s3fs.S3FileSystem(client_kwargs={'endpoint_url': S3_ENDPOINT_URL})
```

**Lister les fichiers d'un **_**bucket**_**.**

```python
BUCKET = "donnees-insee"
fs.ls(BUCKET)
```

**Importer des données dans Python.**

Le package S3Fs permet d'interagir avec les fichiers stockés sur MinIO comme s'il s'agissait de fichiers locaux. La syntaxe est donc très familière pour les utilisateurs de Python. Par exemple, pour importer/exporter des données tabulaires via pandas :

```python
BUCKET = "donnees-insee"
FILE_KEY_S3 = "BPE/2019/BPE_ENS.csv"
FILE_PATH_S3 = BUCKET + "/" + FILE_KEY_S3

with fs.open(FILE_PATH_S3, mode="rb") as file_in:
    df_bpe = pd.read_csv(file_in, sep=";")
```

**Exporter des données vers MinIO.**

```python
BUCKET_OUT = "<mon_bucket>"
FILE_KEY_OUT_S3 = "mon_dossier/BPE_ENS.csv"
FILE_PATH_OUT_S3 = BUCKET_OUT + "/" + FILE_KEY_OUT_S3

with fs.open(FILE_PATH_OUT_S3, 'w') as file_out:
    df_bpe.to_csv(file_out)
```

{% hint style="warning" %}
S3Fs propose également des fonctions pour télécharger/exporter des fichiers à partir du _filesystem_ local de la session Python. Cependant, **copier les fichiers en local dans le service n'est généralement pas une bonne pratique** : cela limite la reproductibilité des analyses, et devient rapidement impossible avec des volumes importants de données. Il est donc préférable de prendre l'habitude d'importer les données comme des fichiers directement dans Python.
{% endhint %}

### Le client MinIO

MinIO propose un client en ligne de commande qui permet d’interagir avec le système de stockage à la manière d'un _filesystem_ UNIX classique. Ce client est installé par défaut et accessible via un terminal dans les différents services du Datalab.

Le stockage du Datalab est accessible via l'alias `s3`. Par exemple, pour lister les fichiers de son _bucket_ personnel :&#x20;

```bash
mc ls s3/mon-bucket-perso
```

{% hint style="info" %}
Le client MinIO propose les commandes UNIX de base, telles que ls, cat, cp, etc. La liste complète est disponible dans la [documentation du client](https://docs.min.io/docs/minio-client-complete-guide.html).
{% endhint %}

{% hint style="warning" %}
La manipulation de fichiers volumineux sur MinIO est une opération coûteuse car les fichiers stockés sont immuables. Ainsi, renommer ou déplacer un fichier implique une suppression du fichier original et une recréation du fichier avec de nouvelles métadonnées. Il est donc important de réfléchir en amont à la structure de données des projets.
{% endhint %}
