# uFrame ECS API

|Name |Description|
|-----|------------|
|[EcsComponent](EcsComponent.md)|The base class for all ECS components, these components are nothing more than just data.   For the sake of Unity Compatability, it listens for a few Unity messages to make sure the ecs component system is always updated.|
|[EcsComponentService](EcsComponentService.md)|The main component service used to register and manage all components and groups.|
|[EcsDispatcher](EcsDispatcher.md)|Used for dispatching entity events that come from standard unity messages/events.|
|[EcsSystem](EcsSystem.md)|This is the base class for all systems.  It derives from SystemServiceMonoBehaviour which is part of the uFrame Kernel|
|[ISystemFixedUpdate](ISystemFixedUpdate.md)|This interface, when added to a system class, will be invoked every fixed update frame.|
|[ISystemUpdate](ISystemUpdate.md)|This interface, when added to a system class, will be invoked every fixed update frame.|
##### NOTE: This documentation has been generated, any edits will likely be overriden, to modify this, modify the doc comments in the source code.
