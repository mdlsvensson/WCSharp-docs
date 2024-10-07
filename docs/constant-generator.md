The ConstantGenerator is a simple program that should be executed from that Launcher. It comes pre-built into the [WCSharp template](getting-started.md). When you run the constant generator, it will generate a number of files:

* `Regions.cs` - Contains WCSharp Rectangle objects corresponding to the regions defined on your map. The Rectangle class has several methods and properties to simplify working with the regions.
* `Cameras.cs` - Contains camerasetup objects corresponding to the cameras defined on your map.
* `Constants.cs` - Contains constant integers for all of the custom units/spells, etc. (everything in the object editor) that you have added to your map. **NOTE:** the name must be different from the base object in order to detect it. If you want to use the original name of the object, please add or modify the editor suffix.

# Options

When running the constant generator, an options class can be passed along to specify how you want the constants to be generated. At present, these are the options available:

| Name | Description |
|---|---|
| IncludeCode | If true, includes the FourCC code in the generated name, i.e. "ABILITY_A001_FAERIE_FIRE" instead of "ABILITY_FAERIE_FIRE". Enabling this will prevent naming conflicts. |