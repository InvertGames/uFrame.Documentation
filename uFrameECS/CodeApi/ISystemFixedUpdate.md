# ISystemFixedUpdate
This interface, when added to a system class, will be invoked every fixed update frame.

### Example
```cs
public class MySystem : EcsSystem, ISystemFixedUpdate {
    public void SystemFixedUpdate() {
        Debug.Log("Every Single Fixed Frame");
    }
}
```
## SystemFixedUpdate Method
Called every fixed frame
