# Simple Classes

[ More uses ]

## Defining custom events

Simple classes can be used to define custom events. Any property given to a Simple Class can then be assigned inside of `Publish()` method.

Events can be published inside of [Controllers](controller.md), [Views](views.md) and [Services](services.md).

![Example of Log Event](images/log_event.png)

To publish the given LogEvent example one would write the following

```csharp
this.Publish(new LogEvent() {
	Message = "Something happened."
});
```

> Multiple Properties are separated with a ", ".
