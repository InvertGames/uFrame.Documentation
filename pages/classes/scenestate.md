# SceneState Enum

_SceneState_ values are used with `SceneLoaderEvent` to define current state that the scene is in.

_SceneState_ values:

* Instantiating

State used while a scene is loaded asynchronously with the Unity `Application.LoadLevelAdditiveAsync()` method. The `SceneLoaderEvent` with this state will be published every 0.1 sec. during the async scene load.

* Instantiated

Instantiated is invoked after new scene is constructed with all it's objects but the Scene Loader wasn't invoked yet.

* Loading

Reserved for future usage.

* Update

This state is used every time you call the `progressDelegate` in the `LoadScene()` method of a [Scene Loader](../scene-loaders.md) class.

* Loaded

State used when a scene finish loading with the `LoadScene()` method of a Scene Loader class.

* Unloading

This state is used every time you call the `progressDelegate` in the `UnloadScene()` method of a [Scene Loader](../scene-loaders.md) class.

* Unloaded

State used when a scene finish unloading with the `UnloadScene()` method of a Scene Loader class.

* Destructed

State used when unloading a scene, after a scene root game object was destroyed with the `Destroy()` method.
