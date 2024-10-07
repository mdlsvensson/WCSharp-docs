# WCSharp.JsonConvert

**Use of this package requires that the compilers IsExportMetadata property is set to true.**

Although probably not directly useful to the average map maker, JsonConvert is the backbone for various other systems such as [WCSharp.SaveLoad](../save-load/index.md), [WCSharp.Sync](../sync.md) and [WCSharp.DateTime](../date-time.md).

WCSharp.JsonConvert allows you to serialize and deserialize generic classes into JSON format and back. This is used by [WCSharp.SaveLoad](../save-load/index.md) and [WCSharp.Sync](../sync.md) to easily store or send complex data formats without requiring users to define additional logic.

By using this format, nested data is also supported, for example:

```csharp
public class First
{
	public int A { get; set; }
	public Second B { get; set; }
}
public class Second
{
	public string C { get; set; }
	public List<Third> D { get; set; }
}
public class Third
{
	public float E { get; set; }
	public Dictionary<int, Fourth> F { get; set; }
}
public class Fourth
{
	public int G { get; set; }
}
```

This structure is fully supported, for example, the below code:

```csharp
var first = new First
{
	A = 5,
	B = new Second
	{
		C = "SecondString",
		D = new List<Third>
		{
			new Third
			{
				E = 3.14f,
				F = new Dictionary<int, Fourth>
				{
					{ 42, new Fourth{ G = -7 } },
					{ -17, new Fourth{ G = 55 } }
				}
			}
		}
	}
};
Console.WriteLine(JsonConvert.Serialize(first));
```

This will output `{"B":{"D":[{"F":{"-17":{"G":55},"42":{"G":-7}},"E":3.14}],"C":"SecondString"},"A":5}`, or with indentation (it currently always outputs non-indented JSON):

```json
{
	"B":{
		"D":[
			{
				"F":{
					"-17":{
						"G":55
					},
					"42":{
						"G":-7
					}
				},
				"E":3.14
			}
		],
		"C":"SecondString"
	},
	"A":5
}
```

Note that attributes are not supported, which means that properties are always serialized/deserialized according to their exact property definitions.

For a more Warcraft III related example, you can check out this [Save/Load example](../save-load/example.md).
