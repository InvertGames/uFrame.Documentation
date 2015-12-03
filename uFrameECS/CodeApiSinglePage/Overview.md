# uFrame ECS API

# uFrame.ECS
|Name |Description|
|-----|------------|
|[AttributeGroup`1](AttributeGroup`1.md)|A Type group reactively keeps up with all components that have a specific attribute defined on the component decleration.|
|[DescriptorGroup`1](DescriptorGroup`1.md)|A Type group reactively keeps up with all components that have a specific base class, or interface implementation. This can be useful for queries such as. GetAllINetworkable, GetAllSerializable|
|[EcsComponent](EcsComponent.md)|The base class for all ECS components, these components are nothing more than just data.   For the sake of Unity Compatability, it listens for a few Unity messages to make sure the ecs component system is always updated.|
|[EcsComponentService](EcsComponentService.md)|The main component service used to register and manage all components and groups.|
|[EcsDispatcher](EcsDispatcher.md)|Used for dispatching entity events that come from standard unity messages/events.|
|[EcsSystem](EcsSystem.md)|This is the base class for all systems.  It derives from SystemServiceMonoBehaviour which is part of the uFrame Kernel|
|[EcsSystemExtensions](EcsSystemExtensions.md)|These extensions are for facilitating the construction of systems. Component Created/Destroyed, Property Changes, Collection Modifications..etc|
|[GroupItem](GroupItem.md)|The base class for all group items, for example ReactiveGroup`TGroupItem`|
|[IEcsComponentManager](IEcsComponentManager.md)|Manages components of a specific type|
|[ISystemFixedUpdate](ISystemFixedUpdate.md)|This interface, when added to a system class, will be invoked every fixed update frame.|
|[ISystemUpdate](ISystemUpdate.md)|This interface, when added to a system class, will be invoked every fixed update frame.|
|[ReactiveGroup`1](ReactiveGroup`1.md)|Reactive Group is the base class of all group type components in ECS.|
# uFrame.Kernel
|Name |Description|
|-----|------------|
|[GameReadyEvent](GameReadyEvent.md)|The game ready event is invoked after the kernel has loaded and two addditional frames have occured.|
|[KernelLoadedEvent](KernelLoadedEvent.md)|This is invoked directly after all scenes of |
|[Scene](Scene.md)|The scene class is used to define a scene as a class,  this MonoBehaviour should live on a gameobject that is at the root level of the scene it is defining. When this type is loaded by unity, it will publish the SceneAwakeEvent.  The SceneManagementService (part of the kernel) will then find the scene loader associated with this scene and invoke its Load Co-Routine method.|
|[SceneAwakeEvent](SceneAwakeEvent.md)|This class is used internally by the Scene class and the kernel to trigger scene loaders load method.|
|[SystemService](SystemService.md)|This class is a generic base class for a systemservice, your probably looking for SystemServiceMonoBehaviour.|
|[SystemServiceMonoBehavior](SystemServiceMonoBehavior.md)|The base class for all services on the kernel.  Services provide an easy communication layer with the use of the EventAggregator.  You can use this.Publish(new AnyType()).  Or you can use this.OnEvent<AnyType>().Subscribe(anyTypeInstance=>{ }); In services you can also inject any instances that are setup in any of the SystemLoaders.|
|[uFrameComponent](uFrameComponent.md)|The uFrameComponent is a simple class that extends from MonoBehaviour, and is directly plugged into the kernel. Use this component when creating any components manually or if you need to plug existing libraries into the uFrame system.public class MyComponent : uFrameComponent { }|
# AttributeGroup`1
A Type group reactively keeps up with all components that have a specific attribute defined on the component decleration.


# DescriptorGroup`1
A Type group reactively keeps up with all components that have a specific base class, or interface implementation. This can be useful for queries such as. GetAllINetworkable, GetAllSerializable


# EcsComponent
The base class for all ECS components, these components are nothing more than just data.   For the sake of Unity Compatability, it listens for a few Unity messages to make sure the ecs component system is always updated.

## Properties
|Name | Type | Description|
|-----|------|------------|
|EntityId|Int32|The id for the entity that this component belongs to.  This id belongs to the IEcsComponent interface and is used for matching under the hood.|
|Enabled|Boolean|Is this component enabled|
|IsQuiting|Boolean|A bool variable to determine if the application is quiting, useful in some situations.|
|CachedTransform|Transform|A lazy loaded cached reference to the transform|
|Entity|Entity|The actual entity component that this component belongs to.|



## CreateObject Method
Creates a new gamobject entity with the component attached to it.
## Parameters
|Name | Description|
|-----|------------|
|componentType|The type of component to add to the game object/entity.|
## CreateObject Method
Creates a new gamobject entity with the component attached to it.
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
# EcsDispatcher
Used for dispatching entity events that come from standard unity messages/events.

### Example
```cs
[UFrameEventDispatcher("On Mouse Down"), uFrameCategory("Unity Messages")]
public class MouseDownDispatcher : EcsDispatcher
{
    public void OnMouseDown()
    {
        Publish(this);
    }
}
```

# EcsSystem
This is the base class for all systems.  It derives from SystemServiceMonoBehaviour which is part of the uFrame Kernel

## Properties
|Name | Type | Description|
|-----|------|------------|
|ComponentSystem|IComponentSystem|Access to the component system.  Use this to get/register components or groups.|
|EventSystem|EcsEventAggregator|The Ecs Event Aggregator, comes with additional features specific to ECS.|



# EcsSystemExtensions
These extensions are for facilitating the construction of systems. Component Created/Destroyed, Property Changes, Collection Modifications..etc


## OnComponentCreated Method
Listens for when a component is created, this also works for groups sinces components and group items both derive from IEcsComponent
## Parameters
|Name | Description|
|-----|------------|
|system||
## OnComponentDestroyed Method
Listens for when a component is destroyed, this also works for groups sinces components and group items both derive from IEcsComponent
## Parameters
|Name | Description|
|-----|------------|
|system||
## PropertyChanged Method
Listens for when a property is changed and ensures that the subscription is properly disposed when the system disposes or when the component disposes.
## Parameters
|Name | Description|
|-----|------------|
|system|This system to install this listner on, it will get disposed with this one.|
|select|A selector for the property observable on TComponentType that you wish to listen for.|
|handler|The method that is invoked when the property changes.|
|getImmediateValue|The lambda method for retreiving the primtive value, if this is not null, it will immedietly invoke the handler with this value.|
|onlyWhenChanged|Only invoke the method when the value actually changes rather than when it is set.|
## PropertyChangedEvent Method
Listens for when a property is changed and ensures that the subscription is properly disposed when the system disposes or when the component disposes.
## Parameters
|Name | Description|
|-----|------------|
|system|This system to install this listner on, it will get disposed with this one.|
|select|A selector for the property observable on TComponentType that you wish to listen for.|
|handler|The method that is invoked when the property changes.|
|getImmediateValue|The lambda method for retreiving the primtive value, if this is not null, it will immedietly invoke the handler with this value.|
|onlyWhenChanged|Only invoke the method when the value actually changes rather than when it is set.|
## CollectionItemAdded Method
Listens for when a Reactive Collection's item has been added, and ensures that the subscription properly disposed when the system disposes or when the component disposes.
## Parameters
|Name | Description|
|-----|------------|
|system|This system to install this listner on, it will get disposed with this one.|
|select|The ReactiveCollection selector.|
|handler|The method that is invoked when an item is added to the collection|
|immediate|Should the handler be invoked for every item that is currently in the list?|
## CollectionItemRemoved Method
Listens for when a Reactive Collection's item has been removed, and ensures that the subscription properly disposed when the system disposes or when the component disposes.
## Parameters
|Name | Description|
|-----|------------|
|system|This system to install this listner on, it will get disposed with this one.|
|select|The ReactiveCollection selector.|
|handler|The method that is invoked when an item is added to the collection|
# GroupItem
The base class for all group items, for example ReactiveGroup`TGroupItem`

## Properties
|Name | Type | Description|
|-----|------|------------|
|Enabled|Boolean|Is this component enabled|
|EntityId|Int32|The entity id for the entity this group item belongs to|
|Entity|Entity|The entity object that this groupitem belongs to|



# IEcsComponentManager
Manages components of a specific type


# ISystemFixedUpdate
This interface, when added to a system class, will be invoked every fixed update frame.

### Example
```cs
public class MySystem : EcsSystem, ISystemFixedUpdate {
    public void SystemFixedUpdate() {
        Debug.Log("Every Single Fixed Frame");
    }
}
```

## SystemFixedUpdate Method
Called every fixed frame
# ISystemUpdate
This interface, when added to a system class, will be invoked every fixed update frame.

### Example
```cs
public class MySystem : EcsSystem, ISystemUpdate {
    public void SystemUpdate() {
        Debug.Log("Every Single Frame");
    }
}
```

## SystemUpdate Method
Called every frame
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
# GameReadyEvent
The game ready event is invoked after the kernel has loaded and two addditional frames have occured.


# KernelLoadedEvent
This is invoked directly after all scenes of 


# Scene
The scene class is used to define a scene as a class,  this MonoBehaviour should live on a gameobject that is at the root level of the scene it is defining. When this type is loaded by unity, it will publish the SceneAwakeEvent.  The SceneManagementService (part of the kernel) will then find the scene loader associated with this scene and invoke its Load Co-Routine method.

## Properties
|Name | Type | Description|
|-----|------|------------|
|KernelScene|String|The kernel scene property is so that this scene can load the correct kernel if it hasn't been loaded yet.|
|DefaultKernelScene|String|The default kernel scene is what is used if the "KernelScene" property is not set.  This is really used by the uFrame designer to remove the extra step of specifying the kernel scene each time a scene is created.|
|Name|String|The Name of this scene, this is set by the kernel so that it can reference back to it and destroy it when the Unload Scene Command is fired.|
|_SettingsObject|ISceneSettings|If this scene was loaded via a |



## Start Method
In this class we override the start method so that we can trigger the kernel to load if its not already.
# SceneAwakeEvent
This class is used internally by the Scene class and the kernel to trigger scene loaders load method.


# SystemService
This class is a generic base class for a systemservice, your probably looking for SystemServiceMonoBehaviour.


# SystemServiceMonoBehavior
The base class for all services on the kernel.  Services provide an easy communication layer with the use of the EventAggregator.  You can use this.Publish(new AnyType()).  Or you can use this.OnEvent<AnyType>().Subscribe(anyTypeInstance=>{ }); In services you can also inject any instances that are setup in any of the SystemLoaders.


## Setup Method
This method is to setup an listeners on the EventAggregator, or other initialization requirements.
## SetupAsync Method
This method is called by the kernel to do any setup the make take some time to complete.  It is executed as  a co-routine by the kernel.
# uFrameComponent
The uFrameComponent is a simple class that extends from MonoBehaviour, and is directly plugged into the kernel. Use this component when creating any components manually or if you need to plug existing libraries into the uFrame system.public class MyComponent : uFrameComponent { }


## OnEvent Method
Wait for an Event to occur on the global event aggregator.
## Publish Method
Publishes a command to the event aggregator. Publish the class data you want, and let any "OnEvent" subscriptions handle them.
## KernelLoading Method
Before we wait for the kernel to load, even if the kernel is already loaded it will still invoke this before it attempts to wait.
## KernelLoaded Method
The first method to execute when we are sure the kernel has completed loading.
##### NOTE: This documentation has been generated, any edits will likely be overriden, to modify this, modify the doc comments in the source code.
