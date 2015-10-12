# EcsComponent
The base class for all ECS components, these components are nothing more than just data.   For the sake of Unity Compatability, it listens for a few Unity messages to make sure the ecs component system is always updated.

## Properties
|Name | Type | Description|
|-----|------|------------|
|EntityId|Int32|The id for the entity that this component belongs to.  This id belongs to the IEcsComponent interface and is used for matching under the hood.|
|IsQuiting|Boolean|A bool variable to determine if the application is quiting, useful in some situations.|
|CachedTransform|Transform|A lazy loaded cached reference to the transform|
|Entity|Entity|The actual entity component that this component belongs to.|


##### NOTE: This documentation has been generated, any edits will likely be overriden, to modify this, modify the doc comments in the source code.
