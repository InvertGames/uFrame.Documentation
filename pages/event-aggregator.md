# Event Aggregator

Event Aggregator is a class that allows you to subscribe to [Events](events.md) and publish them.

Events are nothing more than a simple type with data like this `ServiceLoaderEvent`:

```csharp
public class ServiceLoaderEvent {

    public ServiceState State { get; set; }
    public ISystemService Service { get; set; }
}
```

Read more how to publish and subscribe to events on the [Events](events.md) page.

In the example above, whenever a new [Service](services.md) is loaded, a `ServiceLoaderEvent` with the service's state and a reference is published.

You can subscribe to it from any place in your application to execute custom code.

## FAQ

** Is it possible to call `Publish()` from some random MonoBehaviour?

You can inherit your MonoBehaviour from a [uFrameComponent](classes/uframecomponent.md) to get references to event aggregator and also get callbacks for [KernelLoadedEvent](classes/kernelloadedevent.md) and loading [events](events.md).
