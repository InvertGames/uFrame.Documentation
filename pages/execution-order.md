# Execution Order

## Scene Type

Each scene to be handled by uFrame should have a [Scene Type](pages/scene-types.md) script attached to the root game object. When you enter play mode, the `Start()` method of the scene type will be executed first.

`Start()` is responsible for loading the uFrame Kernel. You can override the `KernelLoading()` method to execute custom code before the kernel starts loading and the `KernelLoaded()` to execute custom code after the kernel finish loading.

After the kernel is loaded, a `SceneAwakeEvent` is published. Read more in the [Events](events.md) page.

## Kernel

Read about kernel execution order in [uFrame Kernel](kernel.md) page.

## Views

To see the execution order on views check the [Views](pages/views.md) page.
