# Services vs Controllers

Basically [Services](services.md) are [Controllers](controller.md) without [ViewModels](view-models.md). Although you may think, every Controller functionality could be replaced by a Service, there are many situations, when using Controllers and Commands make best practice over using Services and Events.

To put it simple:

* Element logic that deals only with itself typically = Controller usage.
* Element logic that deals with other elements and interactions typically = Service usage.

So [Commands](viewmodel-commands.md) are still handled in Controller, but there are also cases where it will be cleaner to let a Service listen for and handle that Command event (so you would publish it, read more on the [Command Node](nodes/command-node.md) page).

For example, a _TakeDamage_ command on an _RTSUnit_ element is typically going to be concerned internally, changing its own _RTSUnit_ properties, so the Controller would be handling that logic (even though you may also want to publish the command to [Event Aggregator](event-aggregator.md) as well, so something else can listen and keep track of statistics or something, etc.).

Continuing example, an _Attack_ Command on an _RTSUnit_ may be cleaner to handle in a Service, because it will most likely concern other parties as well, and in that case there are several specific element types involved... this kind of logic would be cleaner in Services, that way if you go to re-use the _RTSUnit_ in a different project, you don't have to drag in a ton of other element types to avoid errors (ie. that _RTSUnitAttackService_ has all the dependencies, so the _RTSUnit_ element is clean of _ArcherUnit_ references, so you can now do _SpaceNinjaUnits_ for a different theme)

So with the new feature of Services, you can more easily re-use elements (as long as you follow those patterns) by just writing new services.
