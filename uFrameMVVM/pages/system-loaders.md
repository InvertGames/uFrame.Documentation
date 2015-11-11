# System Loaders

System Loaders are classes responsible for registering ViewModels and Controllers into [DI Container](di-ioc-container.md) as a part of the [uFrame Kernel](uframe-kernel.md) loading process.

Each [Subsystem](subsystems.md) graph in the designer will have a [Subsystem node](nodes/subsystem-node.md) in it.

![](http://i.imgur.com/16gGYc5.jpg)

Each _Subsystem_ node has a _Instances_ field that allows you to register any ViewModel from any other system into the DI Container.

Subsystem also registers all Controllers and [ViewModelManagers](classes/viewmodelmanager.md) of all Elements existing in the Subsystem by default.

## Generated System Loaders From Subsystems

All subsystem nodes inside a project will generate a _SystemLoader_. These register an instance of every element controller that lives inside of it, as well as any instances defined on it.

Example `SystemLoaders.designer.cs`:

```
public class MainMenuSystemLoaderBase : uFrame.Kernel.SystemLoader {

    public override void Load() {
        Container.RegisterViewModelManager<MenuScreenViewModel>(new ViewModelManager<MenuScreenViewModel>());
        Container.RegisterController<MenuScreenController>(MenuScreenController);
        // Instance add to the Subsystem node in the designer.
        Container.RegisterViewModel<MainMenuRootViewModel>(MainMenuRoot, "MainMenuRoot");
    }
}
```

## Custom System Loaders

In some cases creating a custom system loader can be very useful for different environments. e.g. Dev Environment, Production Environment..etc

To create a custom system loader, derive from SystemLoader, override the load method, and add it to the kernel.

[add code example]

[add picture]
