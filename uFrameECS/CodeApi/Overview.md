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
##### NOTE: This documentation has been generated, any edits will likely be overriden, to modify this, modify the doc comments in the source code.
