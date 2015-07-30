# Commands

Description of the _Command_ designer node.

## Default Commands

`InstantiateViewCommand()`

Use it to instantiate prefabs with their own Views already attached. It'll require a reference to a ViewModel that the prefab's View will be connected to. You can also reference the VM by its identifier.

```csharp
public class InstantiateViewCommand {
    public string Identifier { get; set; }
    public IScene Scene { get; set; }
    public ViewModel ViewModelObject { get; set; }
    public ViewBase Result { get; set; }
    public GameObject Prefab { get; set; }
}
```

This command will return a reference to the prefab's view in the _Result_ property.

Example usage:

```csharp
public GameObject Prefab;

var cmd = new InstantiateViewCommand() {
    Prefab = Prefab,
    ViewModelObject = ViewModelReference };

    Publish(cmd);

    // Get the prefab's View.
    var instantiatedView = cmd.Result as BrigandView;

    // Initlalize ViewModel with data specified in the View inspector.
    instantiatedView.InitializeViewModel(ViewModelReference);
```
WWOOP WOPPOP
                new the Slack integration

It not quite correct
Command node has nothing to do with this stuff

I mean, we call InstantiateViewCommand a command, when in reality it is just an event.
Command Node serves a different purspose.
When you add command to the element, you can only select one type to be the argument.
Command node allows you to create complex commands, which can hold any data. But they are still bound to a specific view model. While things like InstantiateViewCommand are just events and are not bound to anything

   I knew that something was not right

   That's how you learn :P

   so, commands are the same as events but with args?


   Not really.

   So basically EVENT is ANY type

   commands CAN BE EVENTS if you publish them

   But the ultimate goal of command, is to pass data from View to the controller, when something happens. (Or from a service from controller)... whatever.


   So command is BOUND to a controller. and it is bound to a ViewModel.

   Anytime you do Vm.MyCommand.OnNext(new MyCommand(){....}) it will invoke handler on the controller of this view model. And at this point commands have nothing to do with events.
   and in fact they do not even use event aggregator.

   however, if you right click on command item (or node) in the graph designer, you will see an option called "publish". So if you check it, uFrame will also generate a small piece of code
   to publish your command just as a regular event. And in this case, any other service can handle it. But the priority always goes to the controller. He is absolutely THE FIRST ONE to handle the command. (in terms of order of execution)

   I understand now :)
   last question is
   why InstantiateViewCommand (ok, now I understand why it's called command and not event)

   the key point here is that InstantiateViewEvent (if you name it like this), semantically it is an expression of order, to instantiate a view. So semantically InstantiateViewCommand is a better name. However, it brings a bit of confusion to a user, which has not aaah... learnt the vocabulary yet. In particular, in my head, I call such events as InstantiateViewCommand a "command" and when it comes to commands on a viewmodel, I call them "viewmodel command". It makes a sufficient difference, at least for mesemant

   now I get it. A bit confusing but ultimately fair enough :)
   ok, I must do a little break before updating the docs.

   Sure take your time and play around with it :)
   afk
   disconnecting :)X
