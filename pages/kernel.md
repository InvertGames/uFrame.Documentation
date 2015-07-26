# Kernel

The uFrame Kernel is an essential piece of uFrame, it handles loading scenes, systems and services. The kernel is nothing more than a prefab with these types of components attached to it in an organized manner.

[add kernel image]

In the image above you can see the scene 'BasicsProjectKernelScene'. This scene will always always contain the 'BasicsProjectKernel' prefab and any other things that need to live throughout the entire lifecycle of your application.

Important Note: All SystemLoaders, Services, and SceneLoaders are MonoBehaviours attached their corresponding child game-objects in the kernel prefab.

 Whenever a scene begins, uFrame will ensure that the kernel is loaded, if it hasen't been loaded it will delay its loading mechanism until the kernel has been loaded. This is necessary because you might initialize ViewModels, deserialize them, etc. so the view should not bind until that data is ready.

For convenience uFrame 1.6 makes the process of creating the kernel very easy and straightforward. By pressing the Scaffold/Update Kernel button it will create a scene, and a prefab with all of the types created by the uFrame designer. You can freely modify the kernel, and updating it will only add anything that is not there.

[add button picture]

![](images/kernel_boot_order.png)

[add description to the boot order image]

Once the kernal has loaded, it will publish the event 'GameReadyEvent'. This event ensures that everything is loaded and bindings have properly taken place on views. 
