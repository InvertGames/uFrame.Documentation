# Commands

If you look for Element Commands, go to [Element Commands](nodes/element-commands.md).

Commands are simple classes with data. You can use them to publish a "command" to do something, for. eg. `LoadSceneCommand` will allow you to load a scene specified in one of this command's properties.

Notice that commands described here are different from [Element Commands](nodes/element-commands.md) and [Command Nodes](nodes/command-node.md).

In technical aspect Commands are the same as [Events](events.md). The only difference is their purpose. Events are to inform about some occurence and Command to trigger some behavior (or multiple behaviors).

## Subscribing to Commands

Commands work with [Event Aggregator](event-aggregator.md). You can subscribe to a command using `OnEvent()` method which is available in [Services](services.md) and any class that derives from [uFrameComponent](classes/uframecomponent.md).

This is how the [SceneManagementService](classes/scenemanagementservice.md) class subscribes to the `LoadSceneCommand`.

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

* [LoadSceneCommand](classes/loadscenecommand.md)
* [InstantiateViewCommand](classes/instantiateviewcommand.md)
