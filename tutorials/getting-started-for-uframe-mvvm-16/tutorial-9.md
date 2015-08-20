# uFrame MVVM 1.6 Getting Started IX



Overview


Steps

Learn how to install uFrame MVVM
Learn how to setup example project
Learn how to add property to the element
Learn how to add binding to the view
Learn how to change logic in controllers
Learn how to connect view with GUI
Perform final test

Average Time

6-8 minutes


Transcript
Introduction:
Hello and welcome to uFrame 1.6 getting started series.
This part will show how to install uFrame MVVM and how to setup default project. It also shows how to make one simple change to the default project.

How to install uFrame MVVM
Using asset store, you can install uFrame MVVM just like any other asset: simply import it.

<<<PICTURE OF IMPORT BUTTON>>>

As for uFrame MVVM 1.6, you will find folder called uFrameMVVM inside of your assets folder. It contains 2 other packages with different uFrame MVVM versions. Version 1.5 is shipped for backwards compatibility. If you are new to uFrame MVVM, please stick with version 1.6. Import the version you want by double clicking on the corresponding package.

Picture with 2 assets

You can safely remove uFrameMVVM folder after this step. This tutorial shows basics of uFrame MVVM 1.6. Tutorials for version 1.5 are available on Invert Games Studios website.
<<<URL TO WEBSITE

Learn how to setup default project

Once you import uFrameMVVM 1.6, Welcome Screen will show up. To manually open Welcome Screen, select uFrame -> Welcome Screen in the menu panel.

<<PICTURE OF uFrame -> Welcome Screen>>

In the Welcome Window, navigate to Example and click “SETUP EXAMPLE PROJECT” button.

<<PICTURE OF NAVIGATION>>



Follow the instructions. Example Package will be deployed into your Assets Folder, all the needed scene will be added to your build settings. Finally intro scene will be opened and run.
<<<PICTURE OF FINAL STATE>>>

Learn how to add property to the element
Open uFrame Designer Window using menu panel Windows -> uFrame Designer.

<<<PICTURE OF Windows -> uFrame Designer

First of all you need to open MainMenuSystem graph. Check opened graph tabs:

<<<<INSERT PICTURE WHICH SHOWS TABS>>>>

If it contains MainMenuSystem, simply click it.
If MainMenuSystem tab is not there, click Graph selection control and select MainMenuSystem:

<<<<INSERT PICTURE WHICH HOW TO OPEN EXISTING GRAPH>>>>

MainMenuSystem graph should open, and you should be able to locate MainMenuSystem graph tab.
<<< PICTURE OF FINAL STATE

Just like with any other graph, MainMenuSystem graph contains nodes. Nodes can represent different information. uFrame MVVM nodes contain special sub-header (or tag) which shows what type of node it is:

<<<<INSERT PICTURE WHICH SHOWS NODE TYPE>>>>

In this step we are interested in Element node which is called LoginScreen.

<<<<INSERT PICTURE WHICH SHOWS LoginScreen NODE>>>>

Element node defines entities of your games in terms of data they hold. For example LoginScreen contains information about Username and Password, which user enters to login into the system. You can collapse or expand node by clicking arrow control in the bottom of the node.
<<<<INSERT PICTURE WHICH SHOWS EXPAND/COLLAPSE CONTROL>>>>

Expand LoginScreen. Let us add new property. For this we need to click + button near the Properties section.

<<<<INSERT PICTURE WHICH SHOWS + button>>>>

New node child item will be created. in the Properties section. Such child item will contain type in the left column and name in the right column.

<<<<INSERT PICTURE WHICH SHOWS newly create prop>>>>

Type represents type of the property and by default it is set to string. Name represents name of the property and by default it is set to “Properties”.

We now need to change the name of the property by double clicking on the current name. Field will become editable and you can type in the name. Let us name it ErrorMessage.

<<<<INSERT PICTURE WHICH SHOWS final ErrorMessage property>>>>

This property will be of type string and we do not have to change. However, if you want to change the type of the property, you can click the current type and select desired type from the window which pops up.

<<<<INSERT PICTURE WHICH SHOWS Show window and type selectio>>>>

We have successfully added new property to the LoginScreen element.

Learn how to add binding to the view
Being on the MainMenuSystem graph, let us double click header of LoginScreen element node. This will let us get deeper into the details of the node. As a matter of fact, MVVM stands for Model View ViewModel. While Element node defines Model and ViewModel, View is expressed differently. In this example, View is represented using a View Node called LoginScreenView. It is important to note, that element can have unlimited number of views.

<<<<INSERT PICTURE WHICH SHOWS LoginScreenView NODE>>>>

We plug LoginScreen element node (A) into LoginScreenView element input (B), to express that LoginScreenView represents LoginScreen data. We can also see that LoginScreenView inherits from SubScreenView. You can read more about inheritance in the documentation.

In this step we want to create a binding of ErrorMessageProperty to the Text GUI object.
For that, we need to click plus button near the Bindings section on the LoginScreenView Node.

<<<<INSERT PICTURE WHICH SHOWS + button on bindings>>>>

 A window will popup, with a list of all possible bindings. We need to select “ErrorMessage To Text”.

<<<<INSERT PICTURE WHICH SHOWS bindings selection window>>>>

Once we select it, it should appear in the list of existing bindings.

We have successfully added binding to the LoginScreenView node.

Learn how to change logic in controllers and services

This step involves a little bit of programming. Since we have done some changes to the nodes, we need to Save and Compile our diagram using corresponding button in the top right corner.

<<SHOW SAVE AND COMPILE BINDING

Saving and Compiling process turns diagram items and nodes into CSharp code.

Once the process is complete, we can start modifying the code.
In the previous step we have introduces UI binding for the ErrorMessage property of LoginScreen. Now we want to specify how we set this property.

Right click on the LoginScreen element node header. Select Open -> LoginScreenController.cs

<<SHOW CONTEXT MENU PICTURE>>

This will open your IDE with the corresponding generated file and let you modify the internals of it. Alternatively, you can manually locate LoginScreenController.cs file in your unity project

We are interested in method called Login. It gets invoked when we try to log in. It delegates login procedure to the UserManagementService along with the Username and Password.

<<GIST OF THE METHOD>>

In the very end of the method body let us add the following code:

if(UserManagementService.LocalUser.AuthorizationState != AuthorizationState.Authorized){
	viewModel.ErrorMessage = “Failed to login! Incorrect username or password!”
} else {
	viewModel.ErrorMessage = string.Empty;
}

We have succesfully modified LoginScreenController to set ErrorMessage
Learn how to connect view with GUI
Our last step is creating a text object in the MainMenuScene to show ErrorMessage.
Open MainMenuScene and in the scene hierarchy navigate to _MainMenuSceneRoot/MainMenuCanvas/LoginUI/LoginScreenPanel.
Inside of this object create UGUI text object. Style it however you want.

<<<SHOW PICTURE WITH STRUCTURE>>>

 Now navigate to _MainMenuSceneRoot/MainMenuRoot/LoginScreen. Select this object. In the inspector locate Binding section. Expand it. Locate setting for ErrorMessage property. Drag the text object you created earlier to the Input field.


<<<SHOW EXACTLY WHERE
We have successfully setup ErrorMessage text object.


Perform final test
Run the scene. Try to enter invalid data in the login screen and press Login. You should be able to see your message now.
<<<SHOW FINAL RESULT
