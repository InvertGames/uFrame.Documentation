# Event Aggregator

## FAQ

** Is it possible to call `Publish()` from some random MonoBehaviour?

You can inherit your MonoBehaviour from a [uFrameComponent](classes/uframecomponent.md) to get references to event aggregator and also get callbacks for [KernelLoadedEvent](classes/kernelloadedevent.md) and loading [events](events.md).
