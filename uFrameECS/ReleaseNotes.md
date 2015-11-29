# Release Notes
## 1.0 Beta 9.2
- Fixed deletion issues.
- Fixed custom actions not generating a partial file.
- Other small fixes and enhancements.

## 1.0 Beta 9
- Tons of bug fixes and small feature enhancements.
- Important Change: Code Handlers are now obsolete.  Copy code from your handlers to your systems by overriding the {HandlerName}Handler method.
- Change: Now systems always generate an editable file.

## 1.0 Beta 8
- Feature: Custom Actions can be visualy scripted now.
- Feature: Property changed events now have previous value.
- Performance Enhancement: A new event aggregator has been implemented for drastic performance increases.
- Fix: The event dispatcher UI is confusing
- Fix: Breakpoints remain when set nodes are deleted.
- Feature: Add new component to GO that doesn't have an entity. NRE results - no Entity component is automatically added like in editor.
- Validation for empty groups + Issue types (Warning, Severe)
- Fix: Connectors sometimes behaving oddly
- Fix: The Library Search window's search field is cropped when the uFrame diagram window is small.
- Fix: PropretyChanged Handler - property changed list shows duplicates of properties
- Fix: Pickup and Copy seem to do the same thing, so one of them is redundant.
- Fix: Deleting a string node (without disconnecting it from its input first) doesn't cause the underlying variable reference in the code to go away. (resulting in a compilation error - variable isn't declared anymore)
- Fix: Nodes with dynamic branches that change with the input (such as enum switch) don't refresh unless you collapse and re-expand the node.
- Fix: Component properties exposed in handler nodes behave strangely
- Fix: PropertyChanged Handler - 'OnlyWhenChanged' flag doesn't seem to work with transforms.
- Feature: Request: Allow for a way to use custom/external types as properties of components.
- Fix: OSX Double click issue.
- Fix: When searching the first item should be automatically selected

## 1.0 Beta 7
- Fix: Crash on TreeViewModel.cs:48 - InvalidOperationException due to invoking First() on an empty list
- Fix: Graph Explorer does not refresh when the nodes in a graph change.
- Fix: Check that names of Systems don't match Components?
- Fix: If an output fails to connect to an input, uFrame shouldn't show the node selection window afterwards.
- Fix: Created a handler, save, change to code handler. Error due to duplicate files.  Can ECS deal with this somehow?  Or guide the user?
- Fix: Make sure namespaces are imported correctly on Events, Custom Actions, Components, Systems...etc
- Fix: Enum switch node doesn't direct flow through any case branch.
- Fix: Make a handler with a Unity property (Transform etc) and namespace can't be found in System generated code.
- Fix: Unity crashes when selecting a property in a Property Changed handler.
- Fix: Validation for duplicate properties, collections.
- Fix: Property names should be committed as soon as their text field loses focus.
- Fix: ArgumentException: Getting control 3's position in a group with only 3 controls when doing Repaint Aborting
- Fix: Selecting an event handler after filtering down the event handler list: it ​*adds*​ the node to the db, but in the graph, it is hidden so you can’t see it or interact with it.
- Fix: Can't rename TypeReference node.  Right click to rename, but can't type anything in.
- Fix: When choosing a type for a property, the search box loses focus very easily.
- Fix: Naming nodes inside of Event Handlers doesn't work
- Fix: When saving & compiling a graph, the properties on a component appear to be sorted randomly.
- Feature: Automatic input conversion
- Feature: HideInInspector option on properties
- Feature: Request: move properties in components up and down
- Feature: Add Prop -> Type Select -> Rename
- Feature: Request: Request: Comments.
- Feature: Notes

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
