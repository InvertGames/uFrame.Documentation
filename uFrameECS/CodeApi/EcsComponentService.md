# EcsComponentService
The main component service used to register and manage all components and groups.

### Example
```cs
// If you are inside of a system you can access this via 
this.ComponentSystem
// If you are inside of a code handler, you can access it via 
System.ComponentSystem
//If you are anywhere else you can access via 
EcsComponentService.Instance
```
## Properties
|Name | Type | Description|
|-----|------|------------|
|Instance|EcsComponentService|The singleton instance property, this can be accessed from anywhere.|



## RegisterComponent Method
Registers a component type with the component system.
## Parameters
|Name | Description|
|-----|------------|
|componentType|The type of component to register|
### Example
```cs
var componentManager = System.ComponentSystem.RegisterComponent(typeof(PlayerComponent));
foreach (var item in componentManager) {
    Debug.Log(item.name);
}
```
## RegisterComponentInstance Method
This method should be used to add any entity to the ecs component system  > You can use this if you want to register components that aren't derived from EcsComponent which requires MonoBehaviour, but you won't be able to see it in the unity inspector.
## Parameters
|Name | Description|
|-----|------------|
|componentType|The type of component to register.|
|instance|The actual instance that is being registered|
### Example
```cs
System.ComponentSystem.RegisterComponent(typeof(CustomComponent), new CustomComponent());
```
## RegisterGroup Method
Registers a a reactive group with the list of managers.  If the group already exists it will return it, if not it will create a new one and return that.
## Parameters
|Name | Description|
|-----|------------|
|componentId||
##### NOTE: This documentation has been generated, any edits will likely be overriden, to modify this, modify the doc comments in the source code.
