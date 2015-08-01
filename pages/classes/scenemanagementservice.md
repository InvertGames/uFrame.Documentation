# SceneManagementService Class

`SceneManagementService` class allows you to load and unload scenes additively. When loading scenes you can pass arguments as [Scene Settings](scene-settings.md) object that will be available to the [Scene Loader](scene-loaders.md) of the loaded scene.

`SceneManagementService` subscribes and handles `LoadSceneCommand`, `UnloadSceneCommand` and `SceneAwakeEvent`. It's also responsible registering all [Scene Loaders](scene-loaders.md) in the DI Container.

It's setup by the [uFrame Kernel](uframe-kernel.md) during kernel load procedure.
