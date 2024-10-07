**Use of this package requires that the compilers IsExportMetadata property is set to true.**

The following formats are supported:

* All primitives (bool, int, etc.)
* Strings
* Enums
* Arrays, of which the element type must be a supported format
* Classes that implement `IDictionary<,>`, of which the key type must be a primitive or a string, and the value type must be a supported format
* Classes that implement `ICollection<>`, of which the value type must be a supported format.
* All other classes will be serialized/deserialized according to public properties. The types of all public properties must be a supported format. Note that get or set only properties are not supported and will result in an error.

Furthermore, note that all classes must have a parameterless constructor, including those derived from `IDictionary<,>` and `ICollection<>`.  
Additionally, `KeyValuePair<,>` cannot be (de)serialized outside of their implicit occurance within dictionaries due to how they are implemented within CSharpLua.

Finally, there is special support for classes that implement a public static `Serialize` and `Deserialize` method. These methods are used to (de)serialize a class to and from a string representation. Within WCSharp, `WcDateTime` and `WcTimeSpan` both have these methods, and can therefore be (de)serialized despite lacking a parameterless constructor. For example, the below code illustrates how this is implemented in WcDateTime.cs:

```csharp
public static WcDateTime Deserialize(string @string)
{
	if (int.TryParse(@string, out var seconds))
	{
		return new WcDateTime(seconds);
	}

	return null;
}

public static string Serialize(WcDateTime wcDateTime)
{
	return wcDateTime.seconds.ToString();
}
```

Since this approach has the class itself create a new instance, classes which implement a public static `Serialize` and `Deserialize` method do not require a parameterless constructor.
