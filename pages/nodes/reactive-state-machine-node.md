# Reactive State Machines

A State Machine is a good way to express high level logic using the diagram. It can be used in a lot of different ways. For example, AI logic. uFrame has built in support for State Machines.

The main concept of state machines in uFrame MVVM is that when defining them in your diagrams you don't need to focus on anything other than the states and transitions that make up the machine. Making the machine come to life is a matter of wiring them to computed properties, command bindings, and view bindings. But initially, you can focus purely on the high-level rather than implementation, once commands and transitions are all wired together transitions and states can be easily tweaked with minimal to zero changes in your code.

Reactive State Machines employ the concept of data subscriptions causing transitions, rather than polling data every frame, this can give a definite increase in performance.

![](http://i.imgur.com/CPymbPH.png)

To set a transition in code, you'll need to access the state machine property. This should be the property that is connected to your state machine.

```csharp
{ViewModel}.{StateMachinePropertyName}Property.Transition("NAME OF TRANSITION HERE");
```

You can trigger the machine state the `SetState()` method:

```csharp
{ViewModel}.{StateMachinePropertyName}Property.SetState<STATE_CLASS_NAME>();
```

or

```csharp
{ViewModel}.{StateMachinePropertyName}Property.SetState("STATE_NAME");
```
