![](images/callout_codeheavy.png)
![](images/callout_inprogress.png)


# uFrame MVVM 1.6 Getting Started VII
### Overall Idea

This tutorial illustrates an advanced example of service. It shows how to create basic notifications inside of your game. Tutorial starts by designing the concept from graph perspective, then moves into code implementation details. Finally it shows how to integrate solution with the existing code and how to use it.

### Steps

1. Learn how to create a service, scene type and simple class
2. Learn how to load scene when kernel is loaded
3. Implement notification service
4. Prepare NotificationUIScene
5. Perform final test.

### Average Time
10-12 minutes

## Transcript


###### Learn how to create a service, scene type and simple class

![](images/img_tut7_00000.png)

![](images/img_tut7_0000.png)

<<MENTION STACK ERROR IS HARMLESS>>

![](images/img_tut7_0004.png)

![](images/img_tut7_0005.png)

![](images/img_tut7_0006.png)

![](images/img_tut7_0007.png)

###### Learn how to load scene when kernel is loaded

```cs
public class NotificationService : NotificationServiceBase {
}
```

```cs
using UniRx;
```

```cs
public class NotificationService : NotificationServiceBase {

    public override void Setup()
    {
       base.Setup();
    }

}
```

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

```cs
public class NotificationService : NotificationServiceBase {


    //UI Container which will hold notification ui items, while they are shown1
    public TransformUIContainer Container;

    //A template prefab for notification ui item
    public NotificationItemPrefab;

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


```cs
public class NotificationUIScene : NotificationUISceneBase
{
    //This will let us assign correct container right in the scene
    public Transform UIContainer;
}
```


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

        //Set text to message
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
            var normalisedTime = Mathf.Clamp((elapsed / time) * Time.deltaTime, 0, 1);
            //assign interpolated value
            target.alpha = Mathf.Lerp(target.alpha, alpha, normalisedTime);
            yield return null;
        }

    }

}
```

###### Setup NotificationUIScene
![](images/img_tut7_0008.png)

![](images/img_tut7_0009.png)

![](images/img_tut7_0010.png)
<<Mention to remove EventSystem>>


![](images/img_tut7_0011.png)


![](images/img_tut7_0012.png)

![](images/img_tut7_0013.png)

![](images/img_tut7_0022.png)

![](images/img_tut7_0015.png)

![](images/img_tut7_0016.png)

![](images/img_tut7_0017.png)

![](images/img_tut7_0018.png)

<<SAVE SCENE>>

![](images/img_tut7_0019.png)

![](images/img_tut7_0020.png)

![](images/img_tut7_0021.png)
