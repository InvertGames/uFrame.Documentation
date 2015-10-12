# ISystemUpdate
This interface, when added to a system class, will be invoked every fixed update frame.

### Example
```cs
public class MySystem : EcsSystem, ISystemUpdate {
    public void SystemUpdate() {
        Debug.Log("Every Single Frame");
    }
}
```

## SystemUpdate Method
Called every frame
##### NOTE: This documentation has been generated, any edits will likely be overriden, to modify this, modify the doc comments in the source code.
