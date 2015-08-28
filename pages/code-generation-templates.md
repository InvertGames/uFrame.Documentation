# Code Generation Templates

 Example project contains an example of Template Extensions. You can also learn from the original MVVM templates.

## Type References

When you are creating type references you can easily just add the node in and the code generator will automatically create the right type for you.

However in some instances where your types come from an external assembly you would need to tell the generator how to access the type.

To do this you need to:

1) Create an editor folder in your uframe project
2) Create a new plugin file, i.e. MyProjectPlugin.cs
3) Make the plugin extend from `DiagramPlugin`
4) Override the `Initialize` method
5) Add the line ` InvertApplication.CachedAssemblies.Add(typeof (MyType).Assembly);`

This will tell the code gen where to find your types when using external assemblies.

[todo add more content about the templates]
