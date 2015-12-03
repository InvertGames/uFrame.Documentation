# EcsSystemExtensions
These extensions are for facilitating the construction of systems. Component Created/Destroyed, Property Changes, Collection Modifications..etc


## OnComponentCreated Method
Listens for when a component is created, this also works for groups sinces components and group items both derive from IEcsComponent
## Parameters
|Name | Description|
|-----|------------|
|system||
## OnComponentDestroyed Method
Listens for when a component is destroyed, this also works for groups sinces components and group items both derive from IEcsComponent
## Parameters
|Name | Description|
|-----|------------|
|system||
## PropertyChanged Method
Listens for when a property is changed and ensures that the subscription is properly disposed when the system disposes or when the component disposes.
## Parameters
|Name | Description|
|-----|------------|
|system|This system to install this listner on, it will get disposed with this one.|
|select|A selector for the property observable on TComponentType that you wish to listen for.|
|handler|The method that is invoked when the property changes.|
|getImmediateValue|The lambda method for retreiving the primtive value, if this is not null, it will immedietly invoke the handler with this value.|
|onlyWhenChanged|Only invoke the method when the value actually changes rather than when it is set.|
## PropertyChangedEvent Method
Listens for when a property is changed and ensures that the subscription is properly disposed when the system disposes or when the component disposes.
## Parameters
|Name | Description|
|-----|------------|
|system|This system to install this listner on, it will get disposed with this one.|
|select|A selector for the property observable on TComponentType that you wish to listen for.|
|handler|The method that is invoked when the property changes.|
|getImmediateValue|The lambda method for retreiving the primtive value, if this is not null, it will immedietly invoke the handler with this value.|
|onlyWhenChanged|Only invoke the method when the value actually changes rather than when it is set.|
## CollectionItemAdded Method
Listens for when a Reactive Collection's item has been added, and ensures that the subscription properly disposed when the system disposes or when the component disposes.
## Parameters
|Name | Description|
|-----|------------|
|system|This system to install this listner on, it will get disposed with this one.|
|select|The ReactiveCollection selector.|
|handler|The method that is invoked when an item is added to the collection|
|immediate|Should the handler be invoked for every item that is currently in the list?|
## CollectionItemRemoved Method
Listens for when a Reactive Collection's item has been removed, and ensures that the subscription properly disposed when the system disposes or when the component disposes.
## Parameters
|Name | Description|
|-----|------------|
|system|This system to install this listner on, it will get disposed with this one.|
|select|The ReactiveCollection selector.|
|handler|The method that is invoked when an item is added to the collection|
##### NOTE: This documentation has been generated, any edits will likely be overriden, to modify this, modify the doc comments in the source code.
