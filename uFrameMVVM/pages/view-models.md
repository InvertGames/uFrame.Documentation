# ViewModel

[ViewModel](view-models.md) is a class where all the data associated with a game entity is kept. You can also implement there [Computed Properties](nodes/computed-property-node.md), initialize [State Machines](nodes/reactive-state-machine-node.md), implement your own serialization methods and define any other methods you need.

## Initializing ViewModel

You can initialize a ViewModel with custom data from the ViewModel's [Controller](controller.md).

## Commands

You can execute commands defined on a ViewModel by using its `OnNext()` method:

```csharp
ViewModel.CommandName.OnNext(new CommandName() { /* initialization */ })
```

or you can use the `Publish` method:

```csharp
ViewModel.YourCommand.Publish(new CommandName() { /* initialization */ } )
```

> It is recommended to always use OnNext, because Publish is not Type-Safe.

To avoid inheritance issues with controllers, `OnNext()` does not publish the command using Event Aggregator. This means that it does not trigger the subscriptions like:

```csharp
OnEvent<YourCommand>().Subscribe(...)
```

If you want your command to be published to the world, you have to mark it with `Publish` flag in the Graph Designer:

![](http://i.imgur.com/CuAou2A.png)

After you save and compile, your command execution will trigger both subscriptions and handler in the controller.

> Command handler in the controller has a first-priority. It will be invoked before any other subscriptions.

## Execution order

ViewModels added to the _Instances_ section in the [Subsystem node](nodes/subsystem-node.md) will be instantiated and added to the [DI Container](di-ioc-container.md) inside the `Setup()` method of the related [System Loader](system-loaders.md).

ViewModels that are not added to the DI Container will be created by the [View Service](view-service.md) along with the Views.

When a View is created with the [InstantiateViewCommand](classes/instantiateviewcommand.md), the command handler inside the `ViewService` will publish a [ViewCreatedEvent](classes/viewcreatedevent.md) which will be again handled by the `ViewService` to create a ViewModel (if does not exists already).

Views that were added to the scene in edit mode will use their `KernelLoaded()` method to publish the `ViewCreatedEvent` and therefore obtain the ViewModel.

## ViewModel (Base Class)

A short description of how a `{Name}ViewModelBase` class is constructed from the designer Element data.

`FillProperties()`

containes a little bit of meta information about all the properties declared on the view model

This is used for inspectors, and also can be used for copying ViewModel and many other things, which require a bit of reflection.

## FAQ

**How do I get a View from just a ViewModel?**

 You don’t, because you’d be breaking the pattern; rethink your design.
 A viewmodel doesn’t directly care about views at all, and is only concerned with its own data, so it doesn’t know if there are any views binding to it. The ViewModel can be thought of as existing outside of any game engine, in a virtual theoretical world, where it is a single object with its own set of properties and actions (commands). Further, when performing actions (commands), it allows its Controller to dictate the necessary logic that needs to happen to itself and the world around it.
