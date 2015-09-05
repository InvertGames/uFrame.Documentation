# Elements

## Introduction

Elements represent the [ViewModel](../view-models.md) in the [MVVM pattern](../mvvm-pattern.md). In the game world it can be a player, weapon, menu screen or any other game entity. Elements hold only the data associated with the game’s entity. For example, Player element could contain information about player properties like health, running speed, obtained weapons or actions; Shoot, TakeDamage or Die.

![](https://dl.dropboxusercontent.com/u/75445779/uFrame_wiki/Screenshot_90.png)

Technically, an Element is a node defined in the graph that contains informations used to create ViewModel and Controller classes.

![](https://dl.dropboxusercontent.com/u/75445779/uFrame_wiki/node_to_code_generation.png)

Elements in itself doesn’t contain any logic. They are also completely game engine independent meaning that they have no knowledge of Unity related elements like game objects or components.

Each element can be represented in the game world through a [View](view-node.md). View is a MonoBehaviour that will take the data from an element and represent it in the game. For example, it’ll play the die animation when the player’s health reaches zero.

## Element Attributes

With the Element attributes (Properties, Collections and Commands) you can specify what kind of data the ViewModel that this Element represents will contain and what actions can be executed on it.

Properties can hold data of specified type. Read more about [Properties](element-properties.md).

Collections can hold multiple elements of the same type. Read more about [Collections](element-collections.md).

Commands allows you to change the state of the ViewModel. Read more about [Commands](element-commands.md).

## Linking Nodes

I you make a connection from a property to another node then the node’s type will become type of the property. The same is true for Collections. You can also link a Property to a View and create a [Scene Property](scene-property-node.md).

If you make a connection from a Command to another node, then the command will have a parameter of a type of that node. If there's no link from the Command, it'll have no parameters.

## Context Menu

You have separate context menu for the node header and its attributes. Most of the options is self-explanatory. Other are explained below:

* _Hide_

    Allows to hide nodes that you don’t want to be on the diagram. You can restore node by clicking on the canvas and selecting node from the `Show Item -> <Graph Name>` menu. It is important to note, that _Hide_ is not the same as _Remove_, as Node still present in the system and you can show it in any graph, which supports corresponding node type.

## Generated Code

After creating an Element and recompiling, uFrame will create two editable files: _{ElementName}ViewModel_ and _{ElementName}Controller_.

Read more about [ViewModels](../view-models.md) and [Controllers](../controller.md).

Each of those files is empty by default and is intended to by filled with implementation by the user. All the attributes specified in the diagram and the MVVM code are inherited from their base classes.

Read more about [ViewModelBase](../classes/viewmodelbase.md) and [ControllerBase](ControllerBase) base classes.

## Inheritance

You can connect two elements together to make one of the inherit all attributes from the other one. In the example below, _SettingsScreen_ Element will contain the _IsActive_ property and _Close_ command of its parent _SubScreen_.

![](https://dl.dropboxusercontent.com/u/75445779/uFrame_wiki/Screenshot_97.png)
