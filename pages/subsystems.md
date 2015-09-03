# Subsystems
![](http://i.imgur.com/eeu6Xzl.jpg)
Subsystems allow you to separate various pieces of your project into logical and reusable parts. Subsystems contain any number of Services, Elements, Views, StateMachines, etc.

For each subsystem in the designer, uFrame generates a [System Loader](system-loaders.md) class used to setup uFrame Runtime with any dependencies it might need before the game begins.

System loaders register an instance of every element controller that lives inside of it, as well as any instances defined on it.

## System Loader base class

System loader base classes are defined in the `SystemLoaders.designer` file in the subsystem folder. They contain references to all ViewModels defined in it as well as their controllers.

You can override the default `Load()` method in the `{Subsystem}SystemLoader` class to register other instances to the dependency injection container.

## Custom System Loaders

In some cases creating a custom system loader can be very useful for different environments. e.g. Dev Environment, Production Environment, etc.

To create a custom system loader, derive from SystemLoader, override the load method, and add it to the kernel.

## File structure

Data of each subsystem is held in a folder with the same name as the name of the subsystem.

![](http://i.imgur.com/xpZypFN.jpg)

## Designer

You can create subsystem in two ways:

1. From the _MainGraph_ (the graph that contains the _MVVM Kernel Graph_ node).
2. With the _Create Subsystem Graph_ button (at the top toolbar).

The only difference between Subsystems created with those methods is that the Subsystems created with the _Create Subsystem Graph_ button become exporable ie. you can export them to a separate file.

Also, Subsystem cannot contain another Subsystems.
