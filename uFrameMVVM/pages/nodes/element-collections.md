# Element Collections

Collections can store multiple elements of the same type. For example, you can store references to multiple ViewModels or int values.

You can think of a Collection as a `IList<T>`. In reality, collections are implemented in a more advanced way, as a [ModelCollection](../classes/modelcollection.md) type.

You can create collections on an [Element](element-node.md) in the designer.

![](https://dl.dropboxusercontent.com/u/75445779/uFrame_wiki/Screenshot_95.png)

You can subscribe to changes in the collections and execute custom actions when an element is added/removed from the list. See subscription example in the _Collections as Observables_ section.

## Collections of ViewModels

In your game you may want to have an _EnemyManager_ Element that holds references to all enemy ViewModels. To add elements to the collection you could create a [Command](element-commands.md) _CreateEnemy_ and implement it in the _EnemyManagerController_.

> This is because it's best to modify ViewModel only through a specialised controller class so that in the feature you could easily find the code that changes the model.

![](images/Screenshot_116.png)

The implementation of the _CreateEnemy_ command in the _EnemyManagerController_ class could look like this:

```csharp
// enemyManager is the VM instance we're modifying.
public override void CreateEnemy (EnemyManagerViewModel enemyManager) {
    base.CreateEnemy (enemyManager);

    // Create ViewModel using extension method.
    var enemy = this.CreateViewModel<EnemyViewModel>();

    // Add new enemy instance to the collection.
    enemyManager.Enemies.Add(enemy);
}
```

## Instantiating Views

If you add new enemy ViewModel to the collection, you may also want to instantiate a View for this VM on the scene.

You can achieve that by creating a binding on the View node, with the name _Enemies View Collection Changed_.

![](images/Screenshot_115.png)

It'll create a `CreateEnemyView()` method in the View class
that will be called every time a new enemy VM is added to the collection.

```csharp
/// This binding will add or remove views based on an element/viewmodel collection.
public override ViewBase CreateEnemyView(EnemyViewModel item) {
    // Here you can specify a prefab to use, a ViewModel to associate with,
    // and a starting position the prefab must be located in a Resources folder
    // and have an EnemyView attached.
    var view = InstantiateView(item);

    return view;
}
```

Here your prefab must:

* Have a name that matches the name of your Element, for example _Enemy_.
* Be located in a Resources folder. uFrame will try to find a prefab with a name that matches your element.
* Have a View attached with the name _ElementNameView_, for example _EnemyView_.

You can also specify a prefab manually:

```csharp
/// This binding will add or remove views based on an element/viewmodel collection.
public override ViewBase CreateEnemyView(EnemyViewModel item) {
    // Prefab must have an EnemyView attached.
    var prefab = (GameObject)Resources.Load("SomeFolderWithinResources/PathToPrefab");
    var view = InstantiateView(prefab, item);
    return view;
}
```

On your _EnemyManagerView_ you can then specify a parent for your game objects to spawn under.

![](images/Screenshot_114.png)

The _ViewFirst_ checkbox should be unchecked since you'll be instantiating VMs through code. Read more on the [Instantiation scenarios and methods](../instantiation-scenarios-and-methods.md) page.

## Scene First Collections

Suppose you already have multiple enemy game object on your scene. Each has a _EnemyView_ component attached. If you select the _ViewFirst_ checkbox on the _EnemyManagerView_ (see pic. above) then all those game objects will be added to the collection when the game starts (in the `Bind()` method). Notice: All of the game object must be parented under the game object specified in the _Parent_ field.

## Removing Views

When a enemy VM is removed from the collection, and you have the _Enemies View Collection Changed_ View binding in place, in the View class you'll have a `EnemiesRemoved()` method. It'll be called when a VM gets removed from the collection. Use it to remove the View along with its game object.

```csharp
public override void EnemiesRemoved(uFrame.MVVM.ViewBase view) {
    Destroy(view.gameObject);
}
```

## Collections as Observables

You can subscribe to a Collection with `ViewModelReference.CollectionName.Subscribe( _ => /* some handler code */)`.

Here's an example how to subscribe to a _MainMenuRoot_ VM from a service:

```csharp
public override void Setup() {
    // Every time new Screen is added to the Screens collection, invoke ScreenAdded()
    // and pass the new screen.
    MainMenuRoot.Screens
        .Where(_ => _.Action == NotifyCollectionChangedAction.Add)
        .Select(_ => _.NewItems[0] as SubScreenViewModel)
        .Subscribe(ScreenAdded);
}

private void ScreenAdded(SubScreenViewModel screen) {
    // If screen is of current type, activate it; else deactivate it.
    screen.IsActive = MainMenuRoot.CurrentScreenType == screen.GetType();
}
```
