# Services vs Controllers

Basically Services are Controllers without ViewModels. Although you may think, every controller functionality could be replaced by services, there are many situations, when using Controllers and Commands makes best practice over using Services and Events.

To put it simple:
Element logic that deals only with itself typically = Controller usage.
Element logic that deals with other elements and interactions typically = Service usage.

So commands are still handled in Controllers, but there are also cases where it will be cleaner to let a service listen for and handle that Command event (so you would publish it).

For example, a TakeDamage command on an RTSUnit element is typically going to be concerned internally, changing its own RTSUnit properties, so the controller would be handling that logic (even though you may *also* want to publish the command to event aggregator as well, so something else can listen and keep track of statistics or something, etc.).

Continuing example, an Attack Command on an RTSUnit may be cleaner to handle in a Service, because it will most likely concern other parties as well, and in that case there are several specific element types involved... this kind of logic would be cleaner in Services, that way if you go to re-use the RTSUnit in a different project, you don't have to drag in a ton of other element types to avoid errors (ie. that RTSUnitAttackService has all the dependencies, so the RTSUnit element is clean of ArcherUnit references, so you can now do SpaceNinjaUnits for a different theme)

So with the new feature of Services, you can more easily re-use elements (as long as you follow those patterns) by just writing new services.
