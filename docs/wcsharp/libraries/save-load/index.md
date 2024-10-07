# WCSharp.SaveLoad

**Use of this package requires that the compilers IsExportMetadata property is set to true. If you want a non-metadata reliant version, you can still use v1. However, note that v2 is not backwards compatible, meaning you cannot upgrade from v1 to v2 without all existing saves becoming invalid.**

The save system is a powerful system that provides the following features:

* Automatic saving and loading of complete classes
* Supports any number of save slots
* Automatically handles synchronisation of saved data
* Protects save files from tampering via hashcodes, optionally including the playername
* Each save system is its own instance, allowing multiple save formats and systems to be active on a single map

## Usage

Version 2 of WCSharp.SaveLoad is backed by [WCSharp.JsonConvert](../json-convert/index.md) package, allowing it to save generic and complex data structures, including nested structures. The full scale of just what you can save is better explained in [JsonConvert's supported formats](../json-convert/supported-formats.md) and the [Save/Load Example](example.md).

Along with adding the capability of saving virtually any data structure, v2 also allows for the management of multiple save systems. Why is this useful? Well, using it you can interact with a variety of different saves within a single map, so that you can e.g. have a save file that is for the specific map, but also a save file that is shared between multiple maps.

To set up a new SaveSystem, you need to supply it with a `SaveSystemOptions` like this:

```csharp
new SaveSystem<MySave>(new SaveSystemOptions
{
	SaveFolder = "MySaveFolder",
	...
});
```

The following properties are defined on `SaveSystemOptions`:

| Name | Description |
|---|---|
| Hash1 | Must be greater than 0. It is recommended to pick a large prime number. |
| Hash2 | Must be greater than 0. It is recommended to pick a large prime number. |
| Salt | May not be empty. The salt to use on the string. You can just type or generate something random. Around 16 characters is sufficient. |
| SaveFolder | May not be empty. The folder in which to store the saves. |
| BindSavesToPlayerName | Whether saves are bound to the name of the player. If true, saves will have the player name contained in the filename, and upon loading this will be matched with the current player's name. |
| Suffix | Optional. The given string will be added to the filename of any save stored. |
| Base64Provider | Optional. The save is encoded in Base64, if you want, you can provide a custom Base64 provider to effectively scramble the save. This does not change much in terms of protection, but makes it harder for people to inspect save files. |

More information on these properties can be found through IntelliSense within Visual Studio.

Finally, interaction with saves is primarily done via the `Save` and `Load` methods, as well as the `OnSaveLoaded` event. For details on their usage, please check the [Save/Load Example](example.md).
