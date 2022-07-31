(🇬🇧 English details available in the submodules)

# Qu’est-ce que le projet GMD-22 ?
Son but est simple :
Permettre à l’utilisateur de manger plus **diversifié** et de façon régulière, en simplifiant la préparation de **recettes de cuisines**.

À intervalles de temps réguliers (toutes les 3 semaines actuellement), l’application s’occupe de :
- Proposer à l’utilisateur des recettes de cuisine
- Remplacer certaines recettes si l’utilisateur le souhaite
- Générer la liste de courses associée
- S’occuper de la planification des événements
- Alerter l’utilisateur les jours clés
- Assister l’utilisateur lors de la préparation 

Elle est uniquement destinée à mon utilisation personnelle.

## À quoi ça ressemble ?

La page d'accueil, les recettes proposées et le planning 👉 
![Sans titre (5)](https://user-images.githubusercontent.com/79903008/182024591-b50b21fb-d56d-46e8-a2d0-136126f66bff.png)

La liste de courses générée et l'assistant de préparation 👉
![Sans titre (6)](https://user-images.githubusercontent.com/79903008/182024649-ccc7965c-9d47-494e-9f20-bb3225dfad5a.png)

L'écran de fin 👉
![Sans titre (7)](https://user-images.githubusercontent.com/79903008/182025043-b0d0d8fa-399b-42da-9868-61b3f769a80d.png)


## Mais, c’est une application native ?
La nature même du projet impose que celle-ci soit **utilisable depuis un smartphone**. Il faut par exemple pouvoir cocher les éléments de sa liste de courses au supermarché, avoir un minuteur lors de la préparation d’une recette…

En y réfléchissant, il n’y a d’ailleurs que très peu d’intérêt à pouvoir l’utiliser depuis un PC 🤔

Il pourrait être judicieux de développer une application native téléchargeable depuis l’Apple Store, mais par soucis de simplicité (process de validation complexe et chronophage, forte dépendance avec la plateforme, coût) je n'ai pas choisi cette possibilité.

Ce n’est donc **pas une application native**.

L’application a été réalisée avec React et a été designée uniquement pour être accessible depuis le navigateur d’un smartphone. 
Des navigateurs full-screen comme Kiosk existent, pour améliorer l’expérience utilisateur.
Enfin, la bibliothèque [Alertzy](https://alertzy.app/) permet d’envoyer très facilement des notifications push sur le smartphone.

Une fois hébergée, l’application répond à quasiment tous les critères sans pour autant être native !


## Quel hébergement ?

Les builds du front et du back sont exécutés sur une [Raspberry Pi](https://en.wikipedia.org/wiki/Raspberry_Pi), à travers des conteneurs docker. La raspberry pi est ensuite reliée au réseau local et redirigée sur les ports de mon routeur, pour etre accessible n'importe où, sans être connecté en wifi. 

Le déploiement est réalisée via [un script SSH](https://github.com/sbelalouibertot/gmd22-backend/blob/8317c6661e7c35dae2d5796e6e63c42afd2a351a/deploy.sh).



## Quelle solutions techniques ?
Les bibliothèques et frameworks utilisés sont axés autour de l’écosystème **Javascript**, et de sa surcouche [Typescript](https://www.typescriptlang.org/). 
Le but étant :
- De combler le principal inconvénient de JS : sa trop grande permittivité de typage, qui engendre rapidement des failles et de la dette technique
- D’avoir un langage commun côté client et côté serveur

### Coté Front, 
[React](https://reactjs.org/), pour sa légèreté, les possibilités intéressantes des hooks et la gestion performante des rendus de l’application.

Combiné à [Next](https://nextjs.org/), pour la gestion du routing et des ressources statiques principalement.

[Styled Components](https://styled-components.com/), pour implémenter facilement un design system et une identité visuelle pour l’app, créer des composants génériques avec style conditionnel, utiliser la convention [SASS](https://sass-lang.com/) et améliorer la lisibilité du html.

 
### Côté Back, 

Une base de données [PostgreSQL](https://www.postgresql.org/).

Combinée à l’ORM [Prisma](https://www.prisma.io/), pour générer le data model et effectuer des requêtes CRUD très facilement.

Ainsi que [GraphQL](https://graphql.org/), pour avoir une grande souplesse dans la manipulation des données et récupérer uniquement les données nécessaires, sur demande.

Le front et le back étant relié grâce à [Apollo](https://www.apollographql.com/docs/) (Client & Server) pour gagner en temps et simplicité sur l’écriture des requêtes, et synchroniser les types de données.


### Côté scripts :
- Une partie est exécutée sur demande de l’utilisateur (ex: Initialiser la base de données et le profil d’un utilisateur par défaut). Ces scripts seront sortis des sources et ne seront pas buildés.

- D’autres devront être lancés régulièrement (ex: Générer des recettes et une liste de courses) et seront ajoutés aux sources, puis exécutés via un Cron.


### Côté données :
- La quantité de données devant être massive, celles-ci sont [scrappées](https://github.com/sbelalouibertot/gmd22-backend/blob/8317c6661e7c35dae2d5796e6e63c42afd2a351a/bin/init/scrap.ts) depuis certains site internet en libre accès, en identifiant des instructions de cuisine et des aliments à partir d'une page de recette.
- Il est aussi possible d'ajouter certaines recettes manuellement, en remplissant un [fichier de données](https://github.com/sbelalouibertot/gmd22-backend/blob/8317c6661e7c35dae2d5796e6e63c42afd2a351a/bin/init/data/recipes.data.ts), utilisé lors de l'exécution d'un script de [remplissage de la base de données](https://github.com/sbelalouibertot/gmd22-backend/blob/8317c6661e7c35dae2d5796e6e63c42afd2a351a/bin/init/populateNewDatabase.ts).
