---
description: >-
  Utiliser MLFlow et les pokémons pour découvrir le MLOps et automatiser puis
  améliorer la qualité des modèles de production.
---

# MLOps et Random Forest Clustering : Démonstration de MLFlow avec les statistiques des Pokémons

## La démonstration en quelques mots

Cette démonstration est un modèle Random Forest \(Forêt d’arbre de décision\) pour classer les Pokémon. C'est-à-dire savoir s'ils sont légendaires ou non. Ensuite, nous appliquerons ce modèle dans une application Web et fournirons une API de service Web. L'objectif de cette démonstration est de lister les caractéristiques des Pokémons \(attaque, défense,  vitesse, les points de vie ou points de combats, etc.\) et d'entraîner un modèle de Random Forest afin de classer et prédire si un Pokémon est légendaire. Nous voulons aussi définir l'importance de chaque caractéristique dans la distinction d'un Pokémon légendaire.  Pour finir, nous appliquerons ce modèle dans une application Web et fournirons une API de service Web, afin que chacun puisse tester et vérifier si son Pokémon est légendaire.

## L'usage du datalab 

Ce projet est à l'origine développé en Python pur sans aucun support de ML Ops. Il est donc difficile de poursuivre l'intégration \(CI\), de poursuivre le déploiement \(CD\) et de poursuivre l'entrainement du modèle \(CT\). En migrant ce projet vers le datalab, nous avons pu profiter de la gestion des données et du service ML Ops fournis par la plateforme. Nous voulons mettre en évidence certains avantages : 

1. Amélioration significative des performances du modèle
2. Raccourcissement du cycle de vie de développement du modèle 
3. Automatiser le processus de déploiement du modèle

#### ML Ops avec Datalab

Lorsque vous travaillez en équipe pour développer un modèle, vous devez faire l'expérience douloureuse de la **journalisation** du modèle et de la façon dont le modèle est formé \(par exemple, les réglages d'hyper-paramètres, l'emplacement du jeu de données d'entraînement, etc.\) . Nous pouvons facilement appliquer un développement continu sur notre code de modèle en utilisant des outils tels que **GitHub**.  Nous savons que le code est un facteur qui peut avoir un impact sur les performances d'un modèle. Mais la plupart du temps, le réglage des hyper-paramètres et l'ensemble de données d'entraînement ont encore plus d'impact que le code sur le modèle. Le datalab fournit un service appelé **MLflow** qui résout tous ces problèmes.

**Suivi de modèle**

Pour chaque modèle entraîné, on peut suivre l'importance des caractéristiques des Pokémons, les configurations d'hyper-paramètres, l'emplacement des données d'entraînement, leurs métriques de validation \(précision, ROC, AUC, etc.\). Nous pouvons également comparer différents modèles pour l'améliorer et expliquer quel paramètre rend cette amélioration possible.

![Figure 1 - Suivi de mod&#xE8;le](https://minio.lab.sspcloud.fr/pengfei/diffusion/pokemon/pokemon_metric.PNG)

![Figure 2 - Comparaison de mod&#xE8;le](https://minio.lab.sspcloud.fr/pengfei/diffusion/pokemon/pokemon_mdoel_camparing.PNG)

**Déploiement de modèle**

Pour déployer un modèle, nous devons définir la version et l'état du modèle, et l'application de déploiement peut récupérer le modèle approprié en fonction de ces informations. 

![Figure 3 - D&#xE9;ploiement de mod&#xE8;le](https://minio.lab.sspcloud.fr/pengfei/diffusion/pokemon/model_version.PNG)

Dans l'exemple, notre modèle a quatre versions : une en production, une en développement et deux en archive.

#### Service de gestion des données au sein de Datalab

Chaque année, la société Pokémon publie de nouveaux types de Pokémons. Chaque version est appelée une génération. Jusqu'à présent, nous avons sept générations. En conséquence, les données brutes de Pokémons sont divisées en différents fichiers pour chaque génération. Dans de nombreux cas, ceux qui collectent et téléchargent les données ne sont pas ceux qui les utilisent pour entraîner le modèle. Par conséquent, c'est un nouveau défi de trouver les données appropriées pour entraîner votre modèle. Heureusement, le datalab nous fournit un service de gestion de données appelé Atlas qui nous permet de trouver des données facilement.

We can search data by their name, type, owner, etc. Figure-4 shows an example of full text search.  

Nous pouvons rechercher des données par leur nom, type, propriétaire, etc.

![Figure 4 - Exemple de recherche textuelle \(Atlas\)](https://minio.lab.sspcloud.fr/pengfei/diffusion/pokemon/atlas_search_by_text.PNG)

Si le gestionnaire de données a configuré les métadonnées de classification, nous pouvons même rechercher des données par génération de Pokémons. 

![Figure 5 - Exemple de recherche par g&#xE9;n&#xE9;ration de pok&#xE9;mons \(Atlas\) ](https://minio.lab.sspcloud.fr/pengfei/diffusion/pokemon/atlas_search_by_class.png)

Après avoir trouvé les données, nous pouvons accéder à toutes les métadonnées liées telles que le nom, l'emplacement, le propriétaire, la taille, la date de création, etc.  

![Figure 6 - Exemple de metadonn&#xE9;es pour le jeu de donn&#xE9;es sur les Pok&#xE9;mons \(Atlas\)](https://minio.lab.sspcloud.fr/pengfei/diffusion/pokemon/atlas_data_detail.PNG)

## Liens utiles \(Données et modèle\)

L'ensemble de données nettoyé complet est disponible ici:

{% embed url="https://minio.lab.sspcloud.fr/pengfei/mlflow-demo/pokemon-cleaned.csv" %}

Le codes source sur la façon de d'entrainer, suivre et déployer le modèle sont disponible ici : 

{% embed url="https://github.com/pengfei99/mlflow-pokemon-example.git" %}

Pour utiliser le modèle le plus récemment déployé, vous pouvez utiliser la commande curl suivante pour interroger notre service Web :

```text
curl -X POST -H "Content-Type:application/json; format=pandas-split" --data \
'{"columns":["hp","attack","defense","special_attack","special_defense","speed"],"index":[272,293,414,263,49],
"data":[[80,70,70,90,100,70],[64,51,23,51,23,28],[70,94,50,94,50,66],[38,30,41,30,41,60],[70,65,60,90,75,90]]}' \
https://pokemon.lab.sspcloud.fr/invocations ;
```

