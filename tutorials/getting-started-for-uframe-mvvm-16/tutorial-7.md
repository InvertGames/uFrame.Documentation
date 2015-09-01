![](images/callout_codeheavy.png)
![](images/callout_inprogress.png)


# uFrame MVVM 1.6 Getting Started VII
### Overall Idea

This tutorial illustrates an advanced example of service. It shows how to create basic notifications inside of your game. Tutorial starts by designing the concept from graph perspective, then moves into code implementation details. Finally it shows how to integrate solution with the existing code and how to use it.

### Steps

1. Learn how to create a service, scene type and simple class
2. Learn how to load scene when game is ready
3. Implement notification service
4. Setup NotificationUIScene
5. Perform final test.

### Average Time
10-12 minutes

## Transcript


###### Learn how to create a service, scene type and simple class

In this step we will design the notification feature from the graph standpoint. We will define different nodes
to generate certain classes for us. `NotificationService` will be the main source of logic. `NotifyCommand` will be the main source of data. Being an event, it will also serve as a signal to show our notification. Additionally, notifications require certain UI. We will separate this UI into it's own scene by means of NotificationUIScene node. Such scene will contain all the needed UI objects to display our notification.

Locate and open `MainDiagram` graph.
Right-click somewhere on empty space and select `Add Service`
New service node will appear. Rename to NotificationService.

![](images/img_tut7_00000.png)

Right-click somewhere on empty space and select `Add Scene Type`
New SceneType node will appear. Rename to NotificationUIScene.

![](images/img_tut7_0000.png)

Move inside of NotificationService node by double-clicking the header of this node.
Right-click somewhere on empty space and select `Add SimpleClass`
New SimpleClass node will appear. Rename to NotifyCommand.
Finally add a property to NotifyCommand node. Let it be `Message` of type `string`

![](images/img_tut7_0004.png)

Locate NotificationService node. Add new item in the `Handlers` section. Selection list will appear. Find `MainDiagram : NotifyCommand` and click it

![](images/img_tut7_0005.png)

Now you have to Save and Compile. This will generate all the new classes. After that, you need to update the kernel using corresponding button.

> When kernel is being updated, you may get the following exception:  
> **InvalidOperationException: Operation is not valid due to the current state of the object**  
> This error is harmless and will be fixed in one of upcoming updates

![](images/img_tut7_0006.png)

Finally let's create a scene from NotificationUIScene node. `Assets/Example Project/Scenes/NotificationUIScene.unity` will be generate. We will set this scene up later.

![](images/img_tut7_0007.png)

###### Learn how to load scene when game is ready

Locate and open `Assets/Example Project/MainDiagram/Services/NotificationService.cs`
If you have something generated inside of the class, remove it for now. Make it look like in the snippet.
To keep things shorter, I will omit any `using` statements as well as namespace block. Leave those as they were generated.


```cs
public class NotificationService : NotificationServiceBase {
}
```

Add the following snippet to your `using` statements. This will let us use UniRx specific constructs and unity UI.

```cs
using UniRx;
using UnityEngine.UI; 
```

Our first step is to override Setup method. This method is invoked while kernel is loading.
You set your service up using this method. Most of the time you just subscribe to certain events.

```cs
public class NotificationService : NotificationServiceBase {

    public override void Setup()
    {
       base.Setup();
    }

}
```

So let us subscribe to GameReadyEvent. We will use this event as an entry point to load the UI scene we need for notifications.

```cs
public class NotificationService : NotificationServiceBase {

    public override void Setup()
    {
        base.Setup();
        //Every time GameReadyEvent is published, invoke LoadNotificationUIScene
           this.OnEvent<GameReadyEvent>().Subscribe(evt => LoadNotificationUIScene());
    }

    protected void LoadNotificationUIScene()
    {
    }

}
```

Now let us add the code to actually perform scene loading. Again, it is as simple as publishing an event with a specified SceneName.

```cs
public class NotificationService : NotificationServiceBase {

    public override void Setup()
    {
       base.Setup();
       //Every time GameReadyEvent is published, invoke LoadNotificationUIScene
            this.OnEvent<GameReadyEvent>().Subscribe(evt => LoadNotificationUIScene());
    }

    protected void LoadNotificationUIScene()
    {
       //Will load NotificationUIScene
       this.Publish(new LoadSceneCommand()
       {
           SceneName = "NotificationUIScene"
       });
    }

}
```



###### Implement NotificationService


Now let's add a couple of properties to our service.
UIContainer will serve as a parent for all of notification items.
NotificationItemPrefab will serve as a template for the notification panel.

```cs
public class NotificationService : NotificationServiceBase {


    //UI Container which will hold notification ui items, while they are shown1
    public Transform UIContainer;

    //A template prefab for notification ui item
    public GameObject NotificationItemPrefab;

    public override void Setup()
    {
       base.Setup();
       //Every time GameReadyEvent is published, invoke LoadNotificationUIScene
            this.OnEvent<GameReadyEvent>().Subscribe(evt => LoadNotificationUIScene());
    }

    protected void LoadNotificationUIScene()
    {
       //Will load NotificationUIScene
       this.Publish(new LoadSceneCommand()
       {
           SceneName = "NotificationUIScene"
       });
    }

}
```

The question is: how can I setup the UIContainer, if it lives in the separated scene?
Indeed, both UIContainer and NotificationItemPrefab will let us set them right in the inspector
on the service object. NotificationItemPrefab is a prefab, so we can just drag and drop it from the project window. But UIContainer will live in the separated scene. That is why, we need to think in uFrame style :)

**Q**: What is UIContainer?  
**A**: It is some data (Transform in this case) which comes from a specific scene.  
**Q**: How can we expose Scene-specific data ?  
**A**: Create a scene-specific field using the corresponding SceneType.  
**Q**: How can we set this field?  
**A**: We can set it using Unity Inspector Window on the SceneType object OR through code in the corresponding scene loader.
**Q**: How can we read this field from the service?  
**A**: We can do it using SceneLoaderEvent.  

At this point we have all the information we need. Let us start by exposing Scene-specific data.
Locate and open `Assets/Example Project/MainDiagram/Scenes/NotificationUIScene.cs`.

Add a new transform field called UIContainer.

```cs
public class NotificationUIScene : NotificationUISceneBase
{
    //This will let us assign correct container right in the scene
    public Transform UIContainer;
}
```
We are done. Now we can set it using the Unity inspector. But we will do it later.
For now, let us subscribe to SceneLoaderEvent.
We filter this event by a SceneRoot of a specific type (NotificationUIScene).
Then we invoke `NotificationUISceneLoaded` handler with a SceneRoot cast to the type we need.

```cs
public class NotificationService : NotificationServiceBase {

    //UI Container which will hold notification ui items, while they are shown1
    public Transform UIContainer;

    //A template prefab for notification ui item
    public GameObject NotificationItemPrefab;

    public override void Setup()
    {
        base.Setup();
        //Every time GameReadyEvent is published, invoke LoadNotificationUIScene
           this.OnEvent<GameReadyEvent>().Subscribe(evt => LoadNotificationUIScene());

        //Will invoke NotificationUISceneLoaded when such scene is loaded :)
        this.OnEvent<SceneLoaderEvent>()
            .Where(evt=>evt.SceneRoot is NotificationUIScene)
            .Subscribe(evt => NotificationUISceneLoaded(evt.SceneRoot as NotificationUIScene) );
    }

    protected void NotificationUISceneLoaded(NotificationUIScene scene)
    {
    }

    protected void LoadNotificationUIScene()
    {
        this.Publish(new LoadSceneCommand()
        {
            SceneName = "NotificationUIScene"
        });
    }

}

```

The next step is obvious: read the UIContainer of NotificationUIScene and save it locally in the UIContainer field.

```cs
public class NotificationService : NotificationServiceBase {

    //UI Container which will hold notification ui items, while they are shown1
    public Transform UIContainer;

    //A template prefab for notification ui item
    public GameObject NotificationItemPrefab;

    public override void Setup()
    {
       base.Setup();
       //Every time GameReadyEvent is published, invoke LoadNotificationUIScene
            this.OnEvent<GameReadyEvent>().Subscribe(evt => LoadNotificationUIScene());

       //Will invoke NotificationUISceneLoaded when such scene is loaded :)
       this.OnEvent<SceneLoaderEvent>()
           .Where(evt=>evt.SceneRoot is NotificationUIScene)
           .Subscribe(evt => NotificationUISceneLoaded(evt.SceneRoot as NotificationUIScene) );
    }

    protected void NotificationUISceneLoaded(NotificationUIScene scene)
    {
       UIContainer = scene.UIContainer;
    }

    protected void LoadNotificationUIScene()
    {
       this.Publish(new LoadSceneCommand()
       {
           SceneName = "NotificationUIScene"
       });
    }

}
```

Ok, we now have all the needed information: NotificationItemPrefab and UIContainer. The next step is handling NotifyCommand. We have already added a Handler on NotificationService node. That is why we can override a method called `NotifyCommandHandler`.

```cs
public class NotificationService : NotificationServiceBase {

    //UI Container which will hold notification ui items, while they are shown1
    public Transform UIContainer;

    //A template prefab for notification ui item
    public GameObject NotificationItemPrefab;

    public override void Setup()
    {
        base.Setup();
        //Every time GameReadyEvent is published, invoke LoadNotificationUIScene
           this.OnEvent<GameReadyEvent>().Subscribe(evt => LoadNotificationUIScene());

        //Will invoke NotificationUISceneLoaded when such scene is loaded :)
        this.OnEvent<SceneLoaderEvent>()
            .Where(evt=>evt.SceneRoot is NotificationUIScene)
            .Subscribe(evt => NotificationUISceneLoaded(evt.SceneRoot as NotificationUIScene) );
    }

    protected void NotificationUISceneLoaded(NotificationUIScene scene)
    {
        UIContainer = scene.UIContainer;
    }

    protected void LoadNotificationUIScene()
    {
        this.Publish(new LoadSceneCommand()
        {
            SceneName = "NotificationUIScene"
        });
    }

    public override void NotifyCommandHandler(NotifyCommand data)
    {
        base.NotifyCommandHandler(data);
        //This method exists because we added NotifyCommand in the Handlers section of NotificationService.
        //It will be invoked every time you publish NotifyCommand
    }
}
```

Since we have entry point for displaying the notification (NotifyCommandHandler method), we can implement all the visual details and display our message. This code snippet is heavily commented. Moreover, it is no much uFrame-related, so I will not explain it here.

```cs
public class NotificationService : NotificationServiceBase {

    //UI Container which will hold notification ui items, while they are shown1
    public Transform UIContainer;

    //A template prefab for notification ui item
    public GameObject NotificationItemPrefab;

    public override void Setup()
    {
        base.Setup();
        //Every time GameReadyEvent is published, invoke LoadNotificationUIScene
           this.OnEvent<GameReadyEvent>().Subscribe(evt => LoadNotificationUIScene());

        //Will invoke NotificationUISceneLoaded when such scene is loaded :)
        this.OnEvent<SceneLoaderEvent>()
            .Where(evt=>evt.SceneRoot is NotificationUIScene)
            .Subscribe(evt => NotificationUISceneLoaded(evt.SceneRoot as NotificationUIScene) );
    }

    protected void NotificationUISceneLoaded(NotificationUIScene scene)
    {
        UIContainer = scene.UIContainer;
    }

    protected void LoadNotificationUIScene()
    {
        this.Publish(new LoadSceneCommand()
        {
            SceneName = "NotificationUIScene"
        });
    }

    public override void NotifyCommandHandler(NotifyCommand data)
    {
        base.NotifyCommandHandler(data);
        //This method exists because we added NotifyCommand in the Handlers section of NotificationService.
        //It will be invoked every time you publish NotifyCommand

        StartCoroutine(ShowNotification(data));
    }

    IEnumerator ShowNotification(NotifyCommand notificationData)
    {
        //Construct prefab and cache uiItemCanvas
        var uiItem = Instantiate(NotificationItemPrefab);
        var uiItemCanvas = uiItem.GetComponent<CanvasGroup>();

        //Set alpha to 0
        uiItemCanvas.alpha = 0;

        //Set message to text
        uiItemCanvas.GetComponentInChildren<Text>().text = notificationData.Message;

        //Parent object to the Container
        uiItem.transform.SetParent(UIContainer);

        //Reset scale of an object (unity sometimes messes it up when you parent it to the container with layout group
        uiItem.transform.localScale = Vector3.one;

        //Fade In
        yield return StartCoroutine(Fade(uiItemCanvas, 1, 0.5f));

        //Let it be for 5 seconds
        yield return new WaitForSeconds(5);

        //Fade Out
        yield return StartCoroutine(Fade(uiItemCanvas, 0, 0.5f));

        //Kill the notification ui item
        Destroy(uiItem);
    }

    IEnumerator Fade(CanvasGroup target, float alpha, float time)
    {

        //Start time
        var start = Time.time;
        while (Math.Abs(target.alpha - alpha) > 0.01f)
        {
            //Time passed from start
            var elapsed = Time.time - start;
            //normalized time from 0..1
            var normalizedTime = Mathf.Clamp((elapsed / time) * Time.deltaTime, 0, 1);
            //assign interpolated value
            target.alpha = Mathf.Lerp(target.alpha, alpha, normalizedTime);
            yield return null;
        }

    }

}
```

###### Setup NotificationUIScene

This section describe (mostly) uFrame-unrelated things, like setting up the UI for notifications.
First of all locate and open `Assets/Example Project/Scenes/NotificationUIScene.unity`
By default the scene will look like this:
![](images/img_tut7_0008.png)

Remove all the objects except `_NotificationUISceneRoot`
![](images/img_tut7_0009.png)

Create the following structure of objects.
> Please, remove EventSystem, if Unity generates one. EventSystem will be loaded along with the kernel.

![](images/img_tut7_0010.png)

The next images show object-by-object setup
While setting up `_NotificationUISceneRoot`, do not forget to assign UIContainer object.
![](images/img_tut7_0011.png)

For the Canvas, do not forget to select `Screen Space - Overlay` render mode.
This will make all the notifications to appear on top of any other UI objects.

![](images/img_tut7_0012.png)

While setting up UIContainer, pay attention to the anchors. Do not forget to add Vertical Layout Group component and set it up as shown on the image.

![](images/img_tut7_0013.png)

NotificationItemPrefab requires LayoutElement component and CanvasGroup component. LayoutElement will let us tweak layout settings, while CanvasGroup will allow us to modify transparency.

![](images/img_tut7_0022.png)

Finally setup some text. Pay attention to the anchors.

![](images/img_tut7_0015.png)

As a little test, duplicate your NotificationItemPrefab. All the items should stack on top of each other.

![](images/img_tut7_0016.png)

Create a prefab out of NotificationItemPrefab.

![](images/img_tut7_0017.png)

And clear your scene from any items.
> **Do not forget to save the scene!**

![](images/img_tut7_0018.png)

Now we need to setup NotificationService to use our prefab.
Locate and open `Assets/Example Project/Scenes/ExampleProjectKernelScene`

![](images/img_tut7_0019.png)

Locate Services object and find NotificationService component. Use `Notification Item Prefab` setting to setup item prefab.

![](images/img_tut7_0020.png)

###### Perform final test

Locate and open `Assets/Example Project/MainMenuSystem/Controllers/LevelSelectScreenController.cs`
Locate the following method:
```cs
public override void SelectLevel(LevelSelectScreenViewModel viewModel, LevelDescriptor arg)
{
    base.SelectLevel(viewModel, arg);

    // This can be extracted into a service to load levels from other places
    // without duplicated code. Also external service could handle rules,
    // under which certain level can/cannot be loaded

    if (arg.IsLocked) return; //Level is locked, do not switch scenes

    Publish(new UnloadSceneCommand()
    {
       SceneName = "MainMenuScene" // Unload  main menu scene
    });

    Publish(new LoadSceneCommand()
    {
       SceneName = arg.LevelScene // Load level scene
    });
}
```
Publish NotifyCommand when switching to a Level

```cs
public override void SelectLevel(LevelSelectScreenViewModel viewModel, LevelDescriptor arg)
{
    base.SelectLevel(viewModel, arg);

    // This can be extracted into a service to load levels from other places
    // without duplicated code. Also external service could handle rules,
    // under which certain level can/cannot be loaded

    if (arg.IsLocked) return; //Level is locked, do not switch scenes

    Publish(new UnloadSceneCommand()
    {
        SceneName = "MainMenuScene" // Unload  main menu scene
    });


    Publish(new NotifyCommand()
    {
        Message = "Playing "+arg.Title
    });

    Publish(new LoadSceneCommand()
    {
        SceneName = arg.LevelScene // Load level scene
    });


}
```

Start your game from MainMenuScene and select any level.
You should see a notification pop up in the bottom-right part of the screen.

![](images/img_tut7_0023.png)
