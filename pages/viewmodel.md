# ViewModel

[ViewModel](ViewModel) is a class where all the data associated with a game entity is kept. You can also implement there [Computed Properties](ComputedProperties), initialize [State Machines](ReactiveStateMachines), implement your own serialization methods and define any other methods you need.

## Commands

You can execute commands defined on a ViewModel by using its `OnNext()` method:

```cs
ViewModel.CommandName.OnNext(new CommandName() { // initialization })
```

or you can use the `Publish` method:

```cs
ViewModel.YourCommand.Publish(new CommandName() { // initialization } )
```

> It is recommended to always use OnNext, because Publish is not Type-Safe. 

To avoid inheritance issues with controllers, `OnNext()` does not publish the command using Event Aggregator. This means that it does not trigger the subscriptions like:

```cs
OnEvent<YourCommand>().Subscribe(...)
```

If you want your command to be published to the world, you have to mark it with `Publish` flag in the Graph Designer:

![](http://i.imgur.com/CuAou2A.png)

After you save and compile, your command execution will trigger both subscriptions and handler in the controller.

> Command handler in the controller has a first-priority. It will be invoked before any other subscriptions.
