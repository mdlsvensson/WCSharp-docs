**This page needs to be expanded with more exact definitions.**

A number of convenience methods are defined to make mapmaking easier. Below is a list of the various features:

* WorldBounds provides an easy way of accessing the world bounds of a map and various properties of it in an efficient manner.
* Delay is a small class that can be used to delay a piece of code by a single time step. This can be used to circumvent various issues, such as unit AI locking up if you give them a new order at the same time as they start an attack.
* Extension methods are provided for various things, such as dealing damage or healing a unit, or iterating a group in an efficient manner.
* Data classes are provided to simplify working with points and regions (rectangles).
* Util provides various convenience methods for things such as calculating the distance/angle between points, or handling floating text.

### IterateWithRemoval

This method within ListExtensions deserves a special mention, as it implements behaviour to iterate a list and remove items from it as-you-go. In normal C#, this can be easily implemented with a for-loop, but due to quirks of the C# to Lua transpilation process the standard way of doing this breaks. There are various ways around this, the easiest approach being to use a separate list instead, but that eats performance. This extension method instead provides an easy-to-use generic means of iterating any list with in-between removals via a while loop, making it better for performance.

### Lua/LuaTable

These two classes exist to make it a bit easier for WCSharp to interact with Lua directly. In all likelihood, you want to ignore this. But, if you are a package developer yourself, I can strongly recommend looking into it.

Throughout the development of WCSharp, I've had to deal with a lot of issues surrounding executing raw Lua code from a package, and the way I've solved it ended up working incredibly well. In the end it was an incredibly simple approach, but it took me a long time to get there. So by all means, learn from the code there and don't suffer like I did.

### FastUtil

As of v2.1, a `FastUtil` class is defined alongside `Util`. However, unless you're desperate for performance, you can just ignore this.  
The `FastUtil` methods will inline their calls for maximum performance. However, because of the way this is done, some arguments supplied to its methods should be computed in advance. See IntelliSense to determine which arguments require this. Example:

```csharp
// GOOD
var unit = GetTriggerUnit();
var angle = AngleBetweenPoints(unit, 0, 0);

// BAD
var angle = AngleBetweenPoints(GetTriggerUnit(), 0, 0);
```

Additionally, any methods not present in `FastUtil` are either not able to be inlined, or are already inlined by default (as no argument has the above problem).

### TypeWrapper

TypeWrapper is a simple wrapper class that can be used to circumvent issues with reflection. This class has no purpose outside of reflection.

Normally speaking, if you create a method that expects an argument of type `unit`, or any other WC3 type, this causes problems with matching that type using reflection, as it may be unable to determine the type of `unit`. By changing the argument to `TypeWrapper<unit>`, the issue is resolved.
