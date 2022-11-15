🇬🇧 _English details in the submodules_

Le code est composé d'un [front-end](https://github.com/sbelalouibertot/gmd22-frontend) et d'un [back-end](https://github.com/sbelalouibertot/gmd22-backend).

# Qu’est-ce que le projet GMD-22 ?
Son but est simple :
Permettre à l’utilisateur de manger plus **diversifié** et de façon régulière, en simplifiant la préparation de **recettes de cuisines**.

À intervalles de temps réguliers (toutes les 3 semaines actuellement), l’application s’occupe de :
- Proposer à l’utilisateur des **recettes de cuisine** 👨‍🍳
- Générer la **liste de courses** associée 🛒
- S’occuper de la planification des **événements** 📆

Un **assistant** a également été développé, pour permettre à l'utilisateur d'optimiser son temps de préparation une fois en cuisine.

La nature même du projet impose que celle-ci soit **utilisable depuis un smartphone**. Il faut par exemple pouvoir cocher les éléments de sa liste de courses au supermarché, avoir un minuteur lors de la préparation d’une recette… Ce n'est pas pour autant une application native, je détaille ce point plus bas ;)
<br></br>

### La page d'accueil, les recettes proposées et le planning 👉 

L'utilisateur a accès **aux infos clés** sur la première page: prochaine recette prévue, état d'avancement des étapes, aperçu des paramètres.
Les étapes représentent ici des évènements, et peuvent être de différents types : début/fin de période, courses, préparation d'une recette...

Sur la seconde page, il peut ensuite avoir un aperçu des **recettes actuelles** de la période, et les remplacer à tout moment par une autre recette aléatoire. La liste de courses sera ensuite regénérée automatiquement.

La troisième page permet d'afficher les **évènements générés** de façon détaillée. Arbitrairement, les courses sont le vendredi et la préparation de recettes le lundi, mais il est possible de modifier ces dates via un drag and drop. L'utilisateur recevra une notification push au début de chaque jour contenant des évènements.

![Sans titre (9)](https://user-images.githubusercontent.com/79903008/182037069-bf3e1e85-1ac5-49f9-b4e9-e80c8a1e0e07.png)

### La liste de courses, la page d'un ingrédient et la page d'une recette 👉

Il est possible d'accéder à la **liste de courses**, qui affiche les ingrédients des recettes et leur quantité totale. Si des ingrédients sont utilisés dans plusieurs recettes avec des unités différentes, alors toutes les unités seront affichées, comme par exemple "1 filet et 4 cuillères à soupe".
La liste de courses peut être mise à jour en cochant un élément.

Un clic sur un ingrédient ouvre sa page, et affiche divers informations : toutes les recettes liées à cet ingrédient, ainsi que celles prévues pour la période courante (en vert).

Un clic sur une recette permet ensuite d'afficher les ingrédients nécessaires, leur quantité et les instructions à suivre. Cependant, il sera plus intéressant d'utiliser l'assistant de préparation une fois en cuisine.

![Sans titre (11)](https://user-images.githubusercontent.com/79903008/182038744-3f3a75e4-368b-4e6f-8528-21c3a0efe048.png)


### L'assistant de préparation 👉

L'assitant affiche dans un premier temps les recettes prévues pour la session, et les temps totaux si elles étaient réalisées l'une après l'autre.

L'objectif ici est de les faire en parallèle, et c'est le rôle de l'assistant sur la seconde page. A chaque avancement, l'utilisateur clic sur une étape et celle-ci prend successivement les statuts "Non commencé" -> "en cours" -> "terminé". Le pourcentage d'avancement total de la préparation est alors mis à jour. Egalement, un chronomètre est lancée automatiquement au début de la préparation. Il est aussi possible de programmer un minuteur pour une étape spécifique.

Une fois toutes les étapes terminées, l'utilisateur découvre sa performance !


![Sans titre (10)](https://user-images.githubusercontent.com/79903008/182038738-d6efe6ed-d8b5-451b-abe7-24f04103cada.png)


## Mais, c’est une application native ?

Il pourrait être judicieux de développer une application native téléchargeable depuis l’Apple Store, mais par soucis de simplicité (process de validation complexe et chronophage, forte dépendance avec la plateforme, coût) je n'ai pas choisi cette possibilité.

Ce n’est donc **pas une application native**.

L’application a été réalisée avec React et a été designée uniquement pour être accessible depuis le navigateur d’un smartphone. 
Des navigateurs full-screen comme Kiosk existent, pour améliorer l’expérience utilisateur.
Enfin, la bibliothèque [Alertzy](https://alertzy.app/) permet d’envoyer très facilement des notifications push sur le smartphone.

Une fois hébergée, l’application répond à quasiment tous les critères sans pour autant être native !

<br></br>

## Quel hébergement ?

Les builds du front et du back sont exécutés sur une [Raspberry Pi](https://en.wikipedia.org/wiki/Raspberry_Pi), à travers des conteneurs [Docker](https://www.docker.com/). La raspberry pi est ensuite reliée au réseau local et redirigée sur les ports de mon routeur, pour etre accessible n'importe où, sans être connecté en wifi. 

Le déploiement est réalisé via [un script SSH](https://github.com/sbelalouibertot/gmd22-backend/blob/8317c6661e7c35dae2d5796e6e63c42afd2a351a/deploy.sh).
![raspberry-pi](https://user-images.githubusercontent.com/79903008/182044595-ad7df8db-156d-45f4-a5d8-5ba7fad0e881.png)

<br></br>

## Quelle solutions techniques ?
Les bibliothèques et frameworks utilisés sont axés autour de l’écosystème **Javascript**, et de sa surcouche [Typescript](https://www.typescriptlang.org/). 
Le but étant :
- De combler le principal inconvénient de JS : sa trop grande permittivité de typage, qui engendre rapidement des failles et de la dette technique
- D’avoir un langage commun côté client et côté serveur

<br></br>

### Coté Front, 
[React](https://reactjs.org/), pour sa légèreté, les possibilités intéressantes des hooks et la gestion performante des rendus de l’application.

Combiné à [Next](https://nextjs.org/), pour la gestion des performances, du routing et des ressources statiques principalement.

[Styled Components](https://styled-components.com/), pour implémenter facilement un [design system](https://github.com/sbelalouibertot/gmd22-frontend/tree/2e81b9e1917acf394f781c34202b0ea51a91b86a/src/styles/design-system) et une identité visuelle pour l’app, créer des composants génériques avec style conditionnel, utiliser la convention [SASS](https://sass-lang.com/) et améliorer la lisibilité du html.

<br></br>

### Côté Back, 

Une base de données [PostgreSQL](https://www.postgresql.org/).

Combinée à l’ORM [Prisma](https://www.prisma.io/), pour générer le data model, gérer les migrations et effectuer des requêtes CRUD très facilement.

Ainsi que [GraphQL](https://graphql.org/), pour avoir une grande souplesse dans la manipulation des données et récupérer uniquement les données nécessaires, sur demande.

Le front et le back étant synchronisés grâce à [Apollo](https://www.apollographql.com/docs/) (Client & Server) pour gagner en temps et simplicité sur l’écriture des requêtes, et synchroniser les types de données.

<br></br>

### Côté scripts :
- Une partie est exécutée sur demande de l’utilisateur (ex: Initialiser la base de données et le profil d’un utilisateur par défaut). Ces scripts sont sortis des sources et ne sont pas buildés.

- D’autres doivent être lancés régulièrement (ex: Générer des recettes et une liste de courses) et sont donc ajoutés aux sources, puis exécutés via [des crons](https://github.com/sbelalouibertot/gmd22-backend/blob/8317c6661e7c35dae2d5796e6e63c42afd2a351a/src/bin/crons.ts).

<br></br>

### Côté tests :
Jest, pour réaliser : 
- Des tests d'intégration, sur les services chargés du CRUD et de certains traitements en base
- Des tests unitaires, sur certaines fonctions ciblées

En [TDD](https://www.all4test.fr/blog-du-testeur/les-3-cles-pour-maitriser-le-test-driven-development-tdd/), dans certains cas.

<br></br>

### Côté données :
- La quantité de données devant être massive, celles-ci sont [scrappées](https://github.com/sbelalouibertot/gmd22-backend/blob/8317c6661e7c35dae2d5796e6e63c42afd2a351a/bin/init/scrap.ts) depuis certains site internet en libre accès, en identifiant des instructions de cuisine et des aliments à partir d'une page de recette.
- Il est aussi possible d'ajouter certaines recettes manuellement, en remplissant un [fichier de données](https://github.com/sbelalouibertot/gmd22-backend/blob/8317c6661e7c35dae2d5796e6e63c42afd2a351a/bin/init/data/recipes.data.ts), utilisé lors de l'exécution d'un script de [remplissage de la base de données](https://github.com/sbelalouibertot/gmd22-backend/blob/8317c6661e7c35dae2d5796e6e63c42afd2a351a/bin/init/populateNewDatabase.ts).

<br></br>

## Modèle de données
J'ai commencé par créer une représentation simplifié du modèle de données. 

- Chaque utilisateur dispose de plusieurs préférences (comme par exemple la fréquence de génération des périodes).
- Un évènement généré est toujours liés à un utilisateur et peut être lié à une liste de courses (représente l'action de faire ses courses dans ce cas), ou à plusieurs recettes (= la préparation de plusieurs recettes en parallèle).
- Une recette de cuisine contient des instructions, qui contiennent des aliments, eux-mêmes intégrés à la liste de courses.

![Capture d’écran 2022-07-31 à 22 33 32](https://user-images.githubusercontent.com/79903008/182044224-1a0c40ce-f4e6-4b16-b81d-3f9999209be7.png)


Pour être appliqué à un modèle de type Postgres, j'ai ensuite ajouté des tables de jointures : shopping_lists_events, shopping_lists_food, recipes_instructions_food.

Le modèle de données final généré par Prisma est tel que : 

<img width="1098" alt="Capture d’écran 2022-10-28 à 17 09 20" src="https://user-images.githubusercontent.com/79903008/198669947-efb824ec-6d7c-4ce6-adae-3a2547026afd.png">

(aperçu depuis [prismaliser](https://prismaliser.app/))

<br></br>

## Maquettage

J'ai également réalisé des maquettes Figma en m'inspirant de templates existants, avant de commencer tout développement.

![Capture d’écran 2022-07-31 à 22 54 49](https://user-images.githubusercontent.com/79903008/182045018-d824e134-cd5a-498d-b267-0c53793afdbd.png)

J'ai ensuite pu en déduire : 
- Le design system
- Les composants à créer
- Le routing
- Les queries & mutations à développer côté back-end


## Performances

Je souhaitais avoir une application la plus rapide possible. Ce qui implique d'avoir un [chemin critique de rendu](https://www.codein.fr/blog/le-chemin-critique-du-rendu-comment-ca-marche-performance-web-3-6) de ma page d'accueil le plus court possible et une navigation fluide.

J'ai choisi Next pour plusieurs raisons : 
- Le [pre-rendering](https://nextjs.org/learn/basics/data-fetching/pre-rendering) côté serveur de l'HTML initial est automatique
- Le [prefetch](https://web.dev/route-prefetching-in-nextjs/) de tous les liens de la page courante est automatique
- Next utilise [Webpack](https://webpack.js.org/) et gère notamment: 
  - Le découpage optimisé des bundles
  - Les imports des images, qui sont par défaut lazy loadées

### Chemin critique de rendu et optimisation des bundles
J'ai dans un premier temps cherché à réduire au maximum la taille des bundles, en évitant le code mort. Des plugins comme [Next Bundle Analyze](https://daily-dev-tips.com/posts/exploring-the-nextjs-bundle-analyzer/), permettent d'avoir une représentation des bundles les plus gourmands, ce qui a permis de : 
- Privilégier des imports minimalistes quand c'est possible (ex : `import mapValues from ‘lodash/mapValues’` au lieu de `import {mapValues} from ‘lodash’` )
- Dans le cas contraire, charger les libs volumineuses (ex: framer-motion) en lazy loading, une fois toutes les autres ressources prioritaires chargées, pour ne pas impacter l'interactivité de l'application

<img width="1510" alt="Capture d’écran 2022-11-13 à 23 18 37" src="https://user-images.githubusercontent.com/79903008/201547372-6cc1a4f5-b60a-4519-b76c-6a5fc3b01e52.png">

### Requêtage de données côté client
Les bundles de la première page se chargeant très vite, j'ai ensuite choisi de réaliser un requêtage de données côté client (dans un premier temps).
J'affichais alors des skeletons ciblés sur les zones en attente de données, et des images floutées de faible taille (pour [optimiser mon LCP](https://web.dev/optimize-lcp/?utm_source=lighthouse&utm_medium=devtools#preload-important-resources)) : 

<img width="236" alt="Capture d’écran 2022-11-14 à 22 00 13" src="https://user-images.githubusercontent.com/79903008/201764492-63e5993b-abe2-41a8-96b6-74f057d876ea.png">

### Regénération statique incrémentale
Ensuite, j'ai réalisé qu'il existait d'autres leviers pour accélérer encore le temps de chargement : la [régénération statique incrémentale](https://nextjs.org/docs/basic-features/data-fetching/incremental-static-regeneration)

En effet, ma page d'accueil requête 3 type de données différentes : 
- La prochaine recette à venir et durée
- Un aperçu des évènements en cours
- Les préférences de l'utilisateur

Comme ces données varient très peu (1 fois par jour), il peut être pertinent de faire un fetch unique regénéré quotidiennement. 

### Analyse des performances
En résumé : 

1) Le client (smartphone) requête le serveur (raspberry pi)
2) Le serveur pré-rend très rapidement l’html et les données de la première page, avec des images floutées
3) Le CSS se charge de façon non bloquante pour le JS (async)
4) Le DOM et le CSSOM sont construits
5) Les bundles JS sont chargés en parallèle par batch
6) Les images volumineuses sont chargées
7) Les JS des liens de la première page sont chargés, par batch, tout comme les libs moins prioritaires (citées précédemment)

Une fois en production et herbergé, [Lighthouse](https://chrome.google.com/webstore/detail/lighthouse/blipmdconlkpinefehnmjammfjpmpbjk?hl=fr) indique les résultats suivants : 

<img width="774" alt="Capture d’écran 2022-11-14 à 23 38 36" src="https://user-images.githubusercontent.com/79903008/201782886-97846a29-dbbc-4c7c-b35f-b754e6518a75.png">

A noter que : 
- Les performances varient en fonction des disponibilités des ressources de la Raspberry Pi
- Le score "bonne pratiques" est pénalisé par l'absence de protocole HTTPS


