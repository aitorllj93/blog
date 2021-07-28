---
published: false
---
## My Stack Preferences for Scalable Enterprise Applications

In this article I will summarize the stack of the technologies I like to use and recommend while programming for scalable enterprise applications.

### Source Code

#### Version Control
I like to use [GIT](https://git-scm.com/). For my personal projects I like to use [GitHub](https://github.com/), but I also work with [Azure DevOps](https://dev.azure.com/) and [GitLab](gitlab.com)

#### Repository Structure

I use [Nx](https://nx.dev/) to manage my repository structure as a monorepo splitting the code into different applications and libraries.

#### Dependency Management
I prefer to keep the dependencies centralized in a single package.json so I try to keep everything up-to-date. Also prefer [npm](https://www.npmjs.com/) over [yarn](https://yarnpkg.com/)

#### Programming Languages
I like to have a single programming language across the entire repository if possible, so I use [TypeScript](https://www.typescriptlang.org/), [SASS (SCSS)](https://sass-lang.com/) and [HTML](https://developer.mozilla.org/en-US/docs/Web/HTML). Sometimes [GraphQL](https://graphql.org/) too for the communication layer

### Front-End
#### Framework
I like to use Angular as a FrontEnd Framework as it provides a really helpful set of tools for working in entreprise projects, such as [Dependency Injection based on Modules](https://martinfowler.com/articles/injection.html), Lazy Loading, Class-style code (it's easier to tie to class interfaces while replacing dependencies or testing), Observables Composition ([RxJS](https://rxjs.dev/)) and a powerful CLI with code generation and library updates management features.
#### User Interface
Most of the projects I develop are based on [Bootstrap](https://getbootstrap.com/), but I also work with [Angular Material](https://material.angular.io/) in some of them. I would like to try [Tailwind CSS](https://tailwindcss.com/). For mobile apps I use [Ionic Framework](https://ionicframework.com/)
#### State Management
My personal preference and the one I use the most is [NGXS](https://www.ngxs.io/) but the most popular solution is [NgRx](https://ngrx.io/). Special mention to [xstate](https://xstate.js.org/docs/) for state machines.
#### Server Communication
Depending on the server APIs, I use [OpenAPI](https://swagger.io/specification/) generator ([ng-openapi-gen](https://www.npmjs.com/package/ng-openapi-gen)), [Apollo](https://apollo-angular.com/docs/) for [GraphQL](https://graphql.org/) and [Socket.IO](https://socket.io/) for WebSockets

## Back-End

## Testing

### Unit
I like to use [Jest](https://jestjs.io/) as my main testing framework for both Unit and E2E testing

### E2E
When I'm programming E2E tests y like to use [Puppeteer](https://pptr.dev/) next to [Jest](https://jestjs.io/)

## Databases

For relational databases I prefer to use [PostgreSQL](https://www.postgresql.org/) and for NoSQL ones I use [MongoDB](https://www.mongodb.com/es). I have also worked with [Redis](https://redis.io/).

## Continuous Integration