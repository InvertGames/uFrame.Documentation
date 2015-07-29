# Controller

[Controller](Controller) is a class responsible for implementing logic behind the ViewModel. For any number of ViewModels there's only one Controller class instance. Any command that is specified in the graph Element will be implemented here. Controller is also responsible for creating and initializing ViewModel.

[Add example code]

## Creating ViewModels

You can create a new ViewModel simply by using the `Create{ViewModel}()` method. The new instance will be automatically saved to the [ViewModelManager](viewmodel-manager.md) of this Controller.

```csharp
public class MainMenuRootController : MainMenuRootControllerBase {

    public override void InitializeMainMenuRoot(MainMenuRootViewModel viewModel) {
        base.InitializeMainMenuRoot(viewModel);

        var newMainMenuRootViewModelInstance = CreateMainMenuRoot();
    }
}
```

If you want to create an instance of a ViewModel of a different VM type, you must first inject this VMs controller.

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

## ViewModel Manager

By default every controller generates ViewModel manager property that keeps references to all ViewModel instances of the same type.

[add example code and description]
