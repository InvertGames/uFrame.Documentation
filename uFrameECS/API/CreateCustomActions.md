# Creating Custom Actions

## Static Function Libraries
```cs
using uFrame.Attributes;
using UnityEngine;

[ActionLibrary, uFrameCategory("GameObject")]
public static class GameObjects
{
    [ActionTitle("Instantiate GameObject")]
    public static GameObject Instantiate(GameObject gameObject, Vector3 position, Vector3 rotation)
    {
        return Object.Instantiate(gameObject,
          position, Quaternion.Euler(rotation)) as GameObject;
    }

    [ActionTitle("Destroy GameObject")]
    public static void DestroyGameObject(GameObject gameObject)
    {
        Object.Destroy(gameObject);
    }
}
```

## Custom Action Classes
```cs
using uFrame.Attributes;
using UnityEngine;

[ActionTitle("Interval By Seconds"), uFrameCategory("Timers"), AsyncAction]
public class IntervalBySeconds : UFAction
{

    [In]
    public float Seconds;

    [In]
    public IEcsComponent DisposeWith;

    [Out]
    public Action Tick;

    [Out]
    public IDisposable Result;

    public override void Execute()
    {

        Result = Observable.Interval(TimeSpan.FromSeconds(Seconds)).Subscribe(_ =>
        {
            Tick();
        }).DisposeWith(System);
        if (DisposeWith != null)
        {
            Result.DisposeWith(DisposeWith);
        }
    }
}
```

## Async Action Attribute
The "AsyncAction" attribute is required on actions that invoke an action branch outside of the current execution frame. In the above example "InvervalBySeconds", it tags the attribute because the "Tick" output branch is invoked after a few seconds and not immediately.

## Creating Type Converters
Type converters make it easy to connect various out and ins and minimizing the effort of converting to a necessary type.  This can easily be done in uFrame. To create them it must exist as a static library action.  The following example's shows how you can create your own converters.
```cs
[ActionLibrary, uFrameCategory("Convert")]
public static class MyConverterLib
{
    [ActionTypeConverter()]
    public static GameObject ConvertToObject(string name)
    {
        return GameObject.FindByName(name);
    }
}
```
