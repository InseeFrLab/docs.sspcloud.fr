# FAQ

## 🟠 Je ne trouve pas le mot de passe d’accès à mon service.&#x20;

Le mot de passe est le même pour tous les services et est accessible depuis la page [Mon compte](https://datalab.sspcloud.fr/account) ou la page [Mes services](https://datalab.sspcloud.fr/my-service) en cliquant sur le bouton `Mot de passe des services`

## 🟠 Quand je crée un service R Studio, quels sont les identifiants et le mot de passe d’accès à saisir ?

L'identifiant RStudio est: `onyxia` Le mot de passe est le même pour tous les services et est accessible depuis la page [Mon compte](https://datalab.sspcloud.fr/account) ou la page [Mes services](https://datalab.sspcloud.fr/my-service) en cliquant sur le bouton `Mot de passe des services`

## 🟠 Est-ce que je peux modifier mes identifiants Git une fois le service créé ?&#x20;

Oui, il est possible de modifier plusieurs informations liées au compte depuis la page [Mon compte](https://datalab.sspcloud.fr/account).

## 🟠 J'ai l'impression que Blazing SQL ne fonctionne pas...

Dans le formulaire de configuration, il y a l’onglet dédiées aux ressources où vous devez réserver la mémoire (Mi), la CPU et la GPU d’un service avant son lancement.

## 🟠 Mon service me renvoie une erreur 403.&#x20;

Une erreur 403 est liée à la protection réseau qu'on applique aux services. Les services créés à partir d'une certaine IP ne sont initialement accessibles que depuis cette IP.  Cette protection est gérée dans l'onglet « Security » avec la case à cocher « Enable IP protection ».

## 🟠 Comment obtenir des logs sur le lancement de mon service ?

Cette manipulation nécessite l'usage d'un terminal dans un service RStudio, Jupyter... Il faut d'abord trouver le nom de son *pod* Kubernetes :

```
kubectl get pod
```

Par exemple rstudio-xxxxxx-x ou jupyter-python-xxxxxx-x.
Pour ensuite afficher les logs de ce pod :

```
kubectl logs rstudio-xxxxxx-x
```
