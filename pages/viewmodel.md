# ViewModel

[ViewModel](ViewModel) is a class where all the data associated with a game entity is kept. You can also implement there [Computed Properties](ComputedProperties), initialize [State Machines](ReactiveStateMachines), implement your own serialization methods and define any other methods you need.

## Commands

You can execute commands defined on a ViewModel by publishing it like this:

[example with OnNext()]

You can also use the `Publish` method:

```
ViewModel.YourCommand.Publish(new YourCommand() { ...possible initializer... } )
```

This works the same way as `YourCommand.OnNext()` but it only triggers _YourCommand()_ and _YourCommandHandler()_ override methods in the controller.
It does not trigger the handler function of a service, for that you need to use `this.Publish(new YourCommand())`` from a controller, service, or a view.
