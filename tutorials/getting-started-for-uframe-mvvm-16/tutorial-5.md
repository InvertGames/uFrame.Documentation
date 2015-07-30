# uFrame MVVM 1.6 Getting Started V


### Overall Idea

This tutorial tends to explain registered instances. It explains the purpose of registered viewmodels and shows how to bind certain view to a registered instance. Finally it shows how to make a simple change: how to show local user information inside of MainMenu.

### Steps

1. SubSystem node explained in details
2. Learn how to register instance of a ViewModel inside of SubSystem node
3. Learn how to inject registered ViewModel instance
4. Learn how to modify UserManagementService
5. Learn how to create Views
6. Learn how to place View in the scene
7. Learn how to bind View to a registered viewmodel instance.
8. Perform final test.

### Average Time
8-10 minutes

##Transcript

###### SubSystem node explained in details

From the graph perspective, subsystem node serves mainly to incorporate elements which are semantically related to each other.
User may want to create a CarSystem which holds elements to describe the car itself and all the related parts, like wheels, engines and others.

![](images/img_tut5_0001.png)

From the code perspective SubSystem is represented with so called SystemLoader. The purspose of SystemLoader is to load different parts of your architecture, which are not MonoBehaviors. Unity makes it extremely easy to setup classes which are MonoBehaviors, you just drag and drop them into your scene. But sometimes MonoBehavior is not the case and you need to setup instances, which have nothing to do with Unity in general. For example, Controllers and ViewModels are not MonoBehaviors. To effectively load them into your environment, uFrame generates SystemLoaders.

It is deadly simple: it just registers non-monobehavior instances in the container.
Here is an example of CarSystemLoaderBase class:

```cs

namespace uFrame.ExampleProject {

    public class CarSystemLoaderBase : uFrame.Kernel.SystemLoader {

        private CarController _CarController;

        private EngineController _EngineController;

        private WheelController _WheelController;

        [uFrame.IOC.InjectAttribute()]
        public virtual CarController CarController [...]

        [uFrame.IOC.InjectAttribute()]
        public virtual EngineController EngineController [...]

        [uFrame.IOC.InjectAttribute()]
        public virtual WheelController WheelController [...]

        public override void Load()
        {

            //Register Controllers and Managers in the container.
            Container.RegisterViewModelManager<CarViewModel>(new ViewModelManager<CarViewModel>());
            Container.RegisterController<CarController>(CarController);
            Container.RegisterViewModelManager<EngineViewModel>(new ViewModelManager<EngineViewModel>());
            Container.RegisterController<EngineController>(EngineController);
            Container.RegisterViewModelManager<WheelViewModel>(new ViewModelManager<WheelViewModel>());
            Container.RegisterController<WheelController>(WheelController);
        }
    }
}


```


###### Learn how to register instance of a viewmodel inside of SubSystem node

Sometimes, you will need, so called, globally registered ViewModel.

> Such ViewModel is often refered to as 'Named' instance, 'Shared' instance or 'Globally Registered' instance.

Such instance will have a unique name, when registered in the container, and that is why, any part of your application can get
access to it, by means of dependency injection.

Locate and open UserManagementSystem graph. Locate UserManagementSystem graph node and expand it.

![](images/img_tut5_0000.png)

Locate `Instances` section. This section containes one item called `LocalUser` and has type of UserViewModel. You can use plus button to introduce more items in this section. Remember: there should be no items with both same name and same type. If you want to introduce another registered instance of type `User`, you have to pickup the unique name, for example, `RemoteUser`.

###### Learn how to inject registered ViewModel instance

Locate and open `Assets/ExampleProject/UserManagementSystem/Services/UserManagementService.cs`


```cs

namespace uFrame.ExampleProject
{
    public class UserManagementService : UserManagementServiceBase
    {

        [Inject("LocalUser")] public UserViewModel LocalUser;

        public override void Setup()
        {
            base.Setup();
            LocalUser.AuthorizationState = AuthorizationState.Unauthorized;
        }

        public void AuthorizeLocalUser(string Username, string Password)
        {
            if (Username == "uframe" && Password == "uframe")
            {
                Debug.Log("authorized in service");
                LocalUser.AuthorizationState = AuthorizationState.Authorized;
            }
        }


    }
}

```

This line shows how to inject named instances.

```cs
[Inject("LocalUser")] public UserViewModel LocalUser;
```

uFrame will automatically set the values of this field for you, when setting up the service.

> The same approach will work in Controllers, Services, SceneLoaders and SystemLoaders. uFrame also exposes an option to Inject views. This may come in handy when you have services related to the visual side of your game, like `SoundService`.

After you injected the instance, you can do whatever you want with it. For example, UserManagementService uses it to store up-to-date authorization information.


###### Learn how to modify UserManagementService

Open UserManagementSystem graph and locate User element node. Expand it.

![](images/img_tut5_0002.png)

Add a new propertz to User element node. Name it `Username`, make sure the type is string.

![](images/img_tut5_0003.png)

Save and compile.  
Locate and open `Assets/ExampleProject/UserManagementSystem/Services/UserManagementService.cs`

Let us change authorization logic a little bit to save the given username into the LocalUser ViewModel, if given information is correct.

```cs

namespace uFrame.ExampleProject
{
    public class UserManagementService : UserManagementServiceBase
    {

        [Inject("LocalUser")] public UserViewModel LocalUser;

        public override void Setup()
        {
            base.Setup();
            LocalUser.AuthorizationState = AuthorizationState.Unauthorized;
        }

        public void AuthorizeLocalUser(string Username, string Password)
        {
            if (Username == "uframe" && Password == "uframe")
            {
                Debug.Log("authorized in service");
                LocalUser.Username = Username; //This is how we save the username
                LocalUser.AuthorizationState = AuthorizationState.Authorized;
            }
        }

    }
}

```

At this point, any time we succesfully log in, system will remember the information about the credentials.

###### Learn how to create Views

Our ultimate goal is to show user information inside of main menu. The correct approach would be defining new view for the UserViewModel. However, we will go even further and make it correct from the architecture standpoint.

When it comes to good design, you want different pieces of your game to be reusable. So if we start a new project with a different game, we want to reuse the same UserManagementSystem with all the services and view models. This will save us a lot of time.

But on the other, we want to define a View for UserViewModel which is specific for this particular game: we want to show it in the main menu. uFrame allows you to keep views and elements in different graphs.

Naviage to `MainMenuSystem`, right-click on the empty space on the graph and select
`Show Item` -> `UserManagementSystem` -> `User`.

![](images/img_tut5_0004.png)

A User element node will appear. However, it will have a different look and feel, because it comes from the other graph.

![](images/img_tut5_0005.png)

In such a state, you are not able to modify User element node. However, you can still go inside of it, by double-clicking it's header. Being inside of User element node, we are still working with MainMenuSystem graph. This means that you cannot break UserManagementSystem or User element by any means.

Right-click on empty space and select `Add View`


![](images/img_tut5_0006.png)


Rename view node to MainMenuUserView.
Now we need to plug output connector of User element node into input connector of MainMenuUserView node called `Element`

![](images/img_tut5_0007.png)

We have succesfully created new view for the User element ouside of UserManagementService.

Now we need to add a binding to MainMenuUserView called `Username To Text`.

![](images/img_tut5_0008.png)

It is time for us to save and compile.

###### Learn how to place View in the scene

Open MainMenuScene. Locate `_MainMenuSceneRoot` object and add an empty child to this one.
Call newly created object: `_MainMenuUserView`. Add a script to this object called MainMenuUserView.

![](images/img_tut5_0009.png)

Select `_MainMenuUserView` object and in the Unity inspector window, locate the editor for MainMenuUserView component. Locate Bindings section. Locate settings for Username. There will be a settings for Text object to bind to.

![](images/img_tut5_0010.png)

At this point you should design how your visual representation will look like.
When you are done, link Username binding input setting with the text object.

![](images/img_tut5_0011.png)

###### Learn how to bind View to a registered viewmodel instance

Select `_MainMenuUserView` object and in the Unity inspector window, locate the editor for MainMenuUserView component. Find setting called `ViewModel Identifier`.

![](images/img_tut5_0012.png)

You can either press the button on top of the field called "Use Registered 'LocalUser' Instance" or you can manually type in `LocalUser` into the field  

![](images/img_tut5_0013.png)

Now this view will bind to the registered ViewModel and display it's information. This is the same instance we use inside of UserManagementService to store authorization information.

###### Perform final test

Start the game from MainMenuScene and type in correct credentials. Click login. Your text object should show correct information.

![](images/img_tut5_0014.png)
