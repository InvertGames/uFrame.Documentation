# ViewModel

[ViewModel](ViewModel) is a class where all the data associated with a game entity is kept. You can also implement there [Computed Properties](ComputedProperties), initialize [State Machines](ReactiveStateMachines), implement your own serialization methods and define any other methods you need.

## Commands

You can execute commands defined on a ViewModel by using its `OnNext()` method:

```
ViewModel.CommandName.OnNext(new CommandName() { // initialization })
```

or you can use the `Publish` method:

```
ViewModel.YourCommand.Publish(new CommandName() { // initialization } )
```

This works the same way as `CommandName.OnNext()` but it only triggers _CommandName()_ and _CommandNameHandler()_ override methods in the controller.
It does not trigger the handler function of a service, for that you need to use `this.Publish(new CommandName())`` from a controller, service, or a view.
