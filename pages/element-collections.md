# Element Collections

Collections can store multiple elements of the same type. For example, you can store references to multiple menu screens that can be displayed/hidden on demand or multiple int values.

You can think of a Collection as a `IList<T>`. In fact, collections are implemented in a more advanced way.

You can create collections on an [Element](elements.md) in the designer.

![](https://dl.dropboxusercontent.com/u/75445779/uFrame_wiki/Screenshot_95.png)

You can subscribe to changes in the collections and execute custom actions when an element is added/removed from the list.

[subscription example]

## Collections of ViewModels

In your game you may want to have an _EnemyManager_ Element that holds references to all enemy ViewModels. To add elements to the collection you should create a [command](element-commands.md) _CreateEnemy_ and implement it in the _EnemyManagerController_.

> This is because it's best to modify ViewModel only through a specialised controller class so that in the feature you could easily find the code that changes the model.

The implementation of the _CreateEnemy_ command in the _EnemyManagerController_ class could look like this:

```csharp
// enemyManager is the VM instance we're modifying.
public override void CreateEnemy (EnemyManagerViewModel enemyManager) {
    base.CreateEnemy (enemyManager);

    // Create ViewModel using extension method.
    var enemy = this.CreateViewModel<EnemyViewModel>();

    // Add new enemy instance to the collection.
    enemyManager.Enemies.Add(enemy); //add the ViewModel to our collection
}
```

![](https://camo.githubusercontent.com/fb848ec664de460f744339463998272129633170/687474703a2f2f692e696d6775722e636f6d2f6f7959745037412e706e67)

## Scene first collections

## Collections as Observable

## Serialization
