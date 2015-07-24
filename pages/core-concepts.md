# Core Concepts

With traditional development in Unity, even a small project can sometimes become difficult to keep organized. And while the asset store gives us a fantastic supply of helpful tools and assets at our disposal, they're often mixed-concern components that can be a nightmare to use with each other. uFrame helps keep your game logic organized as your project grows in size by separating different types of logic into different categories. For example, let's say we have a "Player" element in our game. In uFrame that player might look something like this:

![](http://i.imgur.com/STK0vPd.png)

* ViewModel - The objects in your world. Holds your data structure, things like Health, Coins, XP, etc.
* Controller - Handles game/business logic for your commands, like AddHealth(), RemoveCoins(), AddXP(), etc.
* View - The visual/auditory representation of your game object. Binds to the ViewModel for visual/auditory representation in your game

The **ViewModel** and **Controller** live "behind the scenes". They contain pure logic and don't necessarily know what's happening in your scene. The **View** gets attached to your game objects in Unity and are essentially the link between the game world and your data. This is an important concept when using uFrame: **ViewModels** and **Controllers** are pure logic, and your **Views** link them to your Unity game:

![](http://i.imgur.com/6yd1htL.png)

## How the ViewModel, Controller, and View work together
![](http://i.imgur.com/0MShghM.png)

## Typical Flow
1. GameView: UI Button is clicked, ExecutePlay() is called

2. GameViewModel: Sees ExecutePlay() is being called, and asks the GameController what logic to do

3. GameController: Uses the Play(gameViewModel) command, which for example may:
    1. Load a new scene
    2. Execute other commands
    3. Create AIPlayerViewModels, adding them to gameViewModel.AIPlayers
    4. Set the gameViewModel.State property to GameState.Playing, triggering any View collection, and triggering a possible CreateAIPlayersView() binding on the GameView, which invokes InstantiateView(aiPlayer)logic that may be binded to the gameViewModel.State property (ie.- UI transitions, HUD pop-ups, etc.)

4. GameController: Done executing, and commands are void so nothing else to do. Game logic has been carried out, viewmodel properties may have been changed, and viewmodels may have been instantiated (all triggering any further View bindings)

## Okay, I guess that makes sense, but...

**How do I execute commands?**

* From within a view or controller, this.ExecuteCommand(vm.command[, arg]);
* If within a view and executing a command on your own viewmodel, simply:
    this.Execute{CommandName}([arg]); // like this.ExecutePlay(); or just ExecuteJump();

**How do I access a view from a controller, to change sprites or meshes for example?**

* You DON’T! Controllers and Views should not know about each other. They interact loosely through the ViewModel layer, which keeps things clean and less prone to breaking each other.

**How do I get a view from just a ViewModel?**

* You don’t, because you’d be breaking the pattern; rethink your design.
* A viewmodel doesn’t directly care about views at all, and is only concerned with its own data, so it doesn’t know if there are any views binding to it. The ViewModel can be thought of as existing outside of any game engine, in a virtual theoretical world, where it is a single object with its own set of properties and actions (commands). Further, when performing actions (commands), it allows its Controller to dictate the necessary logic that needs to happen to itself and the world around it.
