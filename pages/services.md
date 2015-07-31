# Services

While services can serve for almost any purpose, they can be used to seperate various features of uFrame, and your application. Examples might include, FacebookService, NetworkingService, AchievementsService, etc.

Services are initialized by the [uFrame Kernel](uframe-kernel.md) and therefore must be first added to the _Services_ game object in the kernel prefab.

## Using a Service

Services can be accessed directly from any place in the codebase simply by injecting them into a class.

[service injection example]

However, this will create a coupling to specific service. Better way would be to communicate with the service through events. Services should be listening to events, processing them, and publishing its own events that might be useful to other services.

To subscribe to an event you can use `OnEvent<{EventClassName}>()` and subscribe to it.

```
// Inside Service's Setup() method.
this.OnEvent<{EventClassName}>().Subscribe(evt => {});
```

[example of a class publishing an event]

[explain the code above]

## Service setup

You can setup a service by overriding the `Setup()` method. This method is invoked when the kernel is loading. Since the kernel lives throughout the entire lifecycle of the game, this will only be invoked once. You can use the `Setup()` method to subscribe to events and execute custom actions when those events are fired.

```
public override void Setup() {
    base.Setup();

    // Use the line below to subscribe to events.
    // this.OnEvent<MyEvent>().Subscribe(myEventInstance=>{ Debug.Log("MyEvent was fired.") });
}
```

## Service Async Setup

You can also setup your services by overriding the `SetupAsync()` method. This method is called by the kernel to do any setup that may take some time to complete. It is executed as a co-routine by the kernel. It's invoked once upon kernel initialization.

Method signature:

```
public override IEnumerator SetupAsync()
```

[ **REQ** Code example ]


## Accessing ViewModel

By default uFrame keeps up with viewmodels for us. It maintains a manager for each type of viewmodel you create.

To access these managers, you can inject them into controllers and services using the following code.

```
[Inject] IViewModelManager<PlayerViewModel> AllPlayers { get;set; }
```

Read more about [ViewModel managers](viewmodelmanager.md)

## Accessing Controllers

You can access any controller by injecting it:

```csharp
[inject] PlayerController PlayerController;
```

If your controller property gets value of null, check if all [System Loaders](system-loaders.md) are attached to the [kernel](uframe-kernel.md).

## Default Services

These are the services that are available in uFrame by default.

* [View Service](pages/view-service.md)
* [Scene Management Service](pages/scenemanagementservice.md)
* [System Service](pages/system-service.md)
