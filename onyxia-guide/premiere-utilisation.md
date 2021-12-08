---
description: Visite guidée du Datalab
---

# Première utilisation

Bienvenue sur le Datalab Onyxia, plateforme libre service mutualisée de traitement de données, destinée aux statisticiens et _data scientists_ de l'Etat. Ce tutoriel propose une visite guidée du Datalab pour être rapidement opérationnel dans l'utilisation de ses services.

{% hint style="warning" %}
Les conditions d'utilisation du Datalab sont consultables à [cette adresse](https://sspcloud.fr/tos\_fr.md). Nous rappelons que le Datalab est destiné exclusivement au traitement de **données publiques et non-sensibles**. Des projets d'expérimentation mobilisant des données non ouvertes peuvent être menés en concertation avec l'équipe du Datalab, sous réserve de se conformer aux règles de sécurité spécifiques au projet.
{% endhint %}

## Le catalogue de services

Le [catalogue de services](https://datalab.sspcloud.fr/catalog/inseefrlab-helm-charts-datascience) est au centre de l'utilisation du Datalab. Il propose un ensemble de services destinés aux traitements statistiques de données ainsi qu'à la gestion complète des projets de _data science_.

![](<../.gitbook/assets/Screenshot from 2021-11-14 15-03-50.png>)

### Lancer un service

Pour lancer un service, il suffit de cliquer sur le bouton `Lancer` du service désiré.&#x20;

![Illustration des options de configuration d'un service avec RStudio](<../.gitbook/assets/Screenshot from 2021-11-14 15-09-30.png>)

Une page centrée sur le service demandé s'ouvre alors, qui offre plusieurs possibilités :&#x20;

* cliquer à nouveau sur le bouton `Lancer` pour lancer le service avec sa configuration par défaut ;
* personnaliser le nom que portera l'instance une fois le service lancé ;
* dérouler un menu de configuration afin de personnaliser la configuration du service avant de le lancer ;&#x20;
* sauvegarder une configuration personnalisée en cliquant sur le signet en haut à droite du service.

{% hint style="info" %}
La configuration précise des services du Datalab constitue un usage avancé et n'est donc pas traité dans ce tutoriel, mais dans d'autres pages de ce site documentaire.
{% endhint %}

### Utiliser un service

L'action de lancer un service amène automatiquement sur la page [Mes Services](https://datalab.sspcloud.fr/my-services), où sont listées toutes les instances en activité sur le compte de l'utilisateur.

![Instances en activité de services du Datalab](<../.gitbook/assets/Screenshot from 2021-11-14 15-26-15.png>)

Une fois le service lancé, un bouton `Ouvrir` apparaît qui permet l'accès au service. Un mot de passe — et, selon les services, un nom d'utilisateur — est généralement requis pour pouvoir utiliser le service. Ces informations sont disponibles dans le `README` associé au service, auquel on accède en cliquant sur le bouton du même nom.&#x20;

### Supprimer une instance

Supprimer une instance d'un service s'effectue simplement en cliquant sur l'icône en forme de poubelle en dessous de l'instance.

{% hint style="danger" %}
Pour certains services, la suppression d'une instance entraîne la suppression de toutes les données associées, et cette action est irrémédiable. Il est donc nécessaire de toujours bien lire le `README` associé à l'instance, qui précise les conséquences d'une suppression de l'instance. De manière générale, il est très important de s'assurer que les données ainsi que le code utilisés sont sauvegardés avant de supprimer l'instance. L'idéal est de [versionner son code avec Git](controle-de-version.md) et de procéder à des sauvegardes régulières des données à l'aide du [système de stockage S3](stockage-de-donnees.md).
{% endhint %}

{% hint style="danger" %}
Les ressources mises à disposition pour l'execution des services sont partagées entre les différents utilisateurs du Datalab. Veuillez à ne pas laisser en cours des services dont vous ne faites plus l'usage. Nous procédons parfois à une suppression systématique des instances inactives depuis un certain temps, afin de libérer des ressources.
{% endhint %}

## Aller plus loin

Nous avons souhaité présenter à travers ce tutoriel l'usage standard des services proposés sur le Datalab. Des usages plus avancés sont présentés dans d'autres pages de ce site documentaire :&#x20;

* [versionner son code avec Git](controle-de-version.md)
* [stocker ses données avec MinIO](stockage-de-donnees.md)
* [gérer ses secrets avec Vault](gestion-des-secrets.md)
* travailler sur des projets collaboratifs
* [s'auto-former avec le Datalab](https://www.sspcloud.fr/documentation)

## Support

Le support et l'aide à l'utilisation du Datalab sont effectuées sur un [salon dédié](https://matrix.to/#/#SSPCloudXDpAw6v:agent.finances.tchap.gouv.fr) du service de messagerie instantanée interministériel [Tchap](https://www.tchap.gouv.fr). Toute question sur l'utilisation du Datalab ou suggestion d'amélioration y sont les bienvenues.

Pour les agents n'utilisant pas Tchap, il est également possible de nous contacter par mail sur la BAL innovation@insee.fr.
