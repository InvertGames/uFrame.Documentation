# InstantiateViewCommand

Use it to instantiate prefabs with their own Views already attached. It'll require a reference to a ViewModel that the prefab's View will be connected to. You can also reference the VM by its identifier.

```csharp
public class InstantiateViewCommand {
    public string Identifier { get; set; }
    public IScene Scene { get; set; }
    public ViewModel ViewModelObject { get; set; }
    public ViewBase Result { get; set; }
    public GameObject Prefab { get; set; }
}
```

This command will return a reference to the prefab's view in the _Result_ property.

Example usage:

```csharp
public GameObject Prefab;

var cmd = new InstantiateViewCommand() {
    Prefab = Prefab,
    ViewModelObject = ViewModelReference };

    Publish(cmd);

    // Get the prefab's View.
    var instantiatedView = cmd.Result as BrigandView;

    // Initlalize ViewModel with data specified in the View inspector.
    instantiatedView.InitializeViewModel(ViewModelReference);
```In order to initialize a ViewModel with the View's inspector data, use `View.InitializeData(ViewModel)` on the instantiated View.