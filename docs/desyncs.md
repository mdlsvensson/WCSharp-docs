# Desyncs

This page is primarily dedicated towards desyncs within the C# context. There are a lot of things that can cause desyncs, but for the more general ones I recommend looking at other online sources, such as [this thread on Hiveworkshop](https://www.hiveworkshop.com/threads/known-causes-of-desync.317486/).

## GetHandleId

Do not use GetHandleId. Ever.

This is a frequently used tool back from the JASS era, however it has **zero use** in lua and by extension C#. For dictionaries, you can simply map the handle itself rather than the handle id. Performance-wise this is at least as fast as using the handle id, and it ensures that if you forget to clean up any handle ids, no desyncs can occur.

In simple terms:

- If you make a mistake using handles, you leak some memory.  
- If you make a mistake using handle ids, you desync the entire game!

As such, it makes no sense to use GetHandleId within the context of C#/lua. Doing so exposes you to a number of risks while providing no advantages.

## Dictionary/HashSet enumeration

This uses the lua `pairs` keyword in the background, which is prone to desyncing. You can use a SortedDictionary instead. Alternatively, you can also sort the results prior to enumerating, however...

## Sorting

Lua does not have a stable sorting algorithm. For example, consider the following code:

```csharp
record MyData(int Value, string Name);

var list = new List<MyData>
{
	new MyData(2, "Foo"),
	new MyData(3, "World"),
	new MyData(2, "Bar"),
	new MyData(1, "Hello"),
};

list = list.OrderBy(x => x.Value).ToList();
// Possible output order:
// [(1, "Hello"), (2, "Foo"), (2, "Bar"), (3, "World")]
// [(1, "Hello"), (2, "Bar"), (2, "Foo"), (3, "World")]
```

This is a problem if we're intending to do anything with the `Name` property.

The best way to address this problem is simply by ensuring that each time you perform a sort, there is only a single valid output:

```csharp
list = list.OrderBy(x => x.Value).ThenBy(x => x.Name).ToList();
```