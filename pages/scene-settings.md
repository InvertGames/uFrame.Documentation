# Scene Settings

In the `{SceneName}SceneSettings` class you can specify properties that can be passed along to the scene when it's loaded.

```csharp
// Example settings for the UIScene scene type.
public class UISceneSettings : UISceneSettingsBase {
    public int Width { get; set; }
    public int Height { get; set; }
}
```

You can specify those settings when loading a scene with a `LoadSceneCommand()`.

```csharp
Publish(new LoadSceneCommand() {
    SceneName = sceneName,
    Settings = new UISceneSettings() {
        Width = 640,
        Height = 480
    }
});
```

When the scene loads, you can make use of those settings from the `LoadScene()` method of the scene's loader class.

## Creating your own settings class

You can create you own settings class by inheriting from `SceneSettings<T>`.

```csharp
public class MyCustomSceneSettings : SceneSettings<MyCustomScene> {

    // Value to be passed to the loaded scene.
    public int MyCustomValue;
}
```
