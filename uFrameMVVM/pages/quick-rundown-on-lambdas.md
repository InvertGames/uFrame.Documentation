# Quick Rundown on Lambdas (=>)

Lambdas are used a lot in uFrame, which may be confusing for a large amount of beginners/intermediates.  They're really nothing to be scared of, and once you're comfortable with them, they can be very handy.  Lambdas are a way of defining an _anonymous_ method, meaning it's a block of instructions, but it doesn't have a name like regular functions (ie- DoSomething()).  When you declare the lambda, the left side is parameters, and the right side is the block of instructions, whatever you want it to do with those parameters.

> .Subscribe(**someInt** => Debug.Log("The integer is "+**someInt**));

So the lambda above is being passed a parameter _someInt_ that is probably an integer, and the right side lambda's instructions are to spit that integer _someInt_ out to the console.  You could do the exact same thing with a named method, but that would be more typing...

```cs
    .Subscribe(LogInt);
    // Further below, defining the method
    public void LogInt(int myInteger) {
        Debug.Log("The integer is "+myInteger);
    }
```

You could also define more stuff in a lambda by using the standard function block **{ }**.

```cs
    .Subscribe(someInt => {
        someInt += 2;
        Debug.Log("The integer plus 2 is "+someInt);
        });
```

Usually however, lambdas are used for defining small, simple instructions, not large definitions.
## Naming Conventions
For legibility and sanity, when using lambdas you should always name the left-side args something obvious that denotes their type, even if it's a single letter.  So if it's an integer, you would use "i" at the very least:

> **i** => Debug.Log("The integer is"+**i**)

You may also have seen an underscore character used as the variable.  Naming convention dictates that if you aren't going to use the parameter being passed, use the **\_** character to note that you don't care what type it is:

> **_** => Debug.Log("My thing happened, not using and don't care about the arg or its type.")

```cs
    // So for example
    Observable.EveryUpdate().Where(_ => Input.GetMouseButtonDown(0));
    /* The Observable.EveryUpdate() was passing some kind of variable, but
          the lambda definition didn't use, so the person used _ => ... */
    
    /* The underscore is still a character and a variable however, so the passed
          parameter could still be used technically, like... */
          
    Observable.EveryUpdate().Where(_ => Debug.Log( _ ));
    // ... And the console would show whatever value was passed
```

For clarification, technically you can name the args however you want:

```cs
    (mmm, beer) => Debug.Log(string.Format("mmm + beer = {0} + {1}", mmm, beer))
    // mmm is int value of 2, beer is int value of 3, makes no difference
```

It vastly improves legibility when you name them based on type however, so with something like **(i, f) =>**, I would assume is probably an int and a float.
    
## Parameters

```cs
    () =>          // is for zero parameters 
    x => or (x) => // is for one parameter 
    (x, y) =>      // is for two parameters 
    (x, y, z) =>   // is for three, etc... 
```

For one parameter, since there's only one... you don't need parenthesis, so they're omitted by most people, and zero still requires SOME kind of something for the compiler to recognize the lambda, so empty () parenthesis is used.

## Conclusion
Don't be scared of lambdas, because they're super simple: **(args) => instructions**, read as "_(args) goes to instructions_".
