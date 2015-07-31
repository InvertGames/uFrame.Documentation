# Commands

Commands are simple classes with data. You can use them to publish a "command" to do something, for. eg. `LoadSceneCommand` will allow you to load a scene specified in one of this command's properties.

Notice that commands described here are different from [ViewModel Commands](viewmodel-commands.md) and [command nodes](command-node.md).

In technical aspect Commands are the same as [Events](events.md). The only difference is their purpose. Events are to inform about some occurence and Command to trigger some behavior (or multiple behaviors).

## Subscribing to Commands

Commands work with [Event Aggregator](event-aggregator.md). You can subscribe to a command using `OnEvent()` method which is available in [Services](services.md) and any class that derives from [uFrameComponent](uframe-component.md).

This is how the [SceneManagementService](scenemanagementservice.md) class subscribes to the `LoadSceneCommand`.

```csharp
this.OnEvent<LoadSceneCommand>().Subscribe(_ => {
    this.LoadScene(_.SceneName, _.Settings);
});
```

## Publishing Commands

In order to publish a command use the `Publish()` method.

```csharp
Publish.(new LoadSceneCommand() {
    SceneName = "MainMenuScene"
})
```

## Default Commands

`LoadSceneCommand`

[todo add content]

`InstantiateViewCommand`

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
```
