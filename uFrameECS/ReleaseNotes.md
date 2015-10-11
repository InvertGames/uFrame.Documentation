# Release Notes
## 1.0 Beta 6
- Feature: Navigation System, back, and forward.
- Feature: Request: Output hinting.
- Feature: Request: Extra use of consistent colours and icons.
- Feature: Collection Item Added & Removed Handlers
- Feature: OnPropertyChanged event handler - expose the  `distinctUntilChanged` property somehow?  So we can say to only fire when value has altered from previous value.
- Feature: Request: Right click on a component presents an option to "Add to selected" gameobject in the heirarchy.  Similar to adding a view to a gameobject in MVVM
- Feature: Request: Make text on top of nodes a bit bigger. Can be a struggle to read sometimes..
- Feature: Request: Jump from Publish of Event to Event Definition
- Feature: Request: Better searching
- Fix: Selecting an event handler after filtering down the event handler list: it ​*adds*​ the node to the db, but in the graph, it is hidden so you can’t see it or interact with it.
- Fix: Cannot delete custom action node
- Feature: Added a millisecond timer action
- Fix: String variable entry box very small.
- Fix: Several node types shouldn't be showing input and/or output sockets. (they never seem to connect to anything and just cause confusion)

## 1.0 Beta 5
- Fix: Some connections not being removed correctly when deleting a node
- Feature: Copy & Paste
- Feature: Inspector's Now properly update property changed handlers.
- Fix: Code Handler Tags not updating when set
- Fix: Pull From command not refreshing the node
- Fix: Multi-selection delete/hide/pull works now
- Fix: Async Actions like "Interval" now work with multiple instances. (New action requirement "AsyncAction" attribute should be added to any action that will do this)
