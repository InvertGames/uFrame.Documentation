# uFrame MVVM 1.6 Getting Started V


### Overall Idea

This tutorial attempts to explain registered instances. It explains the purpose of registered ViewModels and shows how to bind a View to a registered instance. Finally it shows how to make a simple change: how to show local user information inside of MainMenu.

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

From the graph perspective, a subsystem node serves mainly to incorporate elements which are semantically related to each other.
Suppose the user may create a CarSystem which holds elements to describe the car itself and all the related parts, like wheels, engines and others.

![](images/img_tut5_0001.png)

From the code perspective a SubSystem is represented by a SystemLoader. The purpose of a SystemLoader is to load different parts of your architecture which are not Monobehaviors. Unity makes it extremely easy to setup classes which are MonoBehaviors: you just drag and drop them into your scene. But sometimes MonoBehavior is not desired and you need to setup class instances which have nothing to do with Unity in general. For example, Controllers and ViewModels are not MonoBehaviors. To effectively load them into your environment, uFrame generates SystemLoaders.

A base SystemLoader is dead simple: it just registers non-Monobehavior instances in the Container.
Here is an example of a CarSystemLoaderBase class:

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


###### Learn how to register an instance of a viewmodel inside of a SubSystem node

Sometimes you may need a globally-registered ViewModel.

> Such a ViewModel is often referred to as 'Named' instance, 'Shared' instance or 'Globally Registered' instance.

Such an instance will have a unique name when registered in the container. That is how any part of your application can get
access to it by means of dependency injection.

Locate and open the UserManagementSystem graph. Locate the UserManagementSystem graph node and expand it.

![](images/img_tut5_0000.png)

Locate the `Instances` section. This section containes one item called `LocalUser` of type UserViewModel. You can use the plus button to add more items to this section. Remember: there should be no items with both the same name and same type. If you want to introduce another registered instance of type `User`, you have to use a unique name, for example, `RemoteUser`.

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

This line demonstrates how to inject named instances.

```cs
[Inject("LocalUser")] public UserViewModel LocalUser;
```

uFrame will automatically set the values of this field for you when setting up the service.

> The same approach will work in Controllers, Services, SceneLoaders and SystemLoaders. uFrame also exposes an option to Inject Views. This may come in handy when you have services related to the presentation side of your game, like `SoundService`.

After you inject the instance you can do whatever you want with it. For example, UserManagementService uses the named instance of UserViewModel to store up-to-date authorization information for the local user.


###### Learn how to modify UserManagementService

Open the UserManagementSystem graph and locate the User element node. Expand it.

![](images/img_tut5_0002.png)

Add a new property to the User element node. Name it `Username` and make sure the type is string.

![](images/img_tut5_0003.png)

Save and compile.  
Locate and open `Assets/ExampleProject/UserManagementSystem/Services/UserManagementService.cs`

Let us change the authorization logic a little bit to save the given username into the LocalUser ViewModel if the given information is correct.

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

Now any time we succesfully log in the system will remember the user's username.

###### Learn how to create Views

Our ultimate goal is to show user information inside of the main menu. The correct approach would be defining  new View for the UserViewModel. However, we will go even further and make it correct from the architecture standpoint.

When it comes to good design, you want different pieces of your game to be reusable. So if we start a new project with a different game, we want to reuse the same UserManagementSystem with all the Services and ViewModels. This will save us a lot of time.

But on the other hand, we want to define a View for the UserViewModel which is specific for this particular game: we want to show it in the main menu. uFrame allows you to keep Views and elements in different graphs.

Naviage to `MainMenuSystem`, right-click on the empty space on the graph and select
`Show Item` -> `UserManagementSystem` -> `User`.

![](images/img_tut5_0004.png)

A User element node will appear. However, it will have a different look and feel because it comes from the other graph.

![](images/img_tut5_0005.png)

In such a state, you are not able to modify the User element node. However, you can still go inside of it by double-clicking its header. Despite being inside of User element node, we are still working within the MainMenuSystem graph. This means that we cannot accidentally break the UserManagementSystem or User element by any means.

Right-click on empty space and select `Add View`


![](images/img_tut5_0006.png)


Rename the View node to MainMenuUserView.
Now we need to plug the output connector of the User element node into the input connector of the MainMenuUserView node called `Element`

![](images/img_tut5_0007.png)

We have succesfully created a new view for the User element ouside of UserManagementService.

Now we need to add a binding to MainMenuUserView called `Username To Text`.

![](images/img_tut5_0008.png)

It is time for us to save and compile.

###### Learn how to place the View in the scene

Open MainMenuScene. Locate the `_MainMenuSceneRoot` GameObject and add an empty child.
Call the newly created GameObject `_MainMenuUserView`. Add a script to this GameObject called MainMenuUserView.

![](images/img_tut5_0009.png)

Select `_MainMenuUserView` object and, in the Unity inspector window, locate the editor for the MainMenuUserView component. Locate the Bindings section. Locate the Username binding and the `Input` property for a Text object to bind to.

![](images/img_tut5_0010.png)

At this point you should design how your visual representation will look like.
When you are done, link the Username binding property with the text object.

![](images/img_tut5_0011.png)

###### Learn how to bind a View to a registered ViewModel instance

Select the `_MainMenuUserView` GameObject and in the Unity inspector window, locate the editor for the MainMenuUserView component. Find the property called `ViewModel Identifier`.

![](images/img_tut5_0012.png)

You can either press the button on top of the field called "Use Registered 'LocalUser' Instance" or you can manually type in `LocalUser` into the field  

![](images/img_tut5_0013.png)

Now this view will bind to the registered ViewModel and display its information. This is the same instance we use inside of UserManagementService to store authorization information.

###### Perform final test

Start the game from MainMenuScene and type in the correct credentials (uframe/uframe). Click `Sign In`. Your text object should show the correct username.

![](images/img_tut5_0014.png)

If you have TextMeshPro then this is how to amend uFrame to enable bindings for it such that they can be used in this tutorial.

In UGUIBindings.cs add the following:
```cs
public static IDisposable BindTextMeshProUGUIToProperty(this ViewBase viewBase, TextMeshProUGUI input, P<string> property)
		{
			if (input != null)
			{
				input.text = property.Value ?? string.Empty;
			}
			
			var d1 = property.Subscribe(value =>
			                            {
				if (input != null) input.text = value;
			});
			
			return d1.DisposeWith(viewBase);
		}


public static IDisposable BindTextMeshProUGUIToProperty<T>(this ViewBase viewBase, TextMeshProUGUI input, P<T> property,
		                                                Func<T, string> selector)
		{
			
			var d1 = property.Subscribe(value =>
			                            {
				input.text = selector(value);
			});
			
			input.text = selector(property.Value);
			
			return d1.DisposeWith(viewBase);
		}
```

In uFrameTemplates.cs add the following:
```cs
	container.AddBindingMethod(typeof (UGUIExtensions), "BindTextMeshProUGUIToProperty",
			                   _ => _ is PropertiesChildItem && _.RelatedTypeName == typeof (string).Name)
			        	    .SetDescription("Binds a string property to a TextMeshProUGUI text label.")
					        .SetNameFormat("{0} To TextMeshProUGUI");
```

That should be it.

This should enable you to enact the binding in the uFrame Designer because it will appear as an option when you click to add a binding.  Once compiled it will allow you to drag a TextMeshProUGUI object from the hierarchy to the field in the Inspector on the view that is bound to the variable you set up in the Designer.
