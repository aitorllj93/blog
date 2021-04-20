---
published: false
tags:
  - angular
  - jest
  - ngxs
---
![How I structure large Angular applications]({{site.baseurl}}/images/how-i-structure-large-angular-applications.png)
In this post I will cover the way I like to structure large Angular applications in order to make them scalable and easy to test.

* TOC
{:toc}

### Folder Structure
Usually, when I work in an Angular project, I try to keep the folder structure like this:
```
├── addresses
│   ├── components
│   │   └── addresses-info
│   ├── pages
│   │   ├── addresses-list
│   │   └── addresses-detail
│   ├── services
│   └── state
├── core
│   └── services
├── customers
│   ├── components
│   │   └── customers-info
│   ├── pages
│   │   ├── customers-list
│   │   └── customers-detail
│   ├── services
│   └── state
└── shared
    ├── components
    ├── directives
    └── pipes
```

I will talk more on this in the next sections. The most important things are: 
* Have core and shared modules for common logic
* Separate presentational and container components
* Have feature folders, each one with its own components, services and state

### Modules
There are 4 different types of modules you should have in an Angular project, based on what they do:

#### Core Module
The core module should be used to store logic that's shared through your application. Mainly services. If you need to import it, you should import it once, in the app module.
#### Shared Module
The shared module should contain generic components/pipes/directives you will use in other modules. It must be imported on each module you will use at.
#### Feature Modules
Feature modules allows you to split your code based on the different features your application will include. Sometimes, they also include **Routing modules** that just provide some routing configuration to the Angular Router module.
### Components
I try to keep components the smaller I can. This way, the code tends to be more reusable and testable. While I'm working at the UI/UX layer I like to have a [Storybook](https://storybook.js.org/) that allows me to have a visual environment where I can isolate generic components. 
#### Components vs Pages/Containers
It's important to separate presentational and container components. Presentational components are just small pieces of code with the responsibility of printing things on the screen and collecting user inputs. Container components use those presentational components and state management access to render real application logic. I like to make those separation more visible by calling container components *Pages* because I think it's more intuitive. Another names that come to my mind are *Screens* or *Scenes* in the game development industry
#### Styling
First of all, use SASS (The SCSS syntax of course). SASS allows you to use some programming common things like variables and loops inside your CSS files. This is especially useful when you are theming or writing repetitive code.
I like to have most of my styles outside the app folder, in a styles folder. I only use component styles when I need them for a unique use case. This way, if I need to develop another application for the same company/product even without Angular, I can just drop those styles inside the project and they will work. A better thing would be to have a separate package with just the styles but that may be overengineering and slow down the development process in most of the cases.
### Services
#### API Services
When I work with REST API services I try those services implement correctly the [OpenApi Specification](https://swagger.io/specification/). This way, I can then use a generator such as [ng-openapi-gen](https://www.npmjs.com/package/ng-openapi-gen) to generate the connection layer in a folder inside my project. Every time the API changes, I just rerun the generation command.
#### GraphQL Services
In the GraphQL case, I have no experiencie with generation libraries. I use the excellent [Apollo Client](https://apollo-angular.com/docs/) package. Inside my services folders, I create two files: One with just the query definition as a gql template, and the other one the service itself that executes that query with the apollo client. Instead of rendering values directly inside the query, I always use variables. 
#### External Libraries
This may be a controversial oppinion, but I like to add most of the external libraries I use to the Angular Injector. I think it makes things clearer than just importing them in your wrapper services. Also makes easier to test logic that uses those libraries to consume external data you won't have in your test environments. It's very easy to add external libraries to the Angular injection system. If you have the class, you can just add it in your providers array and it will work:
```ts
providers: [
  {
    provide: MyFancyLibraryClass,
    useValue: myFancyLibrarySingletonInstance
  }
]
```
If you don't have the class, you can use InjectionTokens, that in the latest versions even allows you to provide them in root omitting the "adding to providers" step.
```ts
const MY_SERVICE_TOKEN = new InjectionToken<MyService>('Manually constructed MyService', {
  providedIn: 'root',
  factory: () => new MyService()
});
```
### State Management
The Angular library I prefer for state management it's called [ngxs](https://www.ngxs.io/). It's not the typical Redux implementation, but an easier one that I think it fits better in the Angular way to do the things.
First of all we have our StateModel, that's just the interface our state will be based on:
```ts
export class AddressesStateModel {
  addresses: Address[];
  selectedAddress: Address;
}
```
Then we have the actions:
```ts
export namespace AddressesActions {
  
  export class FetchAddresses {
    static readonly type = '[Addresses] Fetch Addresses'
  }
  
  export class SelectAddress {
    static readonly type = '[Addresses] Select Address'
    constructor(public payload: string) {}
  }

}
```
And at last, we define our state, which includes selectors and action handlers:
```ts
@State<AddressesStateModel>({
  name: 'addresses',
  defaults: JSON.parse(JSON.stringify(addressesInitialState)),
})
export class AddressesState {

  @Selector()
  static addresses(state: AddressesStateModel) {
    return state.addresses;
  }

  @Selector()
  static selectedAddress(state: AddressesStateModel) {
    return state.selectedAddress;
  }

  @Action(AddressesActions.FetchAddresses)
  fetchAddresses(
    { setState }: StateContext<AddressesStateModel>
  ) {
    setState(
      patch({
        addresses: []
      })
    );
  }

  @Action(AddressesActions.SelectAddress)
  fetchAddresses(
    { getState, setState }: StateContext<AddressesStateModel>,
    { payload }: AddressesActions.SelectAddress
  ) {
    setState(
      patch({
        selectedAddress: getState().addresses.find(address => address.id === payload)
      })
    );
  }
  
}
```

Now that we have configured our state we can use like this inside the components:
```ts
@Component({
  selector: 'app-addresses-list,
  ...
})
export class AddressesListPage {
  @Select(AddressesState.addresses) addresses$: Observable<Address[]>;
  
  constructor(private store: Store) {}
  
  selectAddress(addressId: string) {
    this.store.dispatch(new AddressesActions.SelectAddress(addressId));
  }
}
```

### Testing
When setting up the testing environment/tools I usually remove Jasmine, Karma and Protractor because I feel more comfortable with Jest. For the e2e part, depending on the project I use Cypress or Puppeteer.
