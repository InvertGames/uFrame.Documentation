# Controller

[Controller](Controller) is a class responsible for implementing logic behind the ViewModel. For any number of ViewModels there's only one Controller class instance. Any command that is specified in the graph Element will be implemented here. Controller is also responsible for creating and initializing ViewModel.

[Add example code]

## Creating ViewModels

You can create a new ViewModel simply by using the `Create{ViewModel}()` method. The new instance will be automatically saved to the [ViewModelManager](viewmodel-manager.md) of this Controller.

```csharp
public class MainMenuRootController : MainMenuRootControllerBase {

    public override void InitializeMainMenuRoot(MainMenuRootViewModel viewModel) {
        base.InitializeMainMenuRoot(viewModel);

        var newInstance = CreateMainMenuRoot();
    }
}
```

If you want to create an instance of a ViewModel of a different VM type, you can do this with an extension method:

```csharp
public class MainMenuRootController : MainMenuRootControllerBase {

    public override void InitializeMainMenuRoot(MainMenuRootViewModel viewModel) {
        base.InitializeMainMenuRoot(viewModel);

        var newInstance = this.CreateViewModel<SubScreenViewModel>();
    }

}
```

It'll use the correct VMs controller to create the instance. It is important that you use this method because the controller will initialize the commands on the viewmodel to point to the correct handlers on itself.

You can also inject the correct Controller and use it to create an instance:

```csharp
public class MainMenuRootController : MainMenuRootControllerBase {

    [Inject]
    private SubScreenController SubScreenController;

    public override void InitializeMainMenuRoot(MainMenuRootViewModel viewModel) {
        base.InitializeMainMenuRoot(viewModel);

        // Use injected controller to create new VM instance.
        var newScreen = SubScreenController.CreateSubScreen();

        // You can now add this instance to a collection if you want.
        // Here we have a Screens collection with references to all the screens used in the game's main menu.
        viewModel.Screens.Add(newScreen);
    }
}
```

## Initializing ViewModels

Typically you will use the relevant Controller's `Initialize{ElementName}`` method to initialize a newly created ViewModel with default values and references. It's a great place to subscribe to state changes and [Scene Property](scene-properties.md) changes, or possibly track a list of ViewModel instances ie. acting similarly to a [ViewModel Manager](viewmodel-manager.md).

For convenience, you also have the option of Initializing a ViewModel from a particular View, by checking Initialize ViewModel on the View. This is particularly useful when setting up a scene before runtime or creating prefabs. 

## ViewModel Manager

By default every controller generates ViewModel manager property that keeps references to all ViewModel instances of the same type.

[add example code and description]
