# Events

## Default Events

`SceneAwakeEvent`

Published after the kernel is loaded (read more on the [Execution Order](execution-order.md) page). It'll pass a reference to the currently active scene type monobehaviour.

`SystemLoaderEvent`

Published before and after a System Loader gets loaded. Read more in [uFrame Kernel](uframe-kernel.md).

`ServiceLoaderEvent`

Published before and after a Service gets setup (including async setup).

`SystemLoadedEvent`

Published after all System Loaders are loaded and Services setup.

`KernelLoadedEvent`

Published after all System Loaders are loaded and Services setup.

`GameReadyEvent`

Published when the kernel finish loading.
