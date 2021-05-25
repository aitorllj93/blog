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
- [Game AI Pro - A Reusable, Light-Weight
Finite-State Machine](http://www.gameaipro.com/GameAIPro3/GameAIPro3_Chapter12_A_Reusable_Light-Weight_Finite-State_Machine.pdf)

And if you want a nice implementation of it in JavaScript/TypeScript:
[XState](https://xstate.js.org/docs/). This one is extremelly powerful because it allows Statecharts, that's a more complex version of FSMs.

### Behavior Tree

![Behavior Tree Diagram](http://www.plantuml.com/plantuml/png/SoWkIImgAStDuOhMYbNGrRLJ24ujB4tDIqxbueAn57HJyilpW5BXYJdP8Vak-Vb5Q79WRJcfoIL06HW3BkDVa9hdaeyfWx0g7ejKRc9n1efDyN2vqBoS_EHiL4aCs-9nE3-ZAByqFGEx7vnpyejJmL8EgNafm4010000)

Unlike a Finite State Machine, or other systems used for AI programming, a behaviour tree is a tree of hierarchical nodes that control the flow of decision making of an AI entity. At the extents of the tree, the leaves, are the actual commands that control the AI entity, and forming the branches are various types of utility nodes that control the AI’s walk down the trees to reach the sequences of commands best suited to the situation.

The trees can be extremely deep, with nodes calling sub-trees which perform particular functions, allowing for the developer to create libraries of behaviours that can be chained together to provide very convincing AI behaviour. Development is highly iterable, where you can start by forming a basic behaviour, then create new branches to deal with alternate methods of achieving goals, with branches ordered by their desirability, allowing for the AI to have fallback tactics should a particular behaviour fail. This is where they really shine.

If you want to learn more about behavior trees here are a few links:
- [Gamasutra - Behavior trees for AI: How they work](https://www.gamasutra.com/blogs/ChrisSimpson/20140717/221339/Behavior_trees_for_AI_How_they_work.php)
- [Gamasutra - GDC 2005 Proceeding: Handling Complexity in the Halo 2 AI](https://www.gamasutra.com/view/feature/130663/gdc_2005_proceeding_handling_.php)
- [Unreal Engine - Behavior Tree Overview](https://docs.unrealengine.com/en-US/InteractiveExperiences/ArtificialIntelligence/BehaviorTrees/BehaviorTreesOverview/index.html)
- [Towards a Unified Behavior Trees Framework for Robot Control](https://www.csc.kth.se/~miccol/Michele_Colledanchise/Publications_files/2013_ICRA_mcko.pdf)
- [Game AI Pro - The Behavior Tree Starter Kit](http://www.gameaipro.com/GameAIPro/GameAIPro_Chapter06_The_Behavior_Tree_Starter_Kit.pdf)
- [Game AI Pro - Overcoming Pitfalls
in Behavior Tree Design](http://www.gameaipro.com/GameAIPro3/GameAIPro3_Chapter09_Overcoming_Pitfalls_in_Behavior_Tree_Design.pdf)
- [Game AI Pro - A Character Decision-Making
System for FINAL FANTASY XV
by Combining Behavior
Trees and State Machines](http://www.gameaipro.com/GameAIPro3/GameAIPro3_Chapter11_A_Character_Decision-Making_System_for_FINAL_FANTASY_XV_by_Combining_Behavior_Trees_and_State_Machines.pdf)

Some JavaScript/TypeScript implementations:
- [BehaviorTree.js](https://github.com/Calamari/BehaviorTree.js)
- [Modular Behavior Tree](https://github.com/telcy/modular-behavior-tree)
- [Fluent Behavior Tree](https://github.com/aequasi/fluent-behavior-tree)

### GOAP (Goal-Oriented Action Planning)


