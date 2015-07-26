# uFrame MVVM 1.6 Getting Started III

### Overall Idea

This tutorial tends to explain how settings screen works. It explains direct way of communication between service and controller. Finally it shows how to make a simple change: how to reset settings to default state.

### Steps

1. uFrame event system exaplained
2. Service node explained
3. Service generated code explained
4. Learn how to introduce properties on the Service
5. Learn how to inject Service inside of controller
6. Learn how to initialize ViewModel using data from a Service
7. Learn how to use Service data to handle ViewModel commands
8. Perform final test

### Average Time

6-8 minutes

## Transcript

###### uFrame event system explained.

Open MainMenuService.cs and locate lines 38-41:

```cs

Publish(new RequestMainMenuScreenCommand()
{
    ScreenType = typeof (LevelSelectScreenViewModel)
});

```

Let's rewrite it in a different form:

```cs

var evt= new RequestMainMenuScreenCommand();
evt.ScreenType = typeof(LevelSelectScreenViewModel);
Publish(evt);

```

First line creates an instance of an object. This object is not special in any sense. In fact, uFrame event system does not care about the event object at all.

Second line adds additional information to the event.

Finally third line publishes the event. At this point, event becomes available to the whole world, and any part of the system can handle it.


Now you know how to publish events.
Publish method is available in Services, Controllers and Views.

Open MainMenuServiceBase.cs and locate line 32:

```cs

this.OnEvent<RequestMainMenuScreenCommand>().Subscribe(this.RequestMainMenuScreenCommandHandler);

```

Let's rewrite it in a different form:

```cs

this.OnEvent<RequestMainMenuScreenCommand>().Subscribe(evt => {
    this.RequestMainMenuScreenCommandHandler(evt);
});

```

This line is a classic example of Reactive Extensions:
OnEvent<T> returns an observable, which you can either modify, or directly subscribe to.

.Subscribe(Handler) will call Handler every time when event of type T is published, and will pass an instance of this type.

Now you should know how to subscribe to events.

###### Service node explained
