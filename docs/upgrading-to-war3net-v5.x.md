If you want to upgrade an existing map that uses WCSharp from War3Net 3.x to 5.x, there's a few things you'll have to do. I'd say that there is an easy and a hard way, and I'll go over them both.  

Note that for either approach, you'll still need to address breaking changes from WCSharp v2. This should primarily be addressing the changes made to the buff system, as well as the namespace change from Utils to Shared (I recommend a find & replace using Visual Studio or Notepad++ for that part).

### The easy route

I personally recommend the easy route, which is to copy over the entirety of the [WCSharp template](getting-started.md), and just copy/paste the `.cs` files of your own Source project into the template, as well as your `source.w3x` folder. All the WCSharp packages come pre-installed on this template, and your build chain should be good right away.  
If you were using the old Save/Load system, you'll want to open up NuGet Package Manager and downgrade WCSharp.SaveLoad to v1.4.0.

### The hard route

If you don't want to use the template for some reason, you'll have to go the long route. I will try to go over the steps:

1. In your project properties (for both launcher/source), set the "Target framework" to ".NET 5.0"
2. You'll need to update your Launcher's `Program.cs` considerably. If you want you can try and figure it out using the [War3Net's template](https://github.com/Drake53/War3Map.Template/tree/master/src/War3Map.Template.Launcher), but personally I recommend just copying the entirety of the [WCSharp template](getting-started.md) for this part.
3. Uninstall WCSharp, then install all individual WCSharp packages. If you were using the Save/Load system, you'll probably want to install WCSharp.SaveLoad v1.4.0 instead of v2.
