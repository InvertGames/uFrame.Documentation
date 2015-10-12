# uFrame ECS API

|Name |Description|
|-----|------------|
|[DefaultSceneLoader](DefaultSceneLoader.md)||
|[GameReadyEvent](GameReadyEvent.md)||
|[KernelLoadedEvent](KernelLoadedEvent.md)||
|[LoadSceneCommand](LoadSceneCommand.md)||
|[Scene](Scene.md)|The scene class is used to define a scene as a class,  this MonoBehaviour should live on a gameobject that is at the root level of the scene it is defining. When this type is loaded by unity, it will publish the SceneAwakeEvent.  The SceneManagementService (part of the kernel) will then find the scene loader associated with this scene and invoke its Load Co-Routine method.|
|[SceneAwakeEvent](SceneAwakeEvent.md)|This class is used internally by the Scene class and the kernel to trigger scene loaders load method.|
|[SystemService](SystemService.md)|This class is a generic base class for a systemservice, your probably looking for SystemServiceMonoBehaviour.|
|[SystemServiceMonoBehavior](SystemServiceMonoBehavior.md)|The base class for all services on the kernel.  Services provide an easy communication layer with the use of the EventAggregator.  You can use this.Publish(new AnyType()).  Or you can use this.OnEvent<AnyType>().Subscribe(anyTypeInstance=>{ }); In services you can also inject any instances that are setup in any of the SystemLoaders.|
|[uFrameComponent](uFrameComponent.md)|The uFrameComponent is a simple class that extends from MonoBehaviour, and is directly plugged into the kernel. Use this component when creating any components manually or if you need to plug existing libraries into the uFrame system.public class MyComponent : uFrameComponent { }|
##### NOTE: This documentation has been generated, any edits will likely be overriden, to modify this, modify the doc comments in the source code.
