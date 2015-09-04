# SceneManagementService Class

`SceneManagementService` class allows you to load and unload scenes additively. When loading scenes you can pass arguments as [Scene Settings](pages/scene-settings.md) object that will be available to the [Scene Loader](pages/scene-loaders.md) of the loaded scene.

`SceneManagementService` subscribes and handles `LoadSceneCommand`, `UnloadSceneCommand` and `SceneAwakeEvent`. It's also responsible registering all [Scene Loaders](pages/scene-loaders.md) in the DI Container.

It's setup by the [uFrame Kernel](pages/uframe-kernel.md) during kernel load procedure.
