# Scene Properties

Usually a property binding is a one-way stream of information, where a View is simply receiving information about a property's changing values. In a game environment however, there are times where it makes sense to allow Views to actually determine these values as well, for things like a GameObject's position, rotation, etc. This is where two-way bindings are needed.

![](images/Screenshot_119.png)

Scene Properties are two-way bindings, and allows for a View to calculate and set a property on its ViewModel. For example, adding a Position scene property on a PlayerView will result in these underlying base methods:

* `ResetPosition()`

`ResetPosition()` is mostly used to initialize the binding, is called in the View's base `Bind()` method, and typically doesn't need to be overridden and altered.

* `CalculatePosition()`

`CalculatePosition()` is the main method you would override on your generated PlayerView, where you would return a _Vector3_ to give the player's position.

```csharp
protected override Vector3 CalculatePosition() {
    return transform.position;
}
```

* `GetPositionObservable()`

`GetPositionObservable()` should only be overridden in cases where you have a more convenient or performant method of observing the scene property change, because as you see, this calculation is happening every Update by default. In this case, we know that ViewBase is already monitoring a TransformChangedObservable (and specifically a PositionChangedObservable as well), so on our PlayerView we would override the GetPositionObservable like this:

```csharp
// Unnecessary to use CalculatePosition() in this case, with the further code below.
protected override IObservable<Vector3> GetPositionObservable() {
    // base looks like:
    // return this.UpdateAsObservable().Select(p => CalculatePosition());
    return PositionAsObservable;
}
```
