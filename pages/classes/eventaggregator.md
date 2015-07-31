# Event Aggregator

## FAQ

** Is it possible to call Publish() from some random Monobehaviour?

You can inherit your Monobehaviour from a [uFrameComponent](uframecomponent.md) to get references to event aggregator and also get callbacks for [KernelLoadedEvent](kernelloadedevent.md) and loading [events](events.md).
