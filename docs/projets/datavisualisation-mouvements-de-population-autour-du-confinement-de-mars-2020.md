---
description: >-
  Ce projet documente les mouvements de population autour du confinement de mars
  2020 à partir d'indicateurs anonymes issus de la téléphonie mobile combiné aux
  estimations annuelles de population.
---

# Datavisualisation: Mouvements de population autour du confinement de mars 2020

![Mouvements de population autour du confinement de mars 2020 en France](../../.gitbook/assets/mouvements\_population\_confinement\_mars\_2020.png)

## Le projet en quelques mots

Ce projet documente les mouvements de population autour du confinement de mars 2020 à partir d'indicateurs anonymes issus de la téléphonie mobile fournis à l'Insee par trois opérateurs de téléphonie mobile et que l'Insee a combiné aux estimations annuelles de population. Pour compléter ces résultats, une seconde exploitation a été réalisée en partenariat avec CBS (Institut de Statistiques Néerlandais) permettant une visualisation fine des changements de population observés avant, et pendant, le premier confinement. Celle-ci offre la possibilité d’observer, de façon interactive et département par département, les changements observés (flux entrants et flux sortants). Les données mobilisées dans cette visualisation sont accessibles [ici](https://www.insee.fr/fr/statistiques/fichier/5350073/mouvements\_population\_confinement\_2020\_csv.zip).

#### → Lien vers la [datavisualisation](https://inseefrlab.github.io/lockdown-maps-R/inflows\_FR.html)

## L'usage du datalab&#x20;

Le projet a été réalisé avec Rstudio pour le traitement de données  et la visualisation. L'usage de Github a favorisé les échanges avec le CBS (Institut de Statistiques Néerlandais) mais aussi la réutilisation du code pour l'analyse de mouvement de population dans d'autres pays. De plus, la seconde exploitation a été grandement facilitée par le développement, lors de la première exploitation, d'un code reproductible sous la forme d'un proto-package (ensemble de fonctions) versionné avec Git.

## Les données

Téléchargement: [Données agrégées publiées en mai 2020](https://www.insee.fr/fr/statistiques/fichier/4635407/IA54\_Donnees.xlsx): [Données de la dataviz (données expérimentales), publiées en avril 2021](https://www.insee.fr/fr/statistiques/fichier/5350073/mouvements\_population\_confinement\_2020\_csv.zip) (référence pour les agrégats)

Voir également:[ Déplacements de population lors du confinement au printemps 2020 - Données expérimentales - Bases de données](https://insee.fr/fr/statistiques/5350073)

### Précautions d'usages des données

L’Insee considère ces résultats comme **expérimentaux**. Il faut souligner qu'il s'agit de statistiques expérimentales sujettes à des imprécisions du fait du type de données mobilisées et de leurs incertitudes inhérentes. De plus, ces nouvelles estimations réalisées au niveau de chaque couple département de résidence, département de présence, sont publiées arrondies à la centaine afin de permettre des ré-agrégations comme celles permettant de déployer l’outil de visualisation. Il est cependant préférable d'interpréter les croisements et aggrégations obtenues en arrondissant au millier de personnes.&#x20;

## En savoir plus sur le projet

{% embed url="https://github.com/InseeFrLab/lockdown-maps-R" %}
Lien vers le repo Github
{% endembed %}
