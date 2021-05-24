---
published: false
author: aitor
tags:
  - artificial-intelligence
  - state-machine
  - behavior-tree
  - goap
---
![Patterns in Videogames II: Artificial Intelligence]({{site.baseurl}}/images/patterns-in-videogames-ii-artificial-intelligence.png)

One of the most interesting and important thing in the videogame development ecosystem is the Artificial Intelligence development. With AI you can for example develop behaviours for your game's NPCs, and Enemies. When talking about AI in videogames, we need to keep in mind that most of the times it's not the same kind of software we refer when talking about AI in software development. These times AI in software development it's more focused on Machine Learning, Neural Networks and things like that, while game development is more focused on providing realistic behaviours for NPCs, but things are starting to change because Machine Learning is being used on game development fields like procedural generation.
And now, here are the 3 patterns in videogames AI development I like the most:

### State Machine

![State Machine Diagram](http://www.plantuml.com/plantuml/png/SoWkIImgAStDuOhMYbNGrRLJy4zBu-824WeWTfw2bK9IQKb9Vbv1KMfnSMeA5r08eWX3nY4rBmLe4000)

If you have worked in software development, this could be familiar to you. Finite State Machines have been used a lot as a way to define different states an entity can have, and the transitions able to do between different states. It's build on top of the State pattern, but it's not the pattern itself, but a way to implement it. Specially if your state starts getting complex.

The basic building blocks of a state machine are states and transitions. A state is a situation of a system depending on previous inputs and causes a reaction on following inputs. One state is marked as the initial state; this is where the execution of the machine starts. A state transition defines for which input a state is changed from one to another. Depending on the state machine type, states and/or transitions produce outputs.

If you want to learn more about state machines here are a few links:
- [Yakindu - What is a state machine?](https://www.itemis.com/en/yakindu/state-machine/documentation/user-guide/overview_what_are_state_machines)
- [Game Programming Patterns - State](https://gameprogrammingpatterns.com/state.html)

And if you want a nice implementation of it in JavaScript/TypeScript:
[XState](https://xstate.js.org/docs/). This one is extremelly powerful because it allows Statecharts, that's a more complex version of FSMs.

### Behavior Tree

### GOAP (Goal-Oriented Action Planning)
