# Services

While services can serve for almost any purpose, they can be used to seperate various features of uFrame, and your application. Examples might include, FacebookService, NetworkingService, AchievementsService, etc.

Services can be accessed directly from any place in the codebase simply by injecting them into a class.

[service injection example]

However, this will create a coupling to specific service. Better way would be to communicate with the service through events. Services should be listening to events, processing them, and publishing its own events that might be useful to other services.

[example of a service subscribe to an event]

[example of a class publishing an event]

[explain the code above]

## Accessing ViewModel

By default uFrame keeps up with viewmodels for us. It maintains a manager for each type of viewmodel you create.

To access these managers, you can inject them into controllers and services using the following code.

```
[Inject] IViewModelManager<PlayerViewModel> AllPlayers { get;set; }
```

Read more about [ViewModel managers](viewmodel-manager.md)

# Default Services

These are the services that are available in uFrame by default.

* [View Service](pages/view-service.md)
* [Scene Management Service](pages/scene-management-service.md)
* [System Service](pages/system-service.md)
