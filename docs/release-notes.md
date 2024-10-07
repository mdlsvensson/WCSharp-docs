## 2024-08-02

* Add GetRandomAnglel, GetRandomAngleRad and IsInRange to Util.
* Allow for WCSharp.Missile.RunCollisions to be overridden.
* Change WCSharp.Missiles and WCSharp.Buffs to only skip effect creation if `EffectString` is null, rather than if null or empty string.

## 2024-03-27

* Add SmoothTrigger and SmoothDisposableTrigger to [WCSharp.Events](wcsharp/libraries/events/periodic-events.md#smoothtrigger-smoothdisposabletrigger).

## 2024-03-01 (v3.0.0)

* Add new [WCSharp.Api](wcsharp/api.md) package.
* Update all packages to utilise WCSharp.Api instead of War3Api.
* Fixed the speed of orbital projectiles being incorrect.
* Remove all instances of GetHandleId from WCSharp packages to prevent potential desyncs.
* Fixed an issue where unregistering one event could cause it to unregister all grouped events.
* Add a TypeWrapper class to WCSharp.Shared which acts as a solution to reflection-based type checking of WC3 types. For more information, see the package page.

## 2023-09-24 (v2.2.0)
* Add new [WCSharp.W3MMD](wcsharp/libraries/w3mmd.md) package.
* Periodic events and intervals on buffs and missiles will now potentially tick multiple times in a single frame if the interval is less than a single tick.
* Made many previously internal fields on missiles public properties instead so they can be more easily used for new implementations or those who just prefer radians.
* Each missile now has a set of flight modes to better control how they handle terrain.
* Replaced the Disable/ReactivateArc methods on basic and curve missiles with a property that handles the logic automatically.
* Change Lightning and Missile system to handle Z coordinate retrieval via a new static method in Shared.Util.
* Added an [auto generated documentation](https://github.com/Orden4/WCSharp/blob/master/Docs/README.md) based on the Visual Studio XML documentation.

## 2023-03-31
* Fixed an off-by-one error when sending messages across the SyncSystem. If your save just happened to be `(240*x)+1` characters long, it would generate an invalid message.

## 2023-03-24
* Fixed a potential desync when retrieving a synced DateTime.
* Changed the EnableDebug methods to log both the exception message and the stack trace, instead of just the message.

## 2023-03-03
#### WCSharp.Shared
* Added new extension methods for group enumeration: `FirstOrDefault`, `ToList` and `ToHashSet`. These methods use a faster way of enumerating a group, but also they don't use the slower yield enumeration. In practice, they are always faster than the old `Enumerate` which is marked as obsolete for now.
* Improved the performance of the `Heal` unit extension method.
* The other packages have been updated to utilise these new extension methods where applicable.

## 2023-02-11
#### WCSharp.SaveLoad
* Adding additional ability ids for storage is now done via a new `SaveSystem` instead of `SaveSystem<T>`.
* Default data storage capacity has been increased from 2000 to 6000 characters.
* Storage space warning has been moved from 50% to 75% of capacity.

## 2023-02-11
#### WCSharp.DateTime
* Fixed the `==`, `!=`, `>`, `>=`, `<` and `<=` operators for WcDateTime and WcTimeSpan not handling null values correctly.
* Fixed `CompareTo` for WcDateTime and WcTimeSpan not handling null values correctly.

#### WCSharp.Shared
* Fixed the `==` and `!=` operators for Point and Rectangle not handling null values correctly.

#### WCSharp.SaveLoad
* Fixed multiple SaveLoad systems of the same generic type firing each others event handlers. Meaning, if you create two instances of `SaveSystem<MySave>` and try to load a save on one of them, only the one that you're loading from will fire its event, instead of both.
* Fixed the HashCode comparing the HashCode that it uses to verify save integrity by re-serializing the save file instead of just taking the save string as-is. This could cause it to believe that the save was tampered with if the class file was changed, or if it used dictionaries/hashsets with no guaranteed order.

## 2022-12-24
#### WCSharp.Json
* Removed a leftover debug output
* Fixed HashSets with primitive values not serialising correctly
* Fixed Dictionaries not serialising correctly after a [CSharpLua update](https://github.com/yanghuan/CSharp.lua/commit/ae3b3633577fe25678dbc299171fed7331a3416c)

#### WCSharp.SaveLoad
OnSaveLoaded now returns an enum indicating the state of the save loaded instead of just a boolean. Previously it wasn't possible to determine whether a new save was returned because it failed to load an existing save, or whether the player simply had no save file. This value can now be used to determine that, so that you can potentially warn a player whose save failed to load that continuing to play will cause their save file to be reset.

## 2022-12-23 (v2.1.0)
* Start of this documentation.
* Upgraded all packages to 2.1.0.
* Upgraded to .NET 6.

#### WCSharp.Events
Player unit events rewritten to improve clarity, support single-source events and support listening to the same event multiple times.

## Prior updates
Version 1.x were the early single-package version, with a special WCSharp.SaveLoad v1.4 for backwards compatibility with old saves.
Version 2.0.x were split-package versions for .NET 5.