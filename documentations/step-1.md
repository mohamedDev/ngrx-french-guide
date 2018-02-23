# De Redux à Ngrx

### Début de la branche step-1 

Redux est un pattern déjà bien implémenter sur les principaux frameworks/librairies javascript du moment.
Pour React  => **react-redux**
Pour Vue  => **vuex**
Pour Angular  => **Ngrx**

Il est donc inutile de créer un store from scratch. Comme nous allons créer une application Angular, nous utiliserons **Ngrx**.
Cette librairie est donc une implémentation "reduxienne". Mais pas que... elle prend une bonne couche de **RxJS** comme Angular lui-même et utilise les **Observables** pour populer les states dans les composants Angular.

## Installation

Pour commencer on part sur un angular version 5 donc vous devez avoir votre Cli au dessus de la version **1.6.0**, elle doit comprendre la version **5.5.6** de RxJs pour utliser les **pipes** qui seront utilisé plus tard.
Donc on créer un nouveau projet angular
```shell
$ ng new ngrx-tutoriel-app --style=scss
```
puis dans le dossier rajouter **Ngrx en version 5.0 et plus**
```shell
$ npm install @ngrx/store ou yarn add @ngrx/store
```

## Architecture Folder
Pour le schéma des folders partez de **app/**
```
app
│   app.component.ts
│   app.component.scss
│	app.component.html
│   app.module.ts  
└───store
│   │   index.ts
│   └───actions
│   │   │   exemple.action.ts
│   │   │   ...
│   └───reducers
│  	│   │   exemple.reducer.ts
│   │   │   ...
└───modules
```

## Set up

Pour changer de l'exemple du counter, on va partir sur une **todolist**, eh oui encore ...
Avec l’implémentation de Redux on va penser en actions utilisateur et serveur et faire une synthèse de celle-ci :
Pour faire une todolist on a :

Une initalisation des todos -> **GET**
Création de todo -> **PUT**
Suppression de todo-> **DELETE**
Modification de todo -> **PATCH / POST**

Un bon vieux **CRUD** quoi.

On commence a écrire notre InitializeTodos et l'interface .

*models/todo.ts*
```javascript
export interface Todo {
	userId: number;
	id: number;
	title: string;
	completed: boolean;
}

export interface TodoListState {
	data: Todo[];
	loading: boolean;
	loaded: boolean;
}
```
On se base sur le model de **[JsonPlaceholder](https://jsonplaceholder.typicode.com/)**.
*mocks/todo-list.ts*
```javascript
export const todosMock = [{
    "userId": 1,
    "id": 1,
    "title": "delectus aut autem",
    "completed": false
  },
  {
    "userId": 1,
    "id": 2,
    "title": "quis ut nam facilis et officia qui",
    "completed": false
  },
  {
    "userId": 1,
    "id": 3,
    "title": "fugiat veniam minus",
    "completed": false
  },
  {
    "userId": 1,
    "id": 4,
    "title": "et porro tempora",
    "completed": true
  },
  {
    "userId": 1,
    "id": 5,
    "title": "laboriosam mollitia et enim quasi adipisci quia provident illum",
    "completed": false
  },
  {
    "userId": 1,
    "id": 6,
    "title": "qui ullam ratione quibusdam voluptatem quia omnis",
    "completed": false
  },
  {
    "userId": 1,
    "id": 7,
    "title": "illo expedita consequatur quia in",
    "completed": false
  }]
```
Un petit mock pour tester.

*store/actions/todo-list.action.ts*
```javascript
export namespace TodoListModule {

    export enum ActionTypes {
        INIT_TODOS = '[todoList] Init Todos'
    }

    export class InitTodos {
        readonly type = ActionTypes.INIT_TODOS;
    }

    export type Actions = InitTodos;
}
```
Question pratique je préfère encapsulé le tout dans un **namespace** pour simplifié les import, libre à vous de ne pas le faire.
Le dernier export Actions servira pour le typage du reducer uniquement.


*/store/reducers/todo-list.reducer.ts*
```javascript
import { TodoListModule } from '../actions/todo-list.action';
import { TodoListState  } from '../../models/todo';
import { todosMock } from '../../mocks/todo-list-data';

const initialState: TodoListState = {
    data: [],
    loading: false,
    loaded: false
};

export function todosReducer(
    state: TodoListState = initialState,
    action: TodoListModule.Actions
): TodoListState {

  switch (action.type) {

    case TodoListModule.ActionTypes.INIT_TODOS :
    return {
        ...state,
        data: [
            ...todosMock
        ]
    };

    default:
        return state;
    }
}
```
*exemple : /store/index.ts*
```javascript
import { ActionReducerMap } from '@ngrx/store';
import { InjectionToken } from '@angular/core';

import { todosReducer } from './reducers/todo-list.reducer';
import { TodoListState } from '../models/todo';

const reducers = {
    todos: todosReducer
};

export interface AppState {
    todos: TodoListState;
}

export function getReducers() {
    return reducers;
}

export const REDUCER_TOKEN = new InjectionToken<ActionReducerMap<AppState>>('Registered Reducers');
```

> Le mode Ahead of Time (AoT) Compilation de Angular exige que tous les symboles référencés dans les métadonnées du décorateur soient analysables statiquement. Pour cette raison, nous ne pouvons pas injecter dynamiquement l'état à l'exécution avec AoT sauf si nous fournissons notre **reducers** en tant que fonction. 

*/app.module.ts*
```javascript
import { BrowserModule } from '@angular/platform-browser';
import { NgModule } from '@angular/core';
import { StoreModule} from '@ngrx/store';
import { getReducers, REDUCER_TOKEN } from './store';
import { AppComponent } from './app.component';


@NgModule({
  declarations: [
    AppComponent
  ],
  imports: [
    BrowserModule,
    StoreModule.forRoot(REDUCER_TOKEN)
  ],
  providers: [
    {
      provide: REDUCER_TOKEN,
      useFactory: getReducers
    }
  ],
  bootstrap: [AppComponent]
})
export class AppModule { }
```


>Maintenant pour créer notre **Store**, il suffit de prendre le **StoreModule** et de lui injecter nos reducers.

*exemple : /app.component.ts*
```javascript
import { Store, select } from '@ngrx/store';
import { OnInit, Component } from '@angular/core';
import { Observable } from 'rxjs/Observable';
import { TodoListModule } from './store/actions/todo-list.action';
import { AppState } from './store';
import { Todo } from './models/todo';

@Component({
  selector: 'app-root',
  styleUrls: ['./app.component.scss'],
  template: `
    <h1>la todolist redux style !</h1>
    <ul>
		<li *ngFor="let todo of (todos$ | async)?.data">
			<label>{{ todo.title }}</label>
			<input type="checkbox" [value]="todo.completed"/>
			<button>Supprimer</button>
		</li>
	</ul>
  `
})
export class AppComponent implements OnInit {

  todos$: Observable<Todo[]>;

  constructor(
    private store: Store<AppState>
  ) {
    this.todos$ = store.pipe(select('todos'));

    /* A éviter
	this.todo$.subscribe((todos) => {
		this.todos = todos;
	});

    Dans ce cas de figure on ne fait pas de mutation sur la liste
    de todos dans le composant et cela évite de faire
    un unsubscribe dans le OnDestroy
    ainsi qu'un *ngIf dans le <ul> dans le cas ou la donnée soit vide.
	*/

  }

  ngOnInit() {
    this.store.dispatch(new TodoListModule.InitTodos());
  }

}

```

### Fin de la branche step-1 

<!--stackedit_data:
eyJoaXN0b3J5IjpbLTE3ODM1MTk0MzFdfQ==
-->