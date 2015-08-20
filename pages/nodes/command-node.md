# Command Node

When you add command to the element, you can only select one type to be the argument. Command node allows you to create complex commands, which can hold any data.

![](images/Screenshot_118.png)

```csharp
public partial class ApplyDamageCommand : ViewModelCommand {

    private Int32 _DamageValue;
    private List<SideEffect> _SideEffects;

    public Int32 DamageValue {
        get {
            return _DamageValue;
        }
        set {
            _DamageValue = value;
        }
    }

    public List<SideEffect> SideEffects {
        get {
            return _SideEffects;
        }
        set {
            _SideEffects = value;
        }
    }
}
```

The main difference between a [Command](../commands.md) and a Command Node is that Command can be published to [Event Aggregator](../event-aggregator.md) and handled by any class that subscribes to it. While Node Commands are only used to pass data to the Controller.

You can make Command Node behave like a Command by right-clicking on the Command entry in the Element node and selecting the _Publish_ option. It'll make the command to be published as a regular Event which can be handled by any Service.

> In this case the execution order is that the Controller handler gets always executed first. Then any subscribed services.
