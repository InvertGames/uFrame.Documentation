# Entities (The "E" in ECS)
Entities are nothing more than integer id's, they are used to effeciently keep track of all the entities at runtime and attach components to those entities.

# The Entity Component
When you add any ECS component to any object it will ensure that there is an entity component attached to it.  This is how a component is associated with an Id at runtime.

![](../images/MePCLEc.png)
### A - Entity Proxies
These are mostly useful when you want to trigger an event on an entity from another game object.
An example of this would be detecting a trigger enter event on the player but the collider lives on a child gameobject.

Just drag the "player" gameobject to the "Proxy For" box, and all components on that gameobject will live on the "player" entity.

### B - Entity Id's
- ##### 0 = Auto Assign at runtime
> A lot of times you won't really care what Id the entity has.  But if you do (Ie: LocalPlayer = 1 ..etc) you can specify it manually.  Prefabs should always be left at "0" so that it will be assigned when instantiating, unless you only plan on instantiating it once.

### C - Assigning Components To an Entity
Another way to assign a component to an entity is to just drag it into this field.  By default this is automatically assigned when adding a ecs component to a gameobject to minimize the effort.


# Entity Gotchas
Currently there is a limitation of components and entities, entities can only contain ONE type of a component.  An easy work-around is to just create a new entity for it and create a collection when necessary.
