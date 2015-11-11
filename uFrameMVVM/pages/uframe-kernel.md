# uFrame Kernel

The uFrame Kernel is an essential piece of uFrame, it handles services, subsystems and loading scenes. The kernel is nothing more than a prefab with these types of components attached to it in an organized manner.

![](http://i.imgur.com/5Rg2X25.png)

In the image above you can see the scene _BasicsProjectKernelScene_. This scene will always contain the _BasicsProjectKernel_ prefab and any other things that need to live throughout the entire lifecycle of your application.

You should not add the kernel prefab to your scenes. It'll be added automatically by the [Scene Type](nodes/scene-type-node.md) script attached to your root game object. The kernel game object is set to not destroy when loading other scenes with Unity's `DontDestroyonLoad()` method.

Important Note: All Services, SystemLoaders and SceneLoaders are MonoBehaviours attached to corresponding child game-objects in the kernel prefab.

![](images/Screenshot_113.png)

Whenever a scene begins, uFrame will ensure that the kernel is loaded, if not it will delay its loading mechanism until the kernel has been loaded. This is necessary because you might initialize ViewModels, deserialize them, etc. so the view should not bind until that data is ready.

For convenience uFrame 1.6 makes the process of creating the kernel very easy and straightforward. By pressing the _Scaffold/Update_ Kernel button it will create a scene, and a prefab with all of the types created by the uFrame designer. You can freely modify the kernel, and updating it will only add anything that is not there.

![](http://i.imgur.com/hq0CjJv.jpg)

## Execution order

![](images/kernel_boot_order.png)

When you enter play mode, the Scene Type script will load additively the kernel scene in the `Start()` method. After the kernel game object is added to the scene, its `Awake()` method will call the `Startup()` coroutine.

The `Startup()` method will do:

* Search for all [System Loaders](system-loaders.md) attached to child game objects.
* Execute `Load()` method on each System Loader to register Services in the [DI Container](di-ioc-container.md). `SystemLoaderEvent` with argument `SystemState.Loading` will be published before each load and after with argument `SystemState.Loaded`.
* Search for all [Services](services.md) attached to child game objects.
* Register each service in the [Container](di-ioc-container.md).
* Inject container instances to properties (in all classes) that used the `[Inject]` attribute.
* On each service registered in the Container, call the `Setup()` method and the `SetupAsync()` method. `ServiceLoaderEvent` with argument `SystemState.Loading` will be published before each call to the `Setup()` method and after each `SetupAsync()` with argument `SystemState.Loaded`.
* Publish `SystemsLoadedEvent`.
* Publish `KernelLoadedEvent`.
* Wait two frames.
* Publish `GameReadyEvent`. This event ensures that everything is loaded and bindings have properly taken place on views.
