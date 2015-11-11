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
##### NOTE: This documentation has been generated, any edits will likely be overriden, to modify this, modify the doc comments in the source code.
