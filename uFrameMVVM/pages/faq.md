# FAQ

## How to set custom scene lighting settings for each scene?

To each scene add a custom MonoBehaviour will all the lighting settings.

Create a [Service](services.md) and in it subscribe to the [SceneLoaderEvent](scene-loaders.md). On _SceneLoaderEvent_ with `State` property equal to `Loaded` search the scene hierarchy for the MonoBehaviour and use it to modify the scene lighting settings.

Alternatively, you could omit the Service and setup the lighting setting in the MonoBehaviour `Start()` method.
