# Events

Events are simple classes that contain simple data. It's similar to C# `EventArgs` class that you use to pass data along with the events.

Example event used to pass a reference of a newly created enemy to a subscriber (for eg. a Service).

```csharp
public class EnemyCreatedEvent {

    // Reference to a single Enemy instance.
    public Enemy;
}
```

Example subscription to the `EnemyCreatedEvent` inside the `EnemiesService` class.

```csharp
using UniRx; // This is required for the extension method Subscribe

public List<Enemy> Enemies;

public override void Setup() {
    base.Setup();

    // Adds new enemy to the Enemies list.
    OnEvent<EnemyCreatedEvent>().Subscribe(evt => {
        Enemies.Add(evt.Enemy)
    })
}
```

## Default Events

* `SceneAwakeEvent`

Published after the kernel is loaded (read more on the [Execution Order](execution-order.md) page). It'll pass a reference to the currently active scene type monobehaviour.

* `SystemLoaderEvent`

Published before and after a System Loader gets loaded. Read more in [uFrame Kernel](uframe-kernel.md).

* `ServiceLoaderEvent`

Published before and after a Service gets setup (including async setup).

* `SystemLoadedEvent`

Published after all System Loaders are loaded and Services setup.

* `KernelLoadedEvent`

Published after all System Loaders are loaded and Services setup.

* `GameReadyEvent`

Published when the kernel finish loading.

* `SceneAwakeEvent`

This event is published in the `Start()` method of every [Scene Type](nodes/scene-type-node.md) MonoBehaviour. It'll pass the scene name as an argument. It'll be published only after the [Kernel](uframe-kernel.md) if fully loaded. Check `Scene.Start()` method for implementation details.

* `SceneLoaderEvent`

An event published by the `SceneManagementService` and `uFrameKernel` classes. `SceneLoaderEvent` has a `State` property of type [SceneState](classes/scenestate.md) that holds the current state of the scene.

* `ViewCreatedEvent`

[todo add content]

* `ViewModelCreatedEvent`

Published by Controller when new ViewModel is created with `Create()` method.

* `ViewModelDestroyedEvent`
