# uFrame MVVM 1.6 Getting Started I

## Overview

This tutorial shows how to install uFrame MVVM, shows the default package and proposes a simple change in the default project: creating an error message for login screen

### Steps

1. Learn how to install uFrame MVVM
2. Learn how to setup example project
3. Learn how to add property to the element
4. Learn how to add binding to the view
5. Learn how to change logic in controllers
6. Learn how to connect view with GUI
7. Perform final test

### Average Time

6-8 minutes

## Transcript

###### Introduction:
Hello and welcome to uFrame 1.6 getting started guide.
The entire guide is based on uFrame MVVM 1.6 Example Project.
Every part explains certain features of uFrame MVVM. Almost every part then proposes a simple change in the example project.

This part will show how to install uFrame MVVM and how to setup example project. It also proposes a simple change in the example project: creating an error message for the login screen.

###### How to install uFrame MVVM
Using asset store, you can install uFrame MVVM just like any other asset: simply import it and follow Unity instructions.
![How to import uFrame MVVM](images/assetstore_import.png)

As for uFrame MVVM 1.6, you will find folder called uFrameMVVM inside of your assets folder. It contains 2 other packages with different uFrame MVVM versions.

![2 Package inside of uFrameMVVM folder](images/uframe_importedstate.png)


Version 1.5 is shipped for backwards compatibility. If you are new to uFrame MVVM, please stick with version 1.6. Import the version you want by double clicking on the corresponding package. You can safely remove uFrameMVVM folder after this step.

This tutorial shows basics of uFrame MVVM 1.6. Tutorials for version 1.5 are available on [Invert Games Studios Website](http://invertgamestudios.com/mvvm/overview).

###### Learn how to setup default project

Once you import uFrameMVVM 1.6, Welcome Screen will show up. To manually open Welcome Screen, select uFrame -> Welcome Screen in the menu panel (1).

![uFrame MVVM Welcome Screen](images/uframe_welcome_screen_open.png)

In the welcome screen, navigate to Examples (1) and click “SETUP EXAMPLE PROJECT” button (2).

![uFrame MVVM Setup Example Project Button](images/uframe_welcome_screen_goto.png)

Follow the instructions. Example Package will be deployed into your Assets Folder (1), all the needed scene will be added to your build settings (2). Finally intro scene will be opened and run (3).
![Example Project deployed state](images/example_project_deployed.png)

###### Learn how to add property to the element
Open uFrame Designer Window using menu panel Windows -> uFrame Designer.

![How to open graph designer](images/uframe_open_graph_designer_instruction.png)

First of all you need to open MainMenuSystem graph. Check opened graph tabs:

![Opened graph tabs](images/uframe_graph_tabs.png)

If it contains MainMenuSystem, simply click it.
If MainMenuSystem tab is not there, click Graph selection control and select MainMenuSystem:

![How to open graph](images/open_graph_example.png)

MainMenuSystem graph should open (1), and you should be able to locate MainMenuSystem graph tab (2).


![Main menu graph opened](images/main_menu_graph_opened.png)

Just like with any other graph, MainMenuSystem graph contains nodes. Nodes can represent different information. uFrame MVVM nodes contain special sub-header (or tag) which shows what type of node it is (1). In this step we are interested in Element node which is called LoginScreen:

![LoginScreen element](images/img_0000.png)

Element node defines entities of your games in terms of data they hold. For example LoginScreen contains information about Username and Password, which user enters to login into the system. Such data is revealed when you expand the node. You can expand or collapse a node by clicking arrow control in the bottom of the node (1):

![Expand/Collapse control shown](images\img_tut1_0000.png)

Expand LoginScreen. Let us add new property. For this we need to click + button near the Properties section (1):

![Plus button near properties shown](images/img_0002.png)

New node child item will be created. in the Properties section. Such child item will contain type in the left column and name in the right column (1):

![Property child item shown](images/img_0003.png)

Type (1) represents type of the property and by default it is set to string. Name (2) represents name of the property and by default it is set to “Properties”. We now need to change the name of the property. When you add a new property, field is automatically editable and you can type in the name. Let us name it ErrorMessage. Finish editing by clicking outside of the node. If, at some point, you want to rename existing property later, you need to double click on the current name, to make field editable again. After this step, your node should look like this:

![Final state of LoginScreen element](images/img_0004.png)



This property will be of type string and we do not have to change. However, if you want to change the type of the property, you can click the current type and select desired type from the window which pops up.

![Type selection window](images/img_0006.png)

We have successfully added new property to the LoginScreen element.

###### Learn how to add binding to the view
Being on the MainMenuSystem graph, let us double click header of LoginScreen element node.

![Double click header of Login Screen](images/img_0007.png)

This will let us get deeper into the details of the node. As a matter of fact, MVVM stands for Model View ViewModel. While Element node defines Model and ViewModel, View is expressed differently. In this example, View is represented using a View Node called LoginScreenView.

![Graph filtered by LoginScreen](images/img_0009.png )

> It is important to note, that element can have unlimited number of views.

We plug LoginScreen element node (A) into LoginScreenView element input (B), to express that LoginScreenView represents LoginScreen data. We can also see that LoginScreenView inherits from SubScreenView (C). You can read more about inheritance in the documentation.

In this step we want to create a binding of ErrorMessageProperty to the Text GUI object.
For that, we need to click plus button (1) near the Bindings section on the LoginScreenView Node.

![](images/img_0010.png)

A window will popup, with a list of all possible bindings. We need to select “ErrorMessage To Text” (1):

![](images/img_0011.png)

Once we select it, it should appear in the list of existing bindings (1):

![](images/img_0012.png)

We have successfully added binding to the LoginScreenView node.

###### Learn how to change logic in controllers and services

This step involves a little bit of programming. Since we have done some changes to the nodes, we need to Save and Compile our diagram using corresponding button (1) in the top right corner.

![image](images/img_0013.png)

Saving and Compiling process turns diagram items and nodes into CSharp code.

Once the process is complete, we can start modifying the code.
In the previous step we have introduces UI binding for the ErrorMessage property of LoginScreen. Now we want to specify how we set this property.

Right click on the LoginScreen element node header. Select Open -> LoginScreenController.cs

![](images/img_0014.png)

This will open your IDE with the corresponding generated file and let you modify the internals of it. Alternatively, you can manually locate LoginScreenController.cs file in your unity project

We are interested in method called Login. It gets invoked when we try to log in. It delegates login procedure to the UserManagementService along with the Username and Password.

```cs
public override void Login(LoginScreenViewModel viewModel)
{
    base.Login(viewModel);
    /* Direct call to the service. */
    UserManagementService.AuthorizeLocalUser(viewModel.Username, viewModel.Password);
}
```

Let's modify this method to look like this:

```cs
public override void Login(LoginScreenViewModel viewModel)
{
    base.Login(viewModel);
    /* Direct call to the service. */
    UserManagementService.AuthorizeLocalUser(viewModel.Username, viewModel.Password);
    if(UserManagementService.LocalUser.AuthorizationState != AuthorizationState.Authorized){
    	viewModel.ErrorMessage = "Failed to login! Incorrect username or password!";
    } else {
    	viewModel.ErrorMessage = string.Empty;
    }
}
```

>The key point of the newly added piece of code is that it checks current authorization state and sets the corresponding error message.

We have succesfully modified LoginScreenController to set ErrorMessage

###### Learn how to connect view with GUI

Our last step is creating a text object in the MainMenuScene to show ErrorMessage.

Open `Assets/ExampleProject/Scenes/MainMenuScene.unity` (1) and in the scene hierarchy navigate to `_MainMenuSceneRoot/MainMenuCanvas/LoginUI/LoginScreenPanel` (2):
![](images/img_0015.png)

Inside of this object create UGUI text (1) object. Name and style it however you want (2).

![](images/img_0016.png)

 Now, in the hierarchy navigate to `_MainMenuSceneRoot/MainMenuRoot/LoginScreen`(1). Select this object. In the inspector locate Binding section. Expand it (2). Locate setting for ErrorMessage property (3). Drag the text object you created earlier to the Input field (4).

![](images/img_0017.png)

###### Perform final test
Run the scene. Try to enter invalid data in the login screen and press Login. You should be able to see your message now.

![](images/img_0018.png)
