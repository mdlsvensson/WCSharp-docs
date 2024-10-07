# WCSharp.Api

WCSharp v3.0 offers a new API to replace the default War3Api that comes with War3Net.

The WCSharp API essentially behaves as a wrapper around WC3 handles, allowing you to interact with them in a more natural way:

```csharp
// War3Api
var unit = CreateUnit("hfoo", Player(0), 0, 0, 270); // Create a unit
SetUnitState(unit, UNIT_STATE_MANA, GetUnitState(unit, UNIT_STATE_MANA) + 50); // Increase its mana by 50
KillUnit(unit); // Kill the unit

// WCSharp.Api
var unit = unit.Create("hfoo", Player(0), 0, 0, 270); // constructor support will be added in the future, there's a bug with CSharpLua for the time being
unit.Mana += 50;
unit.Kill();
```

Thanks to CSharpLua templates, despite behaving more like C# objects than WC3 handles, these two pieces of code produce the **exact same code**, but the latter is significantly easier and more intuitive to write.  

Methods and properties exist for almost all WC3 handles that you are likely to use in a project, but if need be you can always fall back to War3Api style code. The WCSharp API still has the entire `Common.j` and `Blizzard.j` accessible via `WCSharp.Api.Common` and `WCSharp.Api.Blizzard`, allowing you to use the same style if need be to access all of WC3's functionality. This also makes it simple to transition a project from War3Api to WCSharp.

## Transitioning from War3Api to WCSharp.Api

**If you are starting a new project, you can simply use the [WCSharp template](../getting-started.md) to get started.**

The procedure isn't the cleanest, but it's fairly simple regardless. The process is as follows:

* In your "Launcher" project, use NuGet to install `WCSharp.CSharpLua.CoreSystem`. Additionally, ensure that `War3Net.CSharpLua` is at least version 2.0.0.
* In your "Source" project, use NuGet to install WCSharp.Api.
* In your "Source" project, use NuGet to install the v3.x versions of any WCSharp packages you are using.
* Use some text editing tool (e.g. Notepad++) to mass replace `using static War3Api.Common;` with `using WCSharp.Api;\r\nusing static WCSharp.Api.Common;`.
* In your "Launcher/Program.cs" file, adjust the part that loads the core system files as such:

```csharp
// Old
var coreSystemFiles = CSharpLua.CoreSystem.CoreSystemProvider.GetCoreSystemFiles();

// New
var coreSystemFiles = CSharpLua.CoreSystem.CoreSystemProvider.GetCoreSystemFiles()
	.Where(x => !x.EndsWith("Common.lua"))
	.Concat(new[] { "CoreSystem/WCSharp.lua" });
```

## Tips & tricks

* Remember that you can always use War3Api style code by simply adding a static import to `WCSharp.Api.Common`.
* All event-related information is centered in the `@event` class, e.g. `@event.Unit` is identical to `GetTriggerUnit();`.
* Whenever you need to clean something up to prevent a memory leak, simply call its `Dispose` method. All actions that remove, destroy or otherwise get rid of a WC3 handle has been unified under `IDisposable` implementations. Due to a bug with CSharpLua however, you should not use `using var` on WC3 handles for now.
* Ability fields provide detailed information regarding which fields they're used in via IntelliSense, allowing you to more easily find which field you should be editing.

## Things to keep in mind

There are a few things to keep in mind when using the WCSharp API.

1. The API is still in an early state. Although it is already feature complete and more than ready for use, please keep in mind that not everything will be perfect and that there may be breaking changes in the future. Feel free to suggest improvements!

2. Because the API is entirely template based, there is no actual C# object, and retrieving any value is invoking a WC3 native. If you intend to use a property multiple times, you should assign it to a variable to avoid multiple native calls!

3. There are some properties that do result in multiple natives being invoked. This is because the actions are simply not possible with a single native call, but they were added to the WCSharp API for convenience. In most cases, this is not important, but if you intend to use these properties very frequently, you may wish to take this into account. These actions are as follows:
	* camerasetup.X.set
	* camerasetup.Y.set
	* item.X.set
	* item.Y.set
	* lightning.Red.set
	* lightning.Green.set
	* lightning.Blue.set
	* lightning.Alpha.set
	* rect.MinX.set
	* rect.MinY.set
	* rect.MaxX.set
	* rect.MaxY.set
	* rect.CenterX.set
	* rect.CenterY.set
	* unit.SkillPoints.set
	* unit.WaygateDestinationX.set
	* unit.WaygateDestinationY.set
	* unit.AttackRange1.set
	* unit.AttackRange2.set
