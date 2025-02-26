---
date: 2017-05-06 10:00:00
---

# Leaking memory with lambda expressions

Lambda expressions are a great feature of C#, but it's also quite easy to accidentally 'leak' memory when using them.

When a lambda expression references a local variable, that expression (and the local variables it references) end up on the heap.
If you use multiple lambda expressions within the same scope (a method, for example), all of them (and the multiple variables they reference) will have the same lifetime.

In other words; that lambda expression that references the huge array, but is only used in the scope of the method, won't be disposed when the method returns like you expect.. it will hang around until the last lambda created in that method is collected.

## Why your local variable is heap-allocated

Lambda expressions don't actually exist in the CLR, they are compiler magic, so we have to do something with them in order for the runtime to understand them.

If your expression doesn't refer to any local variables, the compiler can create a static method and rewrite your code to call that instead.

If you reference a local variable though, things are a little more complicated..

When you create a lambda expression, it's perfectly possible that it could outlive the stack-frame. For example; it could be returned from the method or allocated to a member variable somewhere. That's no good when you use a local variable in that lambda, because that variable might no longer exist when the lambda is used later.

Because of that, the compiler has to keep it all on the heap. It does this by creating a new class with a method containing the contents of your lambda expression. The variables referenced in your expression become member variables in this new class.

Your original code is then re-written to call this new method, safe in the knowledge that the the variables it uses will be there - kept alive in the new class.

Here's the catch though - if you use multiple lambda expressions in the same method, they are created as methods *in the same comiler generated class*. That means that every variable used in those expressions will stay alive until that class can be disposed.

Let's look at an example..

## Leaky lambda expressions in practice

```csharp
public class MyClass
{
    public Func<int, int> Add1()
    {
        var bigThing = new byte[1024 * 1024 * 1024];
        Func<int> bigThingCount = () => bigThing.Length;
        Console.WriteLine(bigThingCount());

        var smallThing = 1;
        return x => x + smallThing;
    }
}
```



Here we have a (slightly contrived) method with two lambda expressions. 

The first of them refers to a large byte array. It calculates the size of this array so we can write it to the console. We'd expect this to be cleaned up after the method returns.

The other only closes over an integer, and is totally unrelated to the first. We return this lambda to the caller. 

Let's take a look and see how this second expression keeps the large byte array alive.

This is what the compiler generates for this class:

```csharp
public class MyClass
{
    [CompilerGenerated]
    private sealed class <>c__DisplayClass0_0
    {
        public byte[] bigThing;

        public int smallThing;

        internal int <Add1>b__0()
        {
            return this.bigThing.Length;
        }

        internal int <Add1>b__1(int x)
        {
            return x + this.smallThing;
        }
    }

    public Func<int, int> Add1()
    {
        MyClass.<>c__DisplayClass0_0 <>c__DisplayClass0_ = new MyClass.<>c__DisplayClass0_0();

        <>c__DisplayClass0_.bigThing = new byte[1073741824];
        Func<int> func = new Func<int>(<>c__DisplayClass0_.<Add1>b__0);
        Console.WriteLine(func());

        <>c__DisplayClass0_.smallThing = 1;
        return new Func<int, int>(<>c__DisplayClass0_.<Add1>b__1);
    }
}
```


Firstly, you can see that the compiler has generated a new class for us, named `<>c_DisplayClass0_0`. We can also see that both local variables (`bigThing` and `smallThing`) have been hoisted to become members of this common class.

The re-written `Add1()` method sets these properties, rather than creating local variables.

It then returns a function that refers to the method in our new class, therefore keeping everything alive until this returned function is collected.

Lambdas are a great language feature, but use them with caution.

If you want to find out more about how lambdas work in C#, check out:
<ul>
    <li><a href="https://blogs.msdn.microsoft.com/ericlippert/2007/06/06/fyi-c-and-vb-closures-are-per-scope/    " target="_blank">A post on this subject</a> by Eric Lippert</li>
    <li>A good article on C# closured from <a href="http://www.codethinked.com/c-closures-explained" target="_blank">CodeThinked</a></li>

    
</ul>
