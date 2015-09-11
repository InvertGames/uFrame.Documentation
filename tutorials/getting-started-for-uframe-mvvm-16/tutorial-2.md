# uFrame MVVM 1.6 Getting Started II

### Overall Idea

This tutorial tends to explain the structure of **MainMenuSystem** from the Example Project. It explains what different elements do in this system and how they are combined into a working solution. It also explains how uFrame stores graphs. Finally it shows how to make a simple change: *animation to show and hide different screens*.

### Steps

1. Learn about uFrame Graphs
2. Learn about MainMenuSystem node
3. Learn about MainMenuRoot node
4. Learn about SubScreen node
5. Learn about different screens nodes
6. Learn about MainMenuService node
7. Learn about SubScreenView node
8. Learn how to modify existing binding
9. Perform final test

### Average Time

6-8 minutes

## Transcript

###### Learn about uFrame Graphs
Open `MainMenuSystem` graph:

![](images/img_tut2_0000.png)

Nodes in this graph represent the main menu from the graph perspective. This means that all the nodes are used to generate certain pieces of code which then form the main menu functionality in runtime. While you can go deeper into the details of certain nodes, let us first expand on how uFrame stores graphs and nodes.

A graph is a physical file which lives inside of a Unity project and stores information about all the nodes defined inside of it. Navigate to `Assets/ExampleProject`:

![](images/img_tut2_0001.png)

As you can see you have 5 asset files here.

1. `LevelSystem`
2. `MainDiagram`
3. `MainMenuSystem`
4. `UserManagementSystem`

All of those are graph files and can be opened inside of uFrame Designer.

The last file is called `ExampleProject` (5) and it represents the project entity. Once we click the project file, we can observe settings of the project using the Unity inspector window

![](images/img_tut2_0002.png)

For example we see that this project is connected to the graphs which were described earlier (1).
To sum it up, graph files can live anywhere inside of a Unity project, and we use uFrame project asset (2) to combine those into a uFrame project.

###### Learn about MainMenuSystem

Open `MainMenuSystem` graph and locate MainMenuSystem node.

![](images/img_tut2_0003.png)

MainMenuSystem node is a SubSystem node. From the graph perspective, the **SubSystem** node serves as a container for the following nodes:

1. Element node
2. Service node
3. Enum node
4. SimpleClass node
5. Command node
6. Type Reference node

This means that while being inside of the SubSystem node, you can right-click on the empty space on the graph and you will be able to add the mentioned nodes to the graph:

![](images/img_tut2_0004.png)

The key thing to remember is that the current node container (or filter) is always shown right below the opened tabs. This way you can instantly know that you are inside of some node. In this case it is MainMenuSystem.

![](images/img_tut2_0005.png)

###### Learn about MainMenuRoot

Locate the MainMenuRoot node. This node is an Element node. From the graph perspective, an Element node defines an entity with a bunch of typed properties. In this case it is a Type property, which is called CurrentScreenType. It holds the type of the screen to be shown at the moment. An Element node allows us to define collections. In this case it is the Screens collection, which holds SubScreenViewModels. Finally, an Element node allows you to define operations which can be performed on this entity. Such operations are called Commands. MainMenuRoot has no Commands defined.

![](images/img_tut2_0006.png)

From the MVVM perspective, an Element node defines a ViewModel with its data. It also defines Controller (which is not part of the original MVVM). In uFrame MVVM, every ViewModel can have Commands. Each Command defines an operation or an event which needs to be handled. This is where the Controller comes into play: it holds code to handle Commands. We will see how this works in the next part.

Finally, an Element node serves as a container for the following nodes:

1. Command node
2. Computed Property node
3. Enum node
4. SimpleClass node
5. StateMachine node
6. Type Reference node
7. View node
8. ViewComponent node

To get inside of an Element node, we need to double-click its header:

![](images/img_tut2_0007.png)

When you double-click on the header, ensure that your current filter is MainMenuRoot (1).
Just like with the SubSystem node, you can right-click on the empty space of the graph to see all the nodes which can be added inside of this container (or filter) (2)

![](images/img_tut2_0008.png)

Finally double-click the header of MainMenuRoot again (1) to get back out of the MainMenuRoot element. Alternatively you can click `MainMenuSystem` near the 'current filter' indicator (2).

![](images/img_tut2_0009.png)

This should bring you back to the MainMenuSystem.

###### Learn about SubScreenNode

Locate SubScreenNode (1) and expand it(2).

![](images/img_tut2_0011.png)

This node is an Element node. From the graph perspective, this node defines a screen entity which will be part of the main menu. Regardless of the purpose of the screen, it will always hold a property indicating if this screen is active or not. This property is called IsActive and has type bool. It will also contain an operation which closes the screen. Such an operation is defined using Close command (2).

###### Learn about different Screens

There are severals elements which inherit from SubScreen element:

![](images/img_tut2_0010.png)

1. **LoginScreen**  
Serves to hold the input of the user when he tries to log into the system. This input is represented using Username and Password properties, which are both of type string. It also exposes a Command called Login.
2. **SettingsScreen**  
Serves to provide configuration for the application. It holds properties called *Resolution* and *Volume*. The `ResolutionInformation` type will be discussed in the next parts. It also holds a collection of all the supported resolutions. Finally it exposes 2 commands: *Apply* and *Default*.
3. **LevelSelectScreen**  
Allows user to select a level to play. This element holds a collection of all the available levels. It also holds a command called SelectLevel. Notice that the Command type is set to LevelDescriptor. This means that every time this command is triggered, it passes an instance of LevelDescriptor along with it.
4. **MenuScreen**  
This element has no Properties, Collections, or Commands. As for now it is just an empty object. However, it is still useful as it represents the screen which allows us to select other screens.

Notice the connection between the SubScreen Element node and all the described nodes:

This means inheritance: all the described screens will contain the IsActive property defined on the SubScreen Element node. They will also expose the Close Command.

###### Learn about MainMenuService node

Locate MainMenuService node.

![](images/img_tut2_0012.png)

From the graph perspective, a Service node defines a set of functionality which is not strictly bound to any specific ViewModel. From the MVVM perspective, a Service can operate on any MVVM entity: View, ViewModel, Data or even Controller. Usually a Service listens for certain events and publishes its own, and performes valuable work in between. Services will be covered in the next parts.

###### Learn about SubScreenView node

A View is an essential part of MVVM. Locate SubScreenNode and go inside of it. You should be able to locate SubScreenView node (1):

![](images/img_tut2_0014.png)

This node defines a visual representation of the data defined on SubScreenNode element. We express this fact by plugging SubScreen element node (A) into SubScreenView node (B).

From the graph perspective, a View node serves to define a living visual representation of the ViewModel data in the physical world. Here, the "physical world" is Unity and a "living object" is a MonoBehaviour.

###### Learn how to modify existing binding

In the bindings section of SubScreenView node, locate the "IsActive Changed" binding (1):

![](images/img_tut2_0015.png)

Ctrl+Click on the SubScreenView node. Alernatively, you can right-click (1) on the header of SubScreenView node and select Open (2) -> MainMenuSystem/Views/SubScreenView.cs (3).

![](images/img_tut2_0016.png)

This should open your IDE with the SubScreenView class:

```cs
namespace uFrame.ExampleProject
{
    /*
     * This view serves as a base class for all the SubScreen views
     * It handles screen activation/deactivation.
     * It also handles binding for Close command. You can configure it using the inspector.
     */
    public class SubScreenView : SubScreenViewBase
    {
        public GameObject ScreenUIContainer;

        protected override void InitializeViewModel(uFrame.MVVM.ViewModel model)
        {
            base.InitializeViewModel(model);
        }

        public override void Bind()
        {
            base.Bind();
        }

        public override void IsActiveChanged(Boolean active)
        {
            /*
             * Always make sure, that you cache components used in the binding handlers BEFORE you actually bind.
             * This is important, since when Binding to the viewmodel, Handlers are invoked immidiately
             * with the current values, to ensure that view state is consistant.
             * For example, you can do this in Awake or in KernelLoading/KernelLoaded.
             * However, in this example we simply use public property to get a reference to ScreenUIContainer.
             * So we do not have to cache anything.
             */
            ScreenUIContainer.gameObject.SetActive(active);
        }

    }
}
```

This class has already implemented the IsActiveChanged method. This method is generated because the SubScreenView node has an `IsActive Changed` binding. This method is invoked every time the IsActive Property is changed and a new value is passed as a parameter. We are going to change the implementation to animate transparency of the screen based on the value of IsActive Property.

There are several ways to achieve this. We will use the Canvas Group component and its alpha property. First of all let us add a new Property to the class called `ScreenUIContainerCanvasGroup` of type CanvasGroup:

```cs
namespace uFrame.ExampleProject
{
    /*
     * This view serves as a base class for all the SubScreen views
     * It handles screen activation/deactivation.
     * It also handles binding for Close command. You can configure it using the inspector.
     */

    public class SubScreenView : SubScreenViewBase
    {

        public GameObject ScreenUIContainer;
        public CanvasGroup ScreenUIContainerCanvasGroup; //New property added

        ...rest of the class...

    }
}
```  

Next we need to introduce code to ensure that the CanvasGroup exists and cache it. We will use the Awake method to achieve this:

```cs
namespace uFrame.ExampleProject
{
    /*
     * This view serves as a base class for all the SubScreen views
     * It handles screen activation/deactivation.
     * It also handles binding for Close command. You can configure it using the inspector.
     */

    public class SubScreenView : SubScreenViewBase
    {

        public GameObject ScreenUIContainer;
        public CanvasGroup ScreenUIContainerCanvasGroup;

        public void Awake()
        {
          if (ScreenUIContainer != null && ScreenUIContainerCanvasGroup == null)
              ScreenUIContainerCanvasGroup = ScreenUIContainer.GetComponent<CanvasGroup>(); //Try to grab canvas group
          if(ScreenUIContainerCanvasGroup == null)
              ScreenUIContainerCanvasGroup = ScreenUIContainer.AddComponent<CanvasGroup>(); //If it does not exist, create one.

          //Coalesc operator (??) does not work here due to weird unity behaviour

        }

        ... rest of the class ...

    }
}

```

Next we need to add a function to fade CanvasGroup to a certain target alpha value:

```cs

namespace uFrame.ExampleProject
{
    /*
     * This view serves as a base class for all the SubScreen views
     * It handles screen activation/deactivation.
     * It also handles binding for Close command. You can configure it using the inspector.
     */

    public class SubScreenView : SubScreenViewBase
    {

        public GameObject ScreenUIContainer;
        public CanvasGroup ScreenUIContainerCanvasGroup;

        public void Awake()
        {
          if (ScreenUIContainer != null && ScreenUIContainerCanvasGroup == null)
              ScreenUIContainerCanvasGroup = ScreenUIContainer.GetComponent<CanvasGroup>(); //Try to grab canvas group
          if(ScreenUIContainerCanvasGroup == null)
              ScreenUIContainerCanvasGroup = ScreenUIContainer.AddComponent<CanvasGroup>(); //If it does not exist, create one.

          //Coalesc operator (??) does not work here due to weird unity behaviour
        }

        ...rest of the class...

        /**
         * target - target CanvasGroup to fade
         * alpha - target alpha to fade to
         * time - time take to fade to target
         * onComplete (optional) - invoked after fading is complete
         * delay (optional) - delay before start fading.
         */
        void FadeAlpha(CanvasGroup target, float alpha, float time, Action onComplete = null, float delay = 0.0f)
        {
            //Stop any fading before starting new one
            StopCoroutine("Fade");
            //Invoke coroutine
            StartCoroutine(Fade(target, alpha, time, onComplete,delay));
        }

        IEnumerator Fade(CanvasGroup target, float alpha, float time, Action onComplete = null, float delay = 0.0f)
        {

            target.interactable = false;

            //Wait for delay
            if(delay > 0.0f) yield return new WaitForSeconds(delay);
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
            //invoke on complete if given
            if (onComplete != null) onComplete();

            target.interactable = true;
        }

    }
}

```

Finally we need to change the IsActiveChanged binding handler to use the new functionality. After all the modifications your class should look like this:


```cs

namespace uFrame.ExampleProject
{
    /*
     * This view serves as a base class for all the SubScreen views
     * It handles screen activation/deactivation.
     * It also handles binding for Close command. You can configure it using the inspector.
     */

    public class SubScreenView : SubScreenViewBase
    {

        public GameObject ScreenUIContainer;
        public CanvasGroup ScreenUIContainerCanvasGroup;

        public void Awake()
        {
          if (ScreenUIContainer != null && ScreenUIContainerCanvasGroup == null)
            ScreenUIContainerCanvasGroup = ScreenUIContainer.GetComponent<CanvasGroup>(); //Try to grab canvas group
        if(ScreenUIContainerCanvasGroup == null)
            ScreenUIContainerCanvasGroup = ScreenUIContainer.AddComponent<CanvasGroup>(); //If it does not exist, create one.

        //Coalesc operator (??) does not work here due to weird unity behaviour

        }

        protected override void InitializeViewModel(uFrame.MVVM.ViewModel model)
        {
            base.InitializeViewModel(model);
        }

        public override void Bind()
        {
            base.Bind();
        }

        public override void IsActiveChanged(Boolean active)
        {
            /*
             * Always make sure, that you cache components used in the binding handlers BEFORE you actually bind.
             * This is important, since when Binding to the viewmodel, Handlers are invoked immidiately
             * with the current values, to ensure that view state is consistant.
             * For example, you can do this in Awake or in KernelLoading/KernelLoaded.
             * However, in this example we simply use public property to get a reference to ScreenUIContainer.
             * So we do not have to cache anything.
             */

            var targetAlpha = active ? 1f : 0f; //fade to 1 if screen is active, else fade to 0
            var time = 0.2f; //how fast to fade
            var delay = active ? time : 0; //delay if screen is active, to let other screen deactivate

            if (active)
            {
                //activate container game object
                ScreenUIContainer.gameObject.SetActive(true);
                //set alpha to zero
                ScreenUIContainerCanvasGroup.alpha = 0;
                //start fading
                FadeAlpha(ScreenUIContainerCanvasGroup, targetAlpha, time, null, delay);
            }
            else
            {
                //fade out
                FadeAlpha(ScreenUIContainerCanvasGroup, targetAlpha, time, () =>
                {
                  //on complete, maintain consistant active status of the gameObject:
                  //This prevents glitches, if IsActive property changes too fast
                    ScreenUIContainer.gameObject.SetActive(SubScreen.IsActive);
                }, delay);
            }

        }

        /**
         * target - target CanvasGroup to fade
         * alpha - target alpha to fade to
         * time - time take to fade to target
         * onComplete (optional) - invoked after fading is complete
         * delay (optional) - delay before start fading.
         */
        void FadeAlpha(CanvasGroup target, float alpha, float time, Action onComplete = null, float delay = 0.0f)
        {
            //Stop any fading before starting new one
            StopCoroutine("Fade");
            //Invoke coroutine
            StartCoroutine(Fade(target, alpha, time, onComplete,delay));
        }

        IEnumerator Fade(CanvasGroup target, float alpha, float time, Action onComplete = null, float delay = 0.0f)
        {
            //While transitioning, make canvas non-interactable
            target.interactable = false;

            //Wait for delay
            if(delay > 0.0f) yield return new WaitForSeconds(delay);
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
            //invoke on complete if given
            if (onComplete != null) onComplete();

            target.interactable = true;
        }

    }
}

```

We have succesfully modified the IsActiveChanged binding to animate the screens.

###### Perform final test

Start MainMenuScene, type in the login and password (uframe/uframe) and hit Login. You should see how the login screen slowly fades out and the menu screen fades in. This should happen every time you transition from one screen to another.
