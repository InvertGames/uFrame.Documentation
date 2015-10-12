# EcsDispatcher
Used for dispatching entity events that come from standard unity messages/events.

### Example
```cs
[UFrameEventDispatcher("On Mouse Down"), uFrameCategory("Unity Messages")]
public class MouseDownDispatcher : EcsDispatcher
{
    public void OnMouseDown()
    {
        Publish(this);
    }
}
```

##### NOTE: This documentation has been generated, any edits will likely be overriden, to modify this, modify the doc comments in the source code.
