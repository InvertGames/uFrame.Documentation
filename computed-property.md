# Computed Properties

# Resources

[uFrame 1.5 - Computed Properties on Youtube](https://www.youtube.com/watch?v=09gPdNbidDs)

# FAQ

#### How to bind Computed Property to a collection change?

This is currently done with code - as you cannot link collections in the uFrame Designer to Computed Properties.

For every Computed Property there exists

	public override IEnumerable<IObservableProperty> Get[ComputedName]Dependents()

Which you can override to define other [Observable Properties](Observable-Property) which the Computed Property is reliant on.

Sources: (Slack)
> there is something like Get<ComputedProperty>Dependents which returns Observables which are used to determinate when to update the computed property

> on your viewmodel, override Get[ComputedName]Dependents()
foreach guys in base.Get[ComputedName]Dependents
2:34
yeild return guys
then yield return viewModel.CollectionProperty 
- sinitreo

Example:
_(Add this to your ViewModel implementation, Replace [ComputedName] with Computed Property Name and [CollectionName] to Collection Property Name you are binding)_

	public override IEnumerable<IObservableProperty> Get[ComputedName]Dependents () {
		foreach ( var dep in base.Get[ComputedName]Dependents ())
    			yield return dep;
  
  		yield return [CollectionName];
	}
