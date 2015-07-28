# Services vs Controllers

Basically Services are Controllers without ViewModels. Although you may think, every controller functionality could be replaced by services, there are many situations, when using controllers and commands makes best practice over using services and events.

To put it simple:
Element logic that deals only with itself typically = controller usage.
Element logic that deals with other elements and interactions typically = service usage.

So commands are still handled in controllers, but there are also cases where it will be cleaner to let a service listen for and handle that command event (so you would publish it).

For example, a TakeDamage command on an RTSUnit element is typically going to be concerned internally, changing its own RTSUnit props, so the controller would be handling that logic (even though you may *also* want to publish the command to event aggregator as well, so something else can listen and keep track of statistics or something, etc.).

Continuing example, an Attack command on an RTSUnit may be cleaner to handle in a service, because it will most likely concern other parties as well, and in that case there are several specific element types involved... this kind of logic would be cleaner in services, that way if you go to re-use the RTSUnit in a different project, you don't have to drag in a ton of other element types to avoid errors (ie. that RTSUnitAttackService has all the dependencies, so the RTSUnit element is clean of ArcherUnit references, so you can now do SpaceNinjaUnits for a different theme)

So with the new feature of services, you can more easily re-use elements (as long as you follow those patterns) by just writing new services.
