# Simple Classes

These are basically simple classes that automatically implement `INotifyPropertyChanged`, which can help when quickly encapsulating portable data and bridging uFrame with other Unity assets, libraries, and frameworks. Class nodes can be used as Element node properties and command parameters, which adds a whole new level of flexibility.

## Defining custom events

Simple classes can be used to define custom events.

![Example of Log Event](images/log_event.png)

Any property given to a Simple Class can then be assigned inside of `Publish()` method.

Events can be published inside of [Controllers](../controller.md), [Views](view-node.md) and [Services](services.md).

To publish the given LogEvent example one would write the following:

```csharp
this.Publish(new LogEvent() {
	Message = "Something happened."
});
```

> Multiple Properties are separated with a ", ".
