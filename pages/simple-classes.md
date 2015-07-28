# Simple Classes
[ More uses ]
## Defining custom events
Simple classes can be used to define custom events.
Any property given to a **Simple Class** can then be assigned inside of `Publish()`
Events can be published inside of [Controllers](controller), [Views](views) and [Services](services).

![Example of Log Event](images/log_event.png)

To publish the given LogEvent example one would write the following
	
    this.Publish(new LogEvent() {
    	Message = "Something happened."
    });
> Multiple Properties are separated with a ", ".
