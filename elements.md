# Elements

- [Elements](#elements)
	- [Introduction](#introduction)
		- [What an element is?](#what-an-element-is)
		- [What is it for?](#what-is-it-for)
	- [Element Attributes](#element-attributes)
		- [Properties](#properties)
		- [Collections](#collections)
		- [Commands](#commands)
		- [Linking nodes](#linking-nodes)
		- [Context Menu](#context-menu)
	- [Code](#code)
		- [How elements are represented in the code?](#how-elements-are-represented-in-the-code)
		- [ViewModel](#viewmodel)
		- [Controller](#controller)
- [Advanced](#advanced)
	- [Inheritance](#inheritance)

## Introduction

### What an element is?

Elements represent the [ViewModel](ViewModel) in the [MVVM pattern](MVVMPattern). In the game world it can be a player, weapon, menu screen or any other game entity. Elements hold only the data associated with the game’s entity. For example, Player element could contain information about player properties like health, running speed, obtained weapons or actions; Shoot, TakeDamage or Die.

![](https://dl.dropboxusercontent.com/u/75445779/uFrame_wiki/Screenshot_90.png)

Technically, an Element is a node defined in the graph that contains informations used to create ViewModel and Controller classes.

![](https://dl.dropboxusercontent.com/u/75445779/uFrame_wiki/node_to_code_generation.png)

Elements in itself doesn’t contain any logic. They are also completely game engine independent meaning that they have no knowledge of Unity related elements like game objects or components.

Each element can be represented in the game world through a [View](Views). View is a MonoBehaviour that will take the data from an element and represent it in the game. For example, it’ll play the die animation when the player’s health reaches zero.

### What is it for?

You can use it to model the game world entities with their attributes and actions even before making any Unity related stuff. They’ll be later represented visually in the game.

## Element Attributes

#### Properties

Properties are like C# class properties but more powerfull. When changed, they can invoke [State Machine](ReactiveStateMachines) transitions, [Computed Properties](ComputedProperties) and any other method that is subscribed to them (read more in the [Code section](#code)).

You can select the property type from a list:

![](https://dl.dropboxusercontent.com/u/75445779/uFrame_wiki/Screenshot_93.png)

or you can drag another element, [Type Reference](TypeReferences), [Enum](Enums) or [StateMachine](StateMachine) to use its type:

![](https://dl.dropboxusercontent.com/u/75445779/uFrame_wiki/Screenshot_94.png)

Linking a property to a view can make it a [Scene Property](SceneProperties).

Read more about [Properties](Properties)

#### Collections

Collections can store multiple elements of the same type. For example, you can store references to multiple menu screens that can be displayed/hidden on demand.

![](https://dl.dropboxusercontent.com/u/75445779/uFrame_wiki/Screenshot_95.png)

You can subscribe to changes in the collections and execute custom actions when an element is added/removed from the list.

[subscription example]

Read more about [Collections](Collections)

#### Commands

Commands allow to change the state of the ViewModel that the Element represents. If player gets damage, the TakeDamage command can be executed to descrease the player’s health. Usually, the ViewModel data should not be changed directly but only through the use of the defined commands. There are however use cases where VMs data can be changed directly from the View with [Scene Properties](SceneProperties) or [Services](Services).

[link to an example]

The actual implementation of the command is not stored inside the ViewModel but inside its [Controller](Controllers). When a ViewModel’s command gets called, it executes the command’s implementation defined in the controller. The controller then changes the ViewModel data what next triggers [bindings](Bindings) defined in the View.

![](https://dl.dropboxusercontent.com/u/75445779/uFrame_wiki/uFrame_MVVM_flow.png)

Read more about [Commands](Commands)

### Linking nodes

I you make a connection from a property to another node then the node’s type will become type of the property. The same is true for Collections. You can also link a Property to a View and create a [Scene Property](SceneProperties).

If you make a connection from a Command to another node, then the command will have a parameter of a type of that node. If there's no link from the Command, it'll have no parameters.

### Context Menu

You have separate context menu for the node header and its attributes. Most of the options is self-explanatory. Other are explained below:

* **Hide**

    Allows to hide nodes that you don’t want to be on the diagram. You can restore node by clicking on the canvas and selecting node from the Show Item -> <Graph Name> menu. It is important to note, that Hide is not the same as Remove, as Node still present in the system and you can show it in any graph, which supports corresponding node type.

## Code

### How elements are represented in the code?
After creating an Element and recompiling, uFrame will create two editable files: {ElementName}ViewModel and {ElementName}Controller.

Each of those files is empty by default and is intended to by filled with implementation by the user. All the attributes specified in the diagram and the MVVM code are inherited from their base classes.

Read more about [ViewModelBase](ViewModelBase) and [ControllerBase](ControllerBase) base classes.

### ViewModel

[ViewModel](ViewModel) is a class where all the data associated with a game entity is kept. You can also implement there [Computed Properties](ComputedProperties), initialize [State Machines](ReactiveStateMachines), implement your own serialization methods  and define any other methods you need.

### Controller

[Controller](Controller) is created along with the ViewModel and it’s responsible for implementing logic behind the ViewModel. Any command that is specified in the graph Element will be implemented here. Controller is also responsible for creating and initializing ViewModel.

[Add example code]

# Advanced

## Inheritance

You can connect two elements together to make one of the inherit all attributes from the other one. In the example below, SettingsScreen Element will contain the IsActive property and Close command of its parent SubScreen.

![](https://dl.dropboxusercontent.com/u/75445779/uFrame_wiki/Screenshot_97.png)
