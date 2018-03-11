# Sommaire
*10/03/2018*

Ceci est un tutoriel pour débuter sur **NGRX**, vous verrez les différents concepts de cette librairie par le biais d'un exercice de création d'une **todo-list** et comment optimiser notre application par l'utilisation du pattern **Redux** .

>Actuellement **Ngrx** est en *version 5* ainsi que **Angular**.

## [0 - Introduction](https://github.com/fausfore/ngrx-guide/blob/master/documentations/introduction.md)
### 0-1 Redux, kesako ?
### 0-2 Pourquoi Redux alors ?
### 0-3 Flux vs Redux
### 0-4 Le Store, la base de tout
### 0-5  Le root reducer
### 0-6 Le schéma
### 0-7 Les actions
### 0-8  Action creator

## [1 - De Redux à NGRX](https://github.com/fausfore/ngrx-guide/blob/master/documentations/step-1.md)
### 1-1 Installation
### 1-2 Architecture Folder
### 1-3 Commençons ! [ début du tutoriel ]

## [2 - Getters & create todo](https://github.com/fausfore/ngrx-guide/blob/master/documentations/step-2.md)
### 2-1 Le Pipe et les opérateurs RXJS
### 2-2 Les States Selectors
### 2-3 Créer une todo

## [3 - Delete todo](https://github.com/fausfore/ngrx-guide/blob/master/documentations/step-3.md)
### 3-1 Gérer les ids

## [4 - Un peu de refacto !](https://github.com/fausfore/ngrx-guide/blob/master/documentations/step-4.md)
### 4-1 @Alias

## [5 - Select & Update Todo](https://github.com/fausfore/ngrx-guide/blob/master/documentations/step-5.md)

## [6 - Créer une API](https://github.com/fausfore/ngrx-guide/blob/master/documentations/step-6.md)
### 6-1 Service Angular Get Todos
### 6-2 Introduction de Effects

## [7 - Load Guard & DevTools](https://github.com/fausfore/ngrx-guide/blob/master/documentations/step-7.md)
### 7-1 Redux Devtools

## [8 - Create Todo v2](https://github.com/fausfore/ngrx-guide/blob/master/documentations/step-8.md)

## [9 - Delete Todo v2](https://github.com/fausfore/ngrx-guide/blob/master/documentations/step-9.md)

## [10 - Update Todo v2](https://github.com/fausfore/ngrx-guide/blob/master/documentations/step-10.md)

## [11 - Les action de type ERROR](https://github.com/fausfore/ngrx-guide/blob/master/documentations/step-11.md)
### 11-1 Système de logs

## [12 - @Ngrx/Entity](https://github.com/fausfore/ngrx-guide/blob/master/documentations/step-12.md)

## [13 - Petit Bonus](https://github.com/fausfore/ngrx-guide/blob/master/documentations/step-13.md)
### 13-1 Schematics
### 13-2 Testings basics
### 13-3 Change Detection OnPush

## [14 - Testings Advanced](https://github.com/fausfore/ngrx-guide/blob/master/documentations/step-14.md)
### 14-1 Mocks
### 14-2 Actions
### 14-3 Reducers
### 14-4 Selectors
### 14-5 Effects

## Conclusion 

J’espère que ce tutoriel vous a permis de comprendre NGRX et son implémentation.
Il reste des points comme les **Meta-reducers** ou le **router-store** que nous n'avons pas vu lors de ce tutoriel, vous pouvez trouvé ces points manquant sur le [gitHub officelle de NGRX](https://github.com/ngrx/platform).

Une autre utilitaire intéressant [ngrx-actions](https://github.com/amcdnl/ngrx-actions), vous permettra de réduire votre code *reduxien* avec des décorateurs.


*Auteur : **@Fausfore / Matias Ljubica***

<!--stackedit_data:
eyJoaXN0b3J5IjpbMTkyMTg3OTk3MCwxODIwNzMxMTEwLDE1Nj
czMzE2NDJdfQ==
-->