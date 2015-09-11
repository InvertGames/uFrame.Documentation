# uFrame MVVM 1.6 Getting Started IV

### Overall Idea

This tutorial tends to explain what is scene type. It explains the purpose of this node as well as the responsibility of generated classes. Finally it shows how to make a simple change: how to add new level to the game.

### Steps

1. Scene Type node explained in details
2. Scene Loader and Scene Loaders explained
3. Learn how to create scene using Scene Type
4. Learn how to setup Level Scene
5. LevelManagementService explained
6. Learn how register new Level scene in LevelManagementService
7. Perform final test.

### Average Time

8-10 minutes

## Transcript

###### Scene Type node explained in details

Open MainDiagram graph and locate LevelScene node:

![](images/img_tut4_0000.png)

LevelScene scene is a SceneType node. SceneType nodes define different types of the scenes you may have in your game. You may have single main menu scene and multiple level scenes, and they all deserve their own scene type: LevelScene type for levels and MainMenuScene type for main menu.

###### Scene Loader, Scene Settings, and Scene Loaders explained  

Right-click on the header of LevelScene node and select `Open` -> `MainDiagram/Scenes/LevelScene.cs`

![](images/img_tut4_0001.png)

This should open your IDE with LevelScene class.

```cs
namespace uFrame.ExampleProject
{

    /*
     * This is a script which defines SCENE TYPE. In this case it is LevelScene.
     * You define scene type for the scene by attaching such script to the root of the scene.
     * You can add any propeties specific for the Scene instance to this class
     * You can modify LevelSceneLoader class to change how such scene loads.
     */
    public class LevelScene : LevelSceneBase
    {
        /*
         * An ID to be used to get meta information for this level
         */
        public int Id;


        private LevelRootViewModel _levelRoot;

        /*
         * When requested, finds LevelRootView in the hierarchy, extracts the viewmodel and caches it
         */

        public LevelRootViewModel LevelRoot
        {
            get { return _levelRoot ?? (_levelRoot = GetComponentInChildren<LevelRootViewBase>().LevelRoot); }
            private set { _levelRoot = value; }
        }
    }
}
```

This class represents scene type. It may have different scene related properties. This script is a MonoBehavior. That is why you can have settings exposed in the Unity inspector window.

Once we use this script in a specific scene, for example, we can assign Id using `Id` property. Based on this this Id, we will be able to retrieve additional information about this level, which is not stored inside of this scene.

Right-click on the header of LevelScene node and select `Open` -> `MainDiagram/Scenes/LevelSceneLoader.cs`

![](images/img_tut4_0002.png)

This should open your IDE with LevelSceneLoader class.

```cs
namespace uFrame.ExampleProject
{
    public class LevelSceneLoader : LevelSceneLoaderBase
    {


        /*
         *  Example of Scene Loader which sets up different ViewModel instances of the game in this scene
         *  Here we assume that every LevelScene has a LevelRoot somewhere around (scene.LevelRoot in this case).
         *  We want to make sure, that once scene is loaded, LevelRoot knows what the current level is.
         *  
         *  We do it specifically in the LevelSceneLoader, because this should happen when we load LevelScene.
         *  With such a setup, it does not matter if we start from MainMenuScene, LevelScene or IntroScene.
         */

        [Inject] public LevelManagementService LevelManagementService;

        protected override IEnumerator LoadScene(LevelScene scene, Action<float, string> progressDelegate)
        {
            /*
             * Old Chinese trick: When loading scene, some of the components in this scene may not be initialized by unity
             * In this situation, object.GetComponent<>() will not work on some game objects.
             * That is why we can just skip one frame and let unity finally do it's job.
             */
            yield return null;

            /*
             * When loading scene, make sure LevelRoot knows about what scene it is currently in.
             * Find LevelDescriptor with the corresponding ID and assign it to the CurrentLevel property of the LevelRoot
             */
            scene.LevelRoot.CurrentLevel = LevelManagementService.Levels.FirstOrDefault(level => level.Id == scene.Id);
            yield break;
        }

        protected override IEnumerator UnloadScene(LevelScene scene, Action<float, string> progressDelegate)
        {
            yield break;
        }
    }
}
```

This class represents loader for level scene. uFrame generates loader for each scene type. Loader allows you to define any logic to load a specific scene. It exposes 2 methods: Load and Unload.

Each method gets passed instance of scene type and a delegate, which you can use to report progress during the loading procedure.

In general, scene loader may be used for absolutely any purpose: from generating worlds to downloading additional content needed for the scene.

###### Learn how to create scene using Scene Type

Open MainDiagram graph and locate LevelScene node:

![](images/img_tut4_0000.png)

Right-click on the header of LevelScene node and select `Create Scene`:

![](images/img_tut4_0003.png)

Save scene dialog will open. Please save your scene inside `Assets/Example Project/Scenes` folder as `Level4.scene`:

![](images/img_tut4_0004.png)

> By default, uFrame is trying to eliminate this step, by automatically creating a scene called `{SceneType}Scene`. If such scene does not exist, uFrame will create it automatically. So for LevelScene type, it may try to create LevelScene in `Assets/Example Project/Scenes`. In this case, Save scene dialog will not open. If that happened, please locate LevelScene and change it's name to Level 4  

Newly created scene will open an the hierarchy will look like this:

![](images/img_tut4_0006.png)

Notice that `_LevelSceneRoot` object will contain LevelScene script. So, scene type scripts are used as roots for your scenes. **It is extremely important to keep all your scene objects under such root object**. If you leave any object outside, this object will stay in the game, when you unload the scene. Let us change the hierarchy and put `Main Camera` under as `_LevelSceneRoot` child. Also let us remove Directional Light:

![](images/img_tut4_0007.png)


###### Learn how to setup Level Scene

Level scene structure is quite primitive. You need to place LevelRootView somewhere under scene root. The structure then may look like this:

![](images/img_tut41_0000.png)

In the inspector of LevelRootView, you will find a setting for `FinishCurrentLevel` binding:

![](images/img_tut41_0001.png)

It requires you to assign a button. When you press this button, Game will transition to main menu. Create a canvas, then create a button. Style it the way you want. Finally drag and drop the button to the `FinishCurrentLevel` binding setting. If Unity creates EventSystem object, remove it. This one will be automatically loaded when you start the game.

![](images/img_tut41_0002.png)

> Remember to keep canvas as a child of `_LevelSceneRoot`

###### LevelManagementService explained

Before we make final step and register newly created level in the game, let us explore how LevelManagementService works. Open `LevelSystem` graph and locate `LevelManagementService` node.

![](images/img_tut41_0003.png)

Open LevelManagementService.cs.

```cs
namespace uFrame.ExampleProject
{
    /*
     * This service introduces example of interesting database.
     * Check the kernel how LevelDescriptors live on a certain GameObject and this service reads them.
     */

    public class LevelManagementService : LevelManagementServiceBase
    {
        private List<LevelDescriptor> _levels;

        //Game object holding LevelDescriptor components
        public GameObject LevelsContainer;

        // This list will hold all the registered levels
        // You can add level descriptors dynamically by adding new LevelDescriptor
        // component on the service object and calling UpdateLevels
        public List<LevelDescriptor> Levels
        {
            get { return _levels ?? (_levels = new List<LevelDescriptor>()); }
            set { _levels = value; }
        }

        public override void Setup()
        {
            base.Setup();
            //On setup register levels initially
            UpdateLevels();
        }

        private void UpdateLevels()
        {
            var levelDescriptorComponents = LevelsContainer.GetComponents<LevelDescriptor>().Except(Levels);
                //Get all non registered level descriptors
            Levels.AddRange(levelDescriptorComponents); //Add those to the list of registered levels
        }
    }
}
```

This service manages all the registered Levels. Levels are represented using LevelDescriptor component. Such components are placed on a special gameobject. LevelManagementService then reads all the LevelDescriptors and registers each descriptor in list.

###### Learn how register new Level scene in LevelManagementService

It is time for us to find out where do all the services live.
Open `Assets/ExampleProject/Scenes/ExampleProjectKernelScene.unity`:

![](images/img_tut41_0004.png)

> Do not forget to save previous scene!

This scene is very special and it is called kernel scene. When you start your game in any scene, this scene will be loaded additively. Kernel scene contains all the dependencies for your game: services, system loaders and scene loaders. It may also contain any other dependency you specify, like `EventSystem` for Unity GUI.

For example, `Services` object contains all the services mentioned in your project:

![](images/img_tut41_0005.png)

Notice `Levels` object in the hierarchy:

![](images/img_tut41_0006.png)

This one contains all the LevelDescriptors. Let us add new descriptor for the level we have created in the previous steps:

![](images/img_tut41_0007.png)

Do not forget to click apply, when you are finished with new level descriptor.
Save the scene and open `Assets/Example Project/Scenes/Level4.unity` scene.
Locate `_LevelSceneRoot` object and select it. Find Id property and enter the same Id you selected for the corresponding level descriptor:

![](images/img_tut41_0012.png)

Finally open build settings and check that `Assets/Example Project/Scenes/Level4.unity` is in the scenes list:

![](images/img_tut41_0010.png)

###### Perform final test

Start your game in main menu and navigate to level selection screen. You should be able to locate your level in the list. If you click it, level scene will open. Once you click "Back To Menu" button, main menu will open again.

![](images/img_tut41_0011.png)
