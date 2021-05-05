---
published: true
author: aitor
tags:
  - angular
  - state-management
  - redux
  - ngrx
  - ngxs
  - akita
---
![State Management in Angular Applications]({{site.baseurl}}/images/state-management-in-angular-applications.png)
In these days, when people speak about state management in Front-End applications most of the time they are talking about Redux, but there are some interesting alternatives that should be taken into account. For this post, I will talk about some of those alternatives, focusing on Angular-ready implementations.

### NgRx
[NgRx](https://ngrx.io/) is a Redux implementation made with RxJS, so, if you know Redux, and you know RxJS, you won't have any problem working with it. It's the most popular State Management library for Angular, but also comes with all the Redux trade-offs such as the amount boilerplate code needed to setup your project. Some of the features it offers as seen in it's official website are:
* Managing global and local state.
* Isolation of side effects to promote a cleaner component architecture.
* Entity collection management.
* Integration with the Angular Router.
* Developer tooling that enhances developer experience when building many different types of applications.

I won't show NgRx code because as I said, it's essentialy Redux + RxJS.

### NGXS
I'm really in love with this library.
[NGXS](https://www.ngxs.io/) is modeled after the CQRS pattern popularly implemented in libraries like Redux and NgRx but reduces boilerplate by using modern TypeScript features such as classes and decorators.

In NGXS you manage your state with the following pieces of code:

#### Actions
Actions are just the same than Redux Actions:
```ts
export class AddAnimal {
  static readonly type = '[Zoo] Add Animal';
  constructor(public name: string) {}
}
```

#### StateModel 
The StateModel class is just a representation of the data you will store.

```ts
export class ZooStateModel {
  animals: Animal[];
}
```

#### State
The State class is the Service where you will define `Selectors` and `Action Handlers`.
```ts
@State<ZooStateModel>({
  name: 'zoo',
  defaults: {
    animals: []
  }
})
@Injectable()
export class ZooState {

  /** You will be able to subscribe this selector in your components **/
  @Selector()
  static pandas(state: ZooStateModel) {
    return state.animals.filter(animal => animal.indexOf('panda') > -1);
  }

  /** This method will be called when you dispatch a FeedAnimals Action **/
  @Action(AddAnimal)
  feedAnimals(
    { setState, getState }: StateContext<ZooStateModel>,
    { payload }: AddAnimal
  ) {
    const animals = getState().animals;
    setState({
      animals: animals.concat(payload)
    });
  }
}
```

### Component
In your components, you can use the State selectors, and also inject the Store so you can dispatch actions.

```ts
@Component({
  selector: 'app-zoo',
  templateUrl: './zoo.component.html'
})
export class ZooComponent {
  
  @Select(ZooState.pandas) pandas$: Observable<string[]>;
  
  constructor(private store: Store) {}
  
  addAnimal(name: string) {
    this.store.dispatch(new AddAnimal(name));
  }
}
```

### Akita
[Akita](https://datorama.github.io/akita/) is a state management pattern, built on top of RxJS, which takes the idea of multiple data stores from Flux and the immutable updates from Redux, along with the concept of streaming data, to create the Observable Data Store model.

Akita solves the State Management problem with this pieces:

#### State

In Akita, the State is the same than NGXS's StateModel. Just a representation of the data you will store.

```ts
export class ZooState {
  animals: Animal[];
}
```

#### Store

Unlike NGXS that has a single Store with multiple States with Action Handlers, Akita has multiple Stores. You can create a Store just extending the Store class.

```ts
import { Store, StoreConfig } from '@datorama/akita';

@StoreConfig({ name: 'zoo' })
export class ZooStore extends Store<ZooState> {
  constructor() {
    super([]);
  }
}
```

#### Services

You then can add it to your services and work with it.

```ts
import { ZooStore } from './zoo.store';
 
export class ZooService {
  constructor(private zooStore: ZooStore) {}

  addAnimal(name: string) {
    this.zooStore.update(state => ({
      animals: state.animals.concat(name)
    }));
  }
}
```

#### Queries
For the Selectors part, Akita uses what they call Queries.

```ts
import { Query } from '@datorama/akita';
import { ZooState } from './zoo.state';
import { ZooStore } from './zoo.store';

export class ZooQuery extends Query<ZooState> {
  pandas$ = this.select(state => state.animals.filter(animal => animal.indexOf('panda') > -1));
  
  constructor(protected store: ZooStore) {
    super(store);
  }
}
```

As you can see, Akita has in common with NGXS that it tries to reduce the boilerplate code, but it does in a different way.

Another interesting thing about Akita is that it's compatible with Vanilla JS so if you want to make framework agnostic code, you may be interested on this one.

## TL;DR

If you're planning to add State Management to your application, I think you should stop just adding Redux and evaluate other options that may fit better and also reduce complexity. One interesting thing about the libraries I chose for this article is that all of them have plugins for the Redux DevTools.