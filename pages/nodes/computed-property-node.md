# Computed Properties

![](http://i.imgur.com/qeDjrVR.png)

A Computed Property is an extremely powerful feature added to uFrame's ViewModel layer. These properties can calculate their value based on other properties, and are recalculated whenever one of those change. So for example, if you have a boolean IsDead computed property on a PlayerViewModel, dependent on a Player.Health property, uFrame will generate these three things on that PlayerViewModel for you to override:

* ResetIsDead

Usually fine without overriding, used to set up the computed observable.

* ComputeIsDead

A "getter" used to compute the property, based on dependents.

* GetIsDeadDepedents

This where you would override and provide additional dependents, if needed.

Note: It's important to correctly set all the dependents, because the computed only knows when it needs to be recalculated by observing these dependents for changes. This is done in the diagram by dragging a link from a ViewModel's property to the computed, or in code by overriding the Get{ComputedName}Dependents() function.

# FAQ

#### How to bind Computed Property to a collection change?

This is currently done with code - as you cannot link collections in the uFrame Designer to Computed Properties.

For every Computed Property there exists

`public override IEnumerable<IObservableProperty> GetComputedNameDependents()`

which you can override to define other [Observable Properties](../classes/signal.md) which the Computed Property is reliant on.

Example:
Add this to your ViewModel implementation, replace _ComputedName_ with the computed property name and _CollectionName_ with collection property name you are binding to.

```
public override IEnumerable<IObservableProperty> GetComputedNameDependents() {
    foreach ( var dep in base.Get[ComputedName]Dependents()) {
        yield return dep;
    }

    yield return [CollectionName];
}
```

# Resources

[uFrame 1.5 - Computed Properties on Youtube](https://www.youtube.com/watch?v=09gPdNbidDs)
