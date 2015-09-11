# Elements Commands

Element Commands (known also as ViewModel Commands) allow you to change the state of the ViewModel that the Element represents. If player gets damage, the TakeDamage command can be executed to decrease the player’s health. Usually, ViewModel data should not be changed directly but only through the use of the defined commands. There are however use cases where VMs data can be changed directly from the View with [Scene Properties](../scene-properties.md) or [Services](../services.md).

The actual implementation of the command is not stored inside the ViewModel but inside its [Controller](../controller.md). When a ViewModel’s command gets called, it executes the command’s implementation defined in the controller. The controller then changes the ViewModel data what next triggers [bindings](view-bindings.md) defined in the View.

![](https://dl.dropboxusercontent.com/u/75445779/uFrame_wiki/uFrame_MVVM_flow.png)

## Executing commands
If you have an Element with a `TakeDamage` command, you can call `ExecuteTakeDamage()` from your view.

```csharp
this.ExecuteTakeDamage()
```

Or use a `TakeDamageCommand()` method if you want to execute command on another ViewModel:

```csharp
this.TakeDamageCommand(vm.command[, arg]);
```

You can also execute the command directy on the ViewModel with:

```
MyViewModel.CommandName.OnNext(new {CommandName}Command() { });
```

[explain the code above]

## Reacting to commands

When a command is executed, first called will be a method containing implementation of that command defined in the controller class.

```
public override TakeDamage() {
  // your implementation here
}
```

Then executed will be the `Execute{CommandName}` method in the related view class. You must first add a binding to the view node in the designer.

[picture of a view with binding to the TakeDamage command]

[example]

You can also make a command to be published to the [Event Aggregator](../event-aggregator.md) and this way execute handlers that subscribe to this command.

[explain how to do it]

[add example]

## Publishing Commands

You can make a Command to be published to the [Event Aggregator](../event-aggregator.md) after it is handled by the Controller. In the designer, right-click on the command and select _Publish_ option.

## Command internal implementation

Commands are defined in the [ViewModel](../view-models.md) classes as properties of type [Signal](../classes/signal.md). It makes it possible to subscribe to them and execute custom code when the command is executed.
