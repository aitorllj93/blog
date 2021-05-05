---
published: true
author: aitor
---
![Patterns in Videogames I&#58; Architectural Patterns]({{site.baseurl}}/images/patterns-in-videogames-i-architectural-patterns.png)
As both a software developer and a gamer, I've always been interested on differences between software and game development. In this article series I will cover some software patterns I find interesting from the game development industry.

### Game Loop

![Game Loop](http://www.plantuml.com/plantuml/png/SoWkIImgAStDuT8eACfFJYqkzimhA2tHLD2rKz0eBKX9B4dLv88o8895gNafgKL0GGW3B8Ycrqh1p22je9ZB0QW2Q3G0)

When talking about game development patterns the most important one is the Game Loop. Every videogame runs inside a Game Loop. Unlike most other software, games keep moving even when the user isn’t providing input. For this reason, we need to make a loop that processes user input, but doesn’t wait for it. 

A game loop runs continuously during gameplay. Each turn of the loop, it processes user input without blocking, updates the game state, and renders the game. It tracks the passage of time to control the rate of gameplay.

[Read more about this pattern](http://gameprogrammingpatterns.com/game-loop.html)

### Object Pool 
![Object Pool](http://www.plantuml.com/plantuml/png/XSqn2i9048NXVaynfB7o0Y8XeBqMNc2IVMhCxaoOsKLoUmE2HHhx_tklyy9MQax1fr5cDixqWC36uJpwd7WXJJ6SLBnc6ErRQYxz2qEvggRZhMteGEoJqrsD3DMvJ_WMOLkk-3UqCaEf9hvHVxuC2UpOAde_8HGLBhj20aHYMR8QGWU9DQSF)

The object pool tries to improve performance and memory use by reusing objects from a fixed pool instead of allocating and freeing them individually.

Define a pool class that maintains a collection of reusable objects. Each object supports an “in use” query to tell if it is currently “alive”. When the pool is initialized, it creates the entire collection of objects up front (usually in a single contiguous allocation) and initializes them all to the “not in use” state.

When you want a new object, ask the pool for one. It finds an available object, initializes it to “in use”, and returns it. When the object is no longer needed, it is set back to the “not in use” state. This way, objects can be freely created and destroyed without needing to allocate memory or other resources.

This pattern is used widely in games for obvious things like game entities and visual effects, but it is also used for less visible data structures such as currently playing sounds. Use Object Pool when:
* You need to frequently create and destroy objects.
* Objects are similar in size.
* Allocating objects on the heap is slow or could lead to memory fragmentation.
* Each object encapsulates a resource such as a database or network connection that is expensive to acquire and could be reused.

[Read more about this pattern](http://gameprogrammingpatterns.com/object-pool.html)

### Entity Component System

ECS ("Entity Component System") describes a design approach which promotes code reusability by separating data from behavior. Data is often stored in cache-friendly ways which benefits performance. An ECS has the following characteristics:

* It has entities, which are unique identifiers
* It has components, which are plain datatypes without behavior
* Entities can contain zero or more components
* Entities can change components dynamically
* It has systems, which are functions matched with entities that have a certain set of components

[Read more about this pattern](https://github.com/SanderMertens/ecs-faq)
