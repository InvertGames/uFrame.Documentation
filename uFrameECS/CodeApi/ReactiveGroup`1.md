# ReactiveGroup`1
Reactive Group is the base class of all group type components in ECS.


## MatchAndSelect Method
Does the given entity match this group filter, if so it will return the group item, otherwise, it will return null.
## Parameters
|Name | Description|
|-----|------------|
|entityId||
## Install Method
This method is used to determine **when** to check that a entity still belongs to this group. It should also initially store any component managers needed for matching. Ex. If a HealthComponent belongs to a PlayerGroup then it should return ComponentSystem.RegisterComponent'HealthComponent'.CreatedObservable.Select(p=>p.EntityId)  This method is invoked from the EcsComponentService after all groups have been registered.
## Parameters
|Name | Description|
|-----|------------|
|ecsComponentService|The component system that is intalling this reactive-group.|
## UpdateItem Method
Determine's wether or not an entity still belongs to this component or not and adjusts the list accordingly.
## Parameters
|Name | Description|
|-----|------------|
|entityId||
## Select Method
Selects the last successfully matched item.
##### NOTE: This documentation has been generated, any edits will likely be overriden, to modify this, modify the doc comments in the source code.
