# View Service

_View Service_ subscribes to and handles [InstantiateViewCommand](classes/instantiateviewcommand.md), [ViewCreatedEvent](classes/viewcreatedevent.md) and [ViewDestroyedEvent](classes/viewdestroyedevent.md).

On _InstantiateViewCommand_ the service will find a prefab to instantiate (either by direct prefab reference or ViewModel type), instantiate it, set it up, parent under the scene root and return reference to the instantiated View component.

On _ViewCreatedEvent_ the service will add the View instance to an internal list of Views.

On _ViewDestroyedEvent_ the service will dispose the related ViewModel and publish `ViewModelDestroyedEvent`. The View must have _DisposeOnDestroy_ option set to true.
