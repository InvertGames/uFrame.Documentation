# Code Generation Templates

Codegen templates allow you to create custom code based upon nodes in the graph, you could target element nodes and create a custom class per ViewModel or even make one per ComputedProperty in a ViewModel. It is extremely powerful but also quite difficult to get started with as there is not really any public knowledge on the subject other than what is being documented here.

Also the example project contains an example of Template Extensions. You can also learn from the original MVVM templates.

## Creating the plugin

There are 2 parts to using codegen templates, to start with you need to make a custom class which implements `IDiagramPlugin` within an `Editor` folder in your Unity project.

Your class should look something like this:

```csharp
public class SomeDiagramPlugin : DiagramPlugin
{
    public override decimal LoadPriority
    {
        get { return 10; }
    }

    public override void Initialize(UFrameContainer container)
    {
        // Registration of Templates Goes Here
    }
}
```

Now the `LoadPriority` indicates what order the plugin should load in, so this way you can load multiple custom plugins and even if they depend upon each other they can be assigned an order in the processing queue.

The `Initialize` method is the bit we really care about though, as that is where we will register our templates against node types, so for example if we were to have:

```csharp
public override void Initialize(UFrameContainer container)
{
    RegisteredTemplateGeneratorsFactory.RegisterTemplate<PropertiesChildItem, SetPropertyActionsTemplate>();
}
```

We would be telling it that we wish to run the `SetPropertyActionsTemplate` against all Properties within nodes, this by default would then run the `SetPropertyActionsTemplate` for each and every property within your graphs, be it on `SimpleClass` objects or `Elements`. You can specify nodes directy too such as:

```csharp
RegisteredTemplateGeneratorsFactory.RegisterTemplate<ComputedPropertyNode, DoSomethingWithComputedPropertiesTemplate>();
```

So once you have created your plugin class above and registered any of your templates you can then start using them by going to the `uFrame > Settings` options within the Unity menu. You should see `SomeDiagramPlugin` in the list with a tick box next to it.


## Creating a template

Once you have your plugin ready to host your templates you will need to make an implementation of `IClassTemplate<T>` making sure that your generic type matches the Node/Item type used when registering, so for example if I had registered a computed node template like shown in the plugin section:

```csharp
RegisteredTemplateGeneratorsFactory.RegisterTemplate<ComputedPropertyNode, DoSomethingWithComputedPropertiesTemplate>();
```

Then I would need to create an implementation of `IClassTemplate<ComputedPropertyNode>` which would look something like this:

```csharp
[TemplateClass(TemplateLocation.DesignerFile, ClassNameFormat = "{0}Output")]
public class SomeComputedPropertyTemplate : IClassTemplate<ComputedPropertyNode>
{
    public TemplateContext<ComputedPropertyNode> Ctx { get; set; }

    public string OutputPath
    {
        get { return Path2.Combine(Ctx.Data.Graph.Name, "YourDirectoryName"); }
    }

    public bool CanGenerate
    {
        get { return true; }
    }

    public void TemplateSetup()
    {
        // Do your setup
    }
}
```

So there is a bit going on here, first of all lets talk about the `TemplateClass` attribute.

### TemplateClassAttribute

This allows you to tell uFrame if you want to create a designer file, a code file or both. So for example the default classes created from templates use `Both` to create both a designer file which is kept in sync with the diagrams and then a code file which is customized by the user. You can specify here if you want a single file or both designer or the code file generated. The other argument is the naming format I want to give the class which I am generating. The `{0}` will automatically be replaced with the name of the thing you are templating, so for example if I had a computed property called `IsAlive` then used `ClassNameFormat = "CheckingIf{0}"` then the outputted class would be `CheckingIfIsAlive`. There are premade class name formats to pick from or you can just pass it a string yourself.

### Ctx property

This is injected for you and is basically your code generator object. The Ctx contains all manner of methods for outputting raw code, assigning attributes, overrides and anything else that you can normally do when writing code.

Given that this is a really complex thing we will talk about this far more in the next section, but for now just know that this needs to match the generic type of the class.

### OutputPath method

This is basically telling it where you want your files to be put, if you are only using designer files I think it is ignored, however if you were creating your own code files for the user to maintain outside of the designer files you would want to make this the destination for the templated outputs.

### CanGenerate method

This is a very important one, as it lets you constrain when your templates are run for a given type. So for example lets say we are wanting to template ComputedProperties as we have been mentioning above, however we only want our template to run when the Computed Properties type is a `Boolean` in this bit you could do:

```csharp
public bool CanGenerate
{
    get { return Ctx.Data.RelatedTypeName == "Boolean"; }
}
```

You can put any predicate in here so you can check the node type, the graph, the connections etc so it is a very powerful concept if you only want to use the templates in certain scenarios.

### TemplateSetup method

I will be honest and I am not sure if you NEED the method to be called this or any public method will be run, however for the moment lets assume this is the entry point into our class. We will want to setup how the template should generate code.

This gets quite complicated from here on as we need to discuss the `TemplateContext` object in some more details.

## Template Context

So the `TemplateContext` is the heart of the template system and allows you to create all manner of code in different ways, there is far more to it than I will explain here but the information below should at least provide some sort of basis for you to develop basic templates with and look more in depth for yourself.

So lets go over some common scenarios and how you would do it using the class template and template context objects.

### Generate a method

So one of the most common things in classes are methods, so you can generate a method in a couple of ways. The most expressive one is to make a method in your class template and then pass it attributes to explain to it how to generate.

```csharp
[GenerateMethod]
protected void DoSomething()
{
    Ctx._("Debug.Log(\"Hello\")");
}
```

So this would output:

```csharp
protected void DoSomething()
{
    Debug.Log("Hello");
}
```

This is an overly simple example, but you can augment the method by either adding more attributes either like the `GenerateMethod` approach with an explicit attribute, or by telling the `Ctx` object to augment the current declaration. Here is an example of both ways.

```csharp
[GenerateMethod, AsOverride]
protected void OverrideSomeMethod()
{ }

// would be the same as

[GenerateMethod]
protected void OverrideSomeMethod()
{
    Ctx.CurrentProperty.Attributes |= MemberAttributes.Override;
}
```

Either of the above examples would output:

```csharp
protected override OverrideSomeMethod()
{}
```

As shown you can add any body you want to the method using the `Ctx._` method which allows you to put in a formatted string which is converted into code. There are other methods and helpers which we will cover in the next few sections but this should at least give you a starting point for creating methods.

### Creating properties

As shown before you have multiple to make a property, and some ways to customize the generation, ie do you want backing fields generated for you, or do you want to specify the getter/setter your self.

Here is an example:

```csharp
[GenerateProperty, WithField]
public string SomeProperty { get; set; }
```

the above would output

```csharp
private string _someProperty;

public string SomeProperty
{
    get { return _someProperty; }
    set { _someProperty = value; }
}
```

You can manually put your own code inside the getters like so:

```csharp
[GenerateProperty]
public string SomeProperty
{
    get
    {
        Ctx._("return 10");
    }
}
```

would output

```csharp
public string SomeProperty
{
    get { return 10; }
}
```

So you can write your own, or you can just let the attributes generate them for you.

### Creating Members/Fields

There are some quirks with member generation, you would probably at first try to do something like:

```csharp
[GenerateMember]
public string MyField;
```

however this would not generate anything, as the field generation has not entirely been completed yet from what I was told. However fear not there is a way around this, and this also shows you another way that you can generate class content without needing to use any attributes. So to begin we need to go back to our `TemplateSetup` method, and within there we can tell the `TemplateContext` to generate fields for us on the current class.

```csharp
public void TemplateSetup()
{
    Ctx.CurrentDeclaration._private_("string", "_data");
}
```

which would output the following on the class:

```csharp
private string _data;
```

You can use `_public_` as well if you want a public field, although if you want to have a protected field you need to jump through a hoop and manually add some meta data to the generation, which would look like so:

```csharp
public void TemplateSetup()
{
    var createdField = Ctx.CurrentDeclaration._private_("string", "_data");
    createdField.Attributes = MemberAttributes.Family;
}
```

The `MemberAttributes.Family` basically tells the object to be protected, you can also use this above method on other kinds of objects and assign things like `MemberAttributes.Override`.

### Augmenting the container class

So now we know how to create content within the class lets just go over the ways we can augment the class itself, and this touched on the last topic discussed.

So the `TemplateContext` object has the notion of `CurrentDeclaration`, it also has `CurrentProperty`, `CurrentMember`, `CurrentMethod` etc, so you can use all these exposed properties to define how you want the context to operate, so if we wanted to augment the class we would add in our `TemplateSetup`:

```csharp
public void TemplateSetup()
{
    Ctx.CurrentDeclaration.IsPartial = true;
    Ctx.AddAttribute(typeof(RequireComponent), "typeof(ViewBase)"); // We have omitted the CurrentDeclaration here but you can use it
}
```

The above would tell the code generator to add `[RequireComponent(typeof(ViewBase))]` to the containing class and make it partial, you can add as many attributes as you want or change the member attributes as shown elsewhere to make your class function as you expect.

You can also add namespace imports to class like so:

```csharp
public void TemplateSetup()
{
    Ctx.TryAddNamespace("uFrame.MVVM");
}
```

This would add `using uFrame.MVVM` to the top of the class file, so you can import whatever dependencies you require for this class.

### Some other things to know

So we have covered most of the main stuff here, however there are some other helpers it is worth knowing about or some other ways to get information if you are stuck. So to begin with there are some helpers for typing things.

You have access to `_ITEMTYPE_` which basically exposes the current itemtype as a class but when the codegen processes it, the type is dynamically replaced. Here is an example:

```csharp
public class SomePropertyTemplate : IClassTemplate<PropertiesChildItem>
{
    // assume mandatory stuff

    [GenerateProperty, WithField]
    public _ITEMTYPE_ MyProperty {get;set;}
}
```

Now if we assume that we have a property of type `Boolean` then the outputted code would be:

```csharp
public class SomePropertyTemplate : IClassTemplate<PropertiesChildItem>
{
    // assume mandatory stuff

    private Boolean _myProperty;
    public Boolean MyProperty
    {
        get { return _myProperty; }
        set { _myProperty = value; }
    }
}
```

So this can be used as a handy way to specify generics or just the current type of your object. You can also access this information from the `TemplateContext` by using `Ctx.Data` which contains a reference to the current information applicable to this node. You can get the actual node by doing `Ctx.Data.Node` this would return the `IDiagramNode` that you can cast or use as is to find connections, input or containing types.

For example if you were doing a computed property template but needed to get the containing view models type, you could do `Ctx.Data.Node.Container().Name.AsViewModel()` which would basically get the current node, get the containing context, retreive the name from that, then there are a few extension methods for strings which expose `AsViewModel`, `AsController` and many others.

## Summary

So hopefully this has given you enough information to get started and see how you can create your classes code through explicit declarations with attributes or manual context invocations.

There is also a lot more you can do with codegen such as making your own nodes and customizing existing nodes, however that is for another article.

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
