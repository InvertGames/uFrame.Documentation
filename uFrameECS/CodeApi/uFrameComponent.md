# uFrameComponent
The uFrameComponent is a simple class that extends from MonoBehaviour, and is directly plugged into the kernel. Use this component when creating any components manually or if you need to plug existing libraries into the uFrame system.public class MyComponent : uFrameComponent { }


## OnEvent Method
Wait for an Event to occur on the global event aggregator.
## Publish Method
Publishes a command to the event aggregator. Publish the class data you want, and let any "OnEvent" subscriptions handle them.
## KernelLoading Method
Before we wait for the kernel to load, even if the kernel is already loaded it will still invoke this before it attempts to wait.
## KernelLoaded Method
The first method to execute when we are sure the kernel has completed loading.
##### NOTE: This documentation has been generated, any edits will likely be overriden, to modify this, modify the doc comments in the source code.
