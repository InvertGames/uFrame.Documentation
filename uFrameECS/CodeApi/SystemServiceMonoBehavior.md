# SystemServiceMonoBehavior
The base class for all services on the kernel.  Services provide an easy communication layer with the use of the EventAggregator.  You can use this.Publish(new AnyType()).  Or you can use this.OnEvent<AnyType>().Subscribe(anyTypeInstance=>{ }); In services you can also inject any instances that are setup in any of the SystemLoaders.


## Setup Method
This method is to setup an listeners on the EventAggregator, or other initialization requirements.
## SetupAsync Method
This method is called by the kernel to do any setup the make take some time to complete.  It is executed as  a co-routine by the kernel.
##### NOTE: This documentation has been generated, any edits will likely be overriden, to modify this, modify the doc comments in the source code.
