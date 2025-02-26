---
date: 2020-06-10 17:00:00
---

# Eliminating code comments

Recently I've started to notice that the majority of comments I write are either redundant, or are excuses for other failings in my code. 

Since that realisation, I've been making a conscious effort to eliminate the majority of comments from the code I write.

I find that comments generally fall into a handful of common categories. Here's how I've been dealing with each:

## Comments that should be methods

I often find myself using a comment, when extracting code to a method would be better.

For example:

```csharp
if (customersFirstOrder) {
    // Apply free shipping
    orderCost = orderCost - shipping;
}
```

I find this much easier to read:

```csharp
if (customersFirstOrder) {
    ApplyFreeShipping();
}
```

Now I don't need to understand _how_ free shipping is applied, only that it _is_ applied when this condition is satisfied.

## Comments that explain conditionals

I write `if` statements like this a lot:

```csharp
// This order has an outstanding payment
if (order.invoiced && !order.paid) {
    SendReminderEmail();
}
```

That first line conflates two concerns; whether a payment is due, and what to do when it is. 

Moving the predicate in to a new variable, with an appropriate name, makes the code easier to read and eliminates the need for the comment:

```csharp
var orderPaymentDue = order.invoiced && !order.paid;
if (orderPaymentDue) {
    SendReminderEmail();
}
```

<p class="footer-text">Note: I might even go one step further here and encapsulate the predicate inside the Order object: `order.PaymentDue`</p>


## Comments as a crutch for poor naming 

This most commonly occurs when I'm declaring a variable, then using it some time later. 

If that variable is poorly named, I'll instinctively write a comment explaining what code that uses it is doing.

```csharp
var outstanding = GetOutstandingOrders();
...
// Process all oustanding orders
Process(outstanding);
```

This is simply fixed by using more descriptive names in the first place:

```csharp
var allOutstandingOrders = GetOutstandingOrders();
...
Process(allOutstandingOrders);
```

<p class="footer-text">Note: Shorter functions, where there isn't a large gap between declaring and using a variable would also help, of course</p>


## Comments as an excuse not to refactor

I usually add these comments when I'm writing code that isn't fully-formed in my head yet.

I have some idea what I need the code to do, so I write the first thing that does the job.

This kind of code is often accompanied by a comment that explains _what the code does_, rather than 
_why it does it_.

What I _should_ do is go back and refactor the code to make it self-documenting, making the comment un-necessary.


## Comments that explain why

These comments are probably the only ones I'm going to keep writing.

Sometimes, I come to code I've written a while ago and have _no idea_ why it is written in such a strange way. 

Often, it's because I've profiled it and found it to be significantly faster, or because it catches an edge-case that a more intuitive method of doing the same thing might not:

```csharp
// When profiled, running two seperate queries and combining the results in 
// memory is 2x faster that doing the join in SQL
var users = sql.GetUsers();
var tasks = sql.GetAllTasks();
... etc ...
```


Comments like this get to stay.



<div class="spacer"></div>

## Summary

The majority of comments I write are a born out of lazy coding, and can be eliminated by simply naming things better or adding more abstraction.

I'm find that when I focus on removing the majority of comments from my code, it actually becomes _more_ readable. 
The exception is comments that explain why, not how.


<a href="https://www.twitter.com/davejsaunders" target="_blank">Let me know</a> if you've found the same thing!