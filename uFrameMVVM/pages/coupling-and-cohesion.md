# Coupling and Cohesion

When it comes to programming architectures we hear a lot about Coupling, Decoupling, High Cohesions, and Low Cohesion. In this tutorial I'll try to explain what they are and why they're important.

Coupling and Cohesion are usually defined as patterns, though they do not define any specific kind of code structure, like other patterns do. I usually like to define them as sort of semi-measurable characteristics. By semi-measurable I mean that we don't measure them using numbers, but instead make distinctions between high and low amounts of Coupling and Cohesion in our code.

The magic rule is the following: if you achieve Low Coupling and High Cohesion, your code will automatically become much more readable, maintainable, extendable, and reusable.

# Coupling

Coupling basically defines how much your classes are connected with each other. Consider the following example:

![](http://i.imgur.com/0633uaF.png)

Here we have high coupling of ClassC with ClassA and ClassB, because they have a link to ClassC. What's the problem with doing this? If you change ClassC, you'd then have to check that nothing in ClassA and ClassB is broken, because they rely on ClassC and use it's API.

Now suppose we have another class called ClassD. This guy does the same thing as ClassC in a slightly different way, and has the same API. And suppose we now want to use it instead of ClassC. The way we've designed ClassA and ClassB to work with ClassC has created another problem: with this setup you cannot use ClassD in place of ClassC without also having to change ClassA and ClassB because they're tightly coupled with ClassC.

Now consider the following system:

![](http://i.imgur.com/UwUAZ1Y.png)

Here our classes are loosely connected, instead making use of Interfaces. Now we can say we have low coupling, because ClassA and ClassB have no idea about ClassC or ClassD, and we can swap out ClassC and ClassD without having to touch any code in ClassA or ClassB.

So the less coupling you have, the less code you have to modify, and you can save your time for something else. If you google for ways to decrease coupling, you'll find a whole set of patterns to help in different situations. But again, the most crucial rule is to use Interfaces as much as possible.

# Cohesion

Cohesion defines to what degree different **pieces of functionality** are related to one another inside of individual **modules**. Usually by module we mean a **class** and by pieces of functionality we mean **methods**. In this example, we have a class called Printer:

![](http://i.imgur.com/teY0AOG.png)

We see a method called Print() and that's great, a Printer should be able to Print(). But we also have:

    public void PrepareHead(){
    ....
    }

    public void LoadInPaper(){
    ....
    }

    public void MakeSomeCoffee(){
    ....
    }

    public void SendEmail(){
    ....
    }

...and so on. This is an example of Low Cohesion, because printing, making coffee, and sending emails are unrelated or very loosely related tasks. Our printer has too many responsibilities. Low Cohesion is bad because the class eventually becomes very hard to manage since it has tons of code.

Here's another way we could design our Printer and other office tasks trying for High Cohesion:

![](http://i.imgur.com/3X03ya1.png)

We have certain classes doing certain things, and we always know where to look for different pieces of functionality. Moreover we can now reuse certain classes elsewhere in our application instead of needing to have to use the entire super-printer everywhere.

Notice how Printer delegates certain tasks to the other classes. Notice also that we have decoupled Printer and its components using Interfaces. This is how you should structure you code: always keep your Cohesion high with a preference for delegation when it makes sense.

Again if you google for ways to increase Cohesion you will find some patterns and tricks, but delegation is your first and foremost friend when it comes to separating your functionality.
