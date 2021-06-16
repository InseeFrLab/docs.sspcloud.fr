# Pokemon ClassificationFR

## Le projet en quelques mots

Ce projet est un modèle Random Forest \(Forêt d’arbre de décision\) pour classer les Pokémon \(c'est-à-dire légendaires, non légendaires\). Ensuite, nous appliquons le modèle dans une application Web et fournissons une API de service Web. L'idée est de lister les caractéristiques des pokémons \(attaque, défense,  vitesse, les points de vie ou points de combats, etc.\) et d'entraîner un modèle de RandomForest afin de classer et prédire si un pokémon est légendaire ou non. Et nous voulons aussi les poids ou l'importance de chaque caractéristique dans la distinction d'un pokémon légendaire. Puis pour mettre ce modèle en production, nous proposons un service web, afin que chacun puisse tester si son pokémon est légendaire ou non.

## L'usage du datalab 

Ce projet est à l'origine développé en python pur sans aucun support de ML Ops. Il est donc difficile de poursuivre l'intégration \(CI\), de poursuivre le déploiement \(CD\) et de poursuivre la formation \(CT\). En migrant ce projet vers le datalab, nous avons pu profiter de la gestion des données et du service ML Ops fournis par la plateforme. Nous voulons mettre en évidence certains avantages : 

1. Amélioration significative des performances du modèle
2. Raccourcissement du cycle de vie de développement du modèle 
3. Automatiser le processus de déploiement du modèle

#### ML Ops avec Datalab

Lorsque vous travaillez en équipe pour développer un modèle, vous devez faire l'expérience douloureuse de la **journalisation** du modèle et de la façon dont le modèle est formé \(par exemple, les paramètres d'hyper-paramètres, l'emplacement du jeu de données d'entraînement, etc.\) . Nous pouvons facilement appliquer un développement continu sur notre code de modèle en utilisant des outils tels que **GitHub**. Mais nous savons que le code est qu'un facteur qui peut avoir un impact sur les performances de votre modèle. Mais la plupart du temps, le réglage des hyper-paramètres et l'ensemble de données d'entraînement ont encore plus d'impact que le code sur le modèle. Le datalab fournit un service appelé **MLflow** qui résout tous ces problèmes.

**Suivi de modèle**

Pour chaque modèle entraîné, on peut suivre l'importance des caractéristiques des pokémons, les configurations d'hyper-paramètres, l'emplacement des données d'entraînement, leurs métriques de validation \(précision, ROC, AUC, etc.\). Nous pouvons également comparer différents modèles pour l'améliorer et expliquer quel paramètre rend cette amélioration possible.

![Suivi de mod&#xE8;le](https://minio.lab.sspcloud.fr/pengfei/diffusion/pokemon/pokemon_metric.PNG)

![Comparaison de mod&#xE8;le](https://minio.lab.sspcloud.fr/pengfei/diffusion/pokemon/pokemon_mdoel_camparing.PNG)

**Déploiement de modèle**

Pour déployer un modèle, nous devons définir la version et l'état du modèle, et l'application de déploiement peut récupérer le modèle approprié en fonction de ces informations. 

![D&#xE9;ploiement de mod&#xE8;le](https://minio.lab.sspcloud.fr/pengfei/diffusion/pokemon/model_version.PNG)

Dans l'exemple, notre modèle a quatre versions : une en production, une en développement et deux en archive.

#### Data management service inside Datalab

Each year, the pokemon company will release some new types of pokemon. Each release is called a generation. So far, we have seven generations. As a result, the raw data of pokemon are split into different files for each generation. In many cases, those who collect and upload the data are not the one who use them to train the model. As a result, it's a new challenge to find the appropriate data to train your model. Fortunately , Datalab provide us a data management service called Atlas which allow us to find data easily.

We can search data by their name, type, owner, etc. Figure-4 shows an example of full text search.  

![](https://minio.lab.sspcloud.fr/pengfei/diffusion/pokemon/atlas_search_by_text.PNG)

If the data steward has configured the classification meta-data, we can even search data by the pokemon generation. Figure-5 shows an example of search by pokemon generation.  **Figure-5:** - Search by pokemon generation

![](https://minio.lab.sspcloud.fr/pengfei/diffusion/pokemon/atlas_search_by_class.png)

After we found the data, we can know all the metadata about the data such as name, location, owner, size, creation date, etc. Figure-6 shows an example of the pokemon data metadata. ![](https://minio.lab.sspcloud.fr/pengfei/diffusion/pokemon/atlas_data_detail.PNG)

**Figure-6:** - Pokemon metadata

## Data and model

The full cleaned dataset can be found

The source codes on how to train, track and deploy model can be found [here](https://minio.lab.sspcloud.fr/pengfei/mlflow-demo/pokemon-cleaned.csv) \([https://github.com/pengfei99/mlflow-pokemon-example.git](https://github.com/pengfei99/mlflow-pokemon-example.git)\)

To use our newly deployed model, you can use the following curl command to query our web service

```text
curl -X POST -H "Content-Type:application/json; format=pandas-split" --data \
'{"columns":["hp","attack","defense","special_attack","special_defense","speed"],"index":[272,293,414,263,49],
"data":[[80,70,70,90,100,70],[64,51,23,51,23,28],[70,94,50,94,50,66],[38,30,41,30,41,60],[70,65,60,90,75,90]]}' \
https://pokemon.lab.sspcloud.fr/invocations ;
```

