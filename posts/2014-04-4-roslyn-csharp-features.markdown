---
date: 2014-04-5 17:00:00
---

At Build 2014, Microsoft unveiled a few new features of C#.
Some of these are simply 'syntactic sugar', while others are
going to be really useful.

Here's a few of my favourites.

## Statics in Using Statements

Visual Basic had this already, but statics in using statements are now in C#.

This means that you can add static classes as using statements, bringing all
of that class' static members into scope.

Essentially it means you can now replace this:

```csharp
class Program
{
  static void Main(string[] args)
  {
    Console.WriteLine("Hello world");
    Console.WriteLine("Another Message");
  }
}
```

with this:

```csharp
using Console;

class Program
{
  static void Main(string[] args)
  {
    WriteLine("Hello world");
    WriteLine("Another Message");
  }
}
```

This is a small change, but could help to make code more readable.


## Primary Constructors

We write code like this all the time:

```csharp
public class Shape
{
  private readonly int _sides;

  public Shape(int sides)
  {
    _sides = sides;
  }
}
```

This is a constructor that takes an argument, that is then stored in a private member.
This is especially common when using an IoC container; where we pass in
our dependencies, store them in private members and use them later.

C# can now generate this boilerplate constructor code for us, and it looks a lot nicer:

```csharp
public class Shape (int sides)
{
  private readonly int _sides = sides;
}
```

In fact, you don't even need to define the *private readonly* backing
variable; you can declare access modifiers for the members like this:

```csharp
public class Shape (private readonly int sides)
{
}
```

This means that a constructor will be auto-generated, taking *sides* as an
argument. It will then be mapped to a *private readonly* member variable.

##Binary literals

You can now declare binary literals, and use 'spacers' in them to make them
more readable. Like this:

```csharp
var binaryValue = &0010_0100
```

## Dictionary Initialisers

In the latest C#, you can populate indexers when initialising objects.

Replacing:

```csharp
var someClass = new MyClass();
someClass["value1"] = 1
someClass["value2"] = "Hello World";
```

with:

```csharp
var someClass = new MyClass() { ["value1"] = 1, ["value2"] = "Hello World" };
```


## Indexed Members

There is a new addition that means you can make indexers
look a little more like 'normal' members, so that:

```csharp
myClass["Value1"]
```

can now be written as:

```csharp
myClass.$Value1
```

This isn't much, but it certainly looks a lot neater.

## Declaration Expressions

This is my *favourite* new feature. It's only small, but it's something i've been
wanting for a long time.

You can now declare variables anywhere, which means you can finally
declare your out parameters as you use them, rather than on the line
before.

No more of this:

```csharp
var outValue = 0;
var result = int.TryParse(someValue, out outValue);
```

Now we can do this:

```csharp
var result = int.TryParse(someValue, out var outValue);
```

The compiler can infer the type of the out parameter; so we
are even allowed to use *var* here.

## Exception Filters

This is another Visual Basic feature that is coming to C#. It wasn't shown in
the demo at Build, but it allows us to add filters to our catch blocks.

It will probably look something like this:

```csharp
try
{

}
catch (Exception e) if (myVariable == someValue)
{
  // Some exception code that happens when the catch fires
  //  and myVariable ==  someValue
}
catch (Exception e)
{
  // Catch everything else..
}
```

## So when can I use it?

These features are currently in preview as part of the latest Roslyn release.
It's probably best not to use this stuff in production yet, but we can already
download it and play around with it until it's officially released.

You can find it
<a href="http://msdn.microsoft.com/en-gb/vstudio/roslyn.aspx" target="_blank">here</a>
