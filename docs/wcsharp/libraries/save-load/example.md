# Save/Load Example

**Use of this package requires that the compilers IsExportMetadata property is set to true.**

This page illustrates a more complex example of how you may want to save/load data in a map, and showcases the strength of WCSharp's save/load system.

In this case, we will design a save file for a hero survival map. Each hero will have its own progression that is individually tracked. Since the save/load system is backed by the [WCSharp.JsonConvert](../json-convert/index.md) package, we can save nested data, as well as dictionary structures. This allows us to very easily create a structure with which we can save separate data for all heroes.

To start off, lets define an enum with 3 heroes that the player:

```csharp
public enum Hero
{
	Archmage = 1,
	Blademaster = 2,
	DemonHunter = 3
}
```

It's important to give the enums an integer value, as the values that these enums map to should be consistent across different compilations of the same map. Instead of just 1/2/3, you could also use e.g. the unit type IDs so you can easily convert between them.

Anyway, now that we have this enum, we can set up a dictionary mapping these enums to our hero data:

```csharp
using System.Collections.Generic;
using WCSharp.DateTime;

public class HeroData
{
	public int Level { get; set; }
	public float Xp { get; set; }
	public int Kills { get; set; }
	public int Wins { get; set; }
	public int Losses { get; set; }
	public WcDateTime LastPlayed { get; set; }
}

public class MySave
{
	public Dictionary<Hero, HeroData> Heroes { get; set; }
}
```

With this we already have a powerful structure. With two classes and a single dictionary, we can store level, xp, kills, wins and losses for each hero, without needing to concern ourselves with avoiding name conflicts or anything like that. We're even using [DateTime](../date-time.md) to store when the player last played that hero!

We could also store more information outside of the `Heroes` dictionary if we want, but for now lets move on to actually turning this into something we can save and load. The first step is very easy, we just need to make `MySave` extend the `Saveable` class like this:

```csharp
using System.Collections.Generic;
using WCSharp.SaveLoad;

public class MySave : Saveable
{
	public Dictionary<Hero, HeroData> Heroes { get; set; }
}
```

We don't need to add anything else, just adding the `: Saveable` will add all we need, namely methods to get/set the player and save slot of this save.

Now in order to easily interact with all our saves, lets create a static class to manage them that other components can easily reach:

```csharp
using System.Collections.Generic;
using WCSharp.SaveLoad;
using WCSharp.Util;

public static class SaveManager
{
	public static Dictionary<player, MySave> SavesByPlayer { get; } = new Dictionary<player, MySave>();
	private static SaveSystem<MySave> saveSystem;
	
	public static void Initialize()
	{
		// Do not just copy/paste these options, you should pick your own hash and salt values
		// You can use IntelliSense to get more information about the options
		// Just know that Hash1, Hash2, Salt and SaveFolder are required
		saveSystem = new SaveSystem<MySave>(new SaveSystemOptions
		{
			Hash1 = 775807,
			Hash2 = 456023,
			Salt = "ZSLJ96ED6sPwYkQM",
			BindSavesToPlayerName = true,
			SaveFolder = "MyHeroSurvivalMap"
		});

		saveSystem.OnSaveLoaded += SaveManager_OnSaveLoaded;

		foreach (var player in Util.EnumeratePlayers())
		{
			saveSystem.Load(player);
		}
	}
	
	public static void SaveManager_OnSaveLoaded(MySave save, LoadResult loadResult)
	{
		SavesByPlayer[save.GetPlayer()] = save;
		
		// If the load result is anything except success, the save will be a newly created object
		if (loadResult != LoadResult.Success)
		{
			// You can also just set the default value of the property to this.
			// This is just to illustrate why you may want to know when it is an empty save,
			// as then things like the heroes dictionary will not be created or filled.
			save.Heroes = new Dictionary<Hero, HeroData>();
		}
		// Extension method for determining whether the load result is any of the failed states
		if (loadResult.Failed())
		{
			Console.WriteLine("An existing save failed to load correctly!");
		}
	}
	
	public static void Save(MySave save)
	{
		saveSystem.Save(save);
	}
}
```

Now once we call `SaveManager.Initialize`, it will fetch the save files on save slot 1 for all players and return them to you via the `SaveManager_OnSaveLoaded` method, which then stores them in a dictionary so that other components can reach them. If the player does not have a save file, it will return a new instance of `MySave` and indicate that this happened via the `isEmptySave`, in case you need to perform special steps for new save files.

Now once the map completes, we can update and store our data something like this:

```csharp
var save = SaveManager.SavesByPlayer[currentPlayer];
if (!save.Heroes.TryGetValue(pickedHero, out var heroData))
{
	heroData = new HeroData();
	save.Heroes[pickedHero] = heroData;
}
heroData.Level = updatedLevel;
heroData.Xp = updatedXp;
heroData.Wins++;
heroData.Kills += killsThisGame;
heroData.LastPlayed = currentTime;
SaveManager.Save(save);
```

And that's it! This is all you need to set up a basic save/load behaviour and save/load just about any data you might be interested in. Note that you will probably want to expand this example a bit further by e.g. ensuring that all save files are loaded before the map starts properly, or ensuring in some way that your components don't try to interact with a save for a player that it doesn't have.
