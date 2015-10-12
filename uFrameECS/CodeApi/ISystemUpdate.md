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
