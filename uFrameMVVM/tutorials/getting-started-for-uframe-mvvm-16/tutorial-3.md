# uFrame MVVM 1.6 Getting Started III

### Overall Idea

This tutorial tends to explain how settings screen works. It explains direct way of communication between service and controller. Finally it shows how to make a simple change: how to reset settings to default state.

### Steps

1. uFrame event system explained
2. Services explained
3. Learn how to introduce properties on the Service
4. Learn how to inject Service inside of controller
5. Learn how to initialize ViewModel using data from a Service
6. Learn how to use Service data to handle ViewModel commands
7. Perform final test

### Average Time

6-8 minutes

## Transcript

###### uFrame event system explained.

Open MenuScreenView.cs and locate lines 38-41:

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

First line creates an instance of an object. This object is not special in any sense. In fact, uFrame event system does not care about the event object at all. That is why any object can become an event. uFrame event system differentiates between different events based on the type of the object you use.

Second line adds additional information to the event.

Finally third line publishes the event. At this point, event becomes available to the whole world, and any part of the system can handle it.


Now you know how to publish events.
Publish method is available in Services, Controllers and Views.

Open `MainMenuSystem\Services.designer.cs` and locate the following line:

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

###### Services explained

Open MainDiagram graph and locate SettingsService node:

![](images/img_tut3_0000.png)

From graph perspective, Service node describes a piece of functionality, which is not bound to any specific ViewModel. SettingsService exposes functions and properties to setup game settings, like sound volume and resolution.

Services are not strictly bound to MVVM pattern. Any controller, view or viewmodel may get a reference to this service using dependency inject.

On components level, there are two common use cases for the service.
The first way is event-based. In this scenario service publishes and handles different events and you do not need direct reference to the service to communicate with it.
The second way involves direct reference and direct API calls.

###### Learn how to introduce properties on the Service

Right-click on the header of SettingsService node and select `Open` -> `MainDiagram/Services/SettingsService.cs`:

![](images/img_tut3_0001.png)

Alternatively you can ctrl+click on the SettingsService node. This should open your IDE with SettingsService class. Our goal is to introduce properties with default values for Resolution and Volume.  

```cs

namespace uFrame.ExampleProject
{
    public class SettingsService : SettingsServiceBase
    {

        public float DefaultVolume
        {
            get { return 0.6f; }
        }

        public ResolutionInformation DefaultResolution
        {
            get { return GetAvailableResolutions().First(); }
        }

        ...Rest of the class...

    }
}

```

Default volume is chosen to be 0.6f, and default resolution is the any first item from available resolutions list.

Now let's modify Volume and Resolution properties, to make use of default values:


```cs

namespace uFrame.ExampleProject
{
    public class SettingsService : SettingsServiceBase
    {

        ...Rest of the class...

        //Simple property which works of PlayerPrefs
         public float Volume
         {
             get { return PlayerPrefs.GetFloat("Settings_Volume", DefaultVolume); } //Make use of DefaultVolume
             set { PlayerPrefs.SetFloat("Settings_Volume", value); }
         }

         //Advanced version of it, which works of PlayerPrefs, but uses string to
         //store serialized version of the object
         //Please notice that by default, Simple Class does not have Serialize/Deserialize methods
         //This is generated due to the extension of SimpleClassTemplate.
         public ResolutionInformation Resolution
         {
             get
             {
                 if (PlayerPrefs.HasKey("Settings_Resolution"))
                 {
                     var resInfo = new ResolutionInformation();
                     //Deserialize into the new instance
                     resInfo.Deserialize(PlayerPrefs.GetString("Settings_Resolution"));
                     return resInfo;
                 }
                 else
                 {
                     //return default value, if player never changed settings
                     return DefaultResolution; //make use of DefaultResolution
                 }
             }
             set
             {
                 //Serialize
                 PlayerPrefs.SetString("Settings_Resolution", value.Serialize());
             }
         }

         ...Rest of the class...

    }
}

```

###### Learn how to inject Service inside of controller

Open MainMenuSystem graph and locate SettingsScreen element node:

![](images/img_tut3_0002.png)

Right-click on the header of SettingsScreen node and select Open -> MainMenuSystem/Controllers/SettingsScreenController.cs

![](images/img_tut3_0003.png)

Alternatively, you can ctrl+click on the header of SettingsScreen node.
This should open your IDE with SettingsScreenController class.

Locate line 23:

```cs

[Inject] public SettingsService SettingsService;

```

This line shows how to get a reference to SettingsService using dependecy injection.
'[Inject]' attribute tells uFrame that this is a property/field which needs to be automatically injected. In other words, it tells uFrame to automatically find the value for the property/field. In this case we are interested in SettingsService instance. Please notice, that property/field has to be public in order to be injected.

You can use dependency injection in services and controllers. To use dependency injection in the View, you will need to toggle on "Inject View" setting on the view.

###### Learn how to initialize ViewModel using data from a Service

Locate line 25 with a method called `InitializeSettingsScreen`:

```cs

      public override void InitializeSettingsScreen(SettingsScreenViewModel viewModel)
       {
           base.InitializeSettingsScreen(viewModel);

           /* Add known resolutions to the list */
           viewModel.AvailableResolutions.AddRange(SettingsService.AvailableResolutions);

           /* Setup current resolution */
           viewModel.Resolution = SettingsService.Resolution;

           /* Setup volume */
           viewModel.Volume = SettingsService.Volume;
       }

```

Since every controller is bound to work with a specific view model, every controller has a method like this:

```cs
Initialize{ElementName} ({ElementName}ViewModel viewModel) { ... }
```
This method is invoked every time you create a ViewModel of the corresponding type.

Notice how we use SettingsService to initialize the SettingsScreenViewModel:

```cs
/* Setup current resolution */
viewModel.Resolution = SettingsService.Resolution;

/* Setup volume */
viewModel.Volume = SettingsService.Volume;
```

This allows settings screen to show correct information.

###### Learn how to use Service data to handle ViewModel commands

Locate line 55 with a method called `Default`:

```cs
public override void Default(SettingsScreenViewModel viewModel)
{
    base.Default(viewModel);
}
```

This method serves a handler for Default command defined on SettingsScreenViewModel
Let us modify it, to actually set game settings to default values:

```cs
public override void Default(SettingsScreenViewModel viewModel)
{
    base.Default(viewModel);

    viewModel.Volume = SettingsService.Volume = SettingsService.DefaultVolume;
    viewModel.Resolution= SettingsService.Resolution = SettingsService.DefaultResolution;
}
```

###### Perform final test

Run MainMenuSceen, log in, and open settings screen. Play around with the values and click `Default` button. Your settings should be set to default values now.
