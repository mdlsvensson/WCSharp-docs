**Use of this package requires that the compilers IsExportMetadata property is set to true.**

Thanks to Lua having the option to retrieve the OS time, WCSharp is able to retrieve the time for each player and use them to acquire a synchronised time for all players. However, unlike the C# built-in types of DateTime and TimeSpan, Lua's precision only goes to seconds instead of ticks, and Warcraft III is currently unable to even perform 64-bit calculations.

As a result, even if we can retrieve the current time, DateTime and TimeSpan cannot be used.

This is where WCSharp.DateTime comes in, as it recreates both DateTime and TimeSpan in a Warcraft III/Lua compliant fashion. As such, for the most part I recommend reading the official documentation on [DateTime](https://docs.microsoft.com/en-us/dotnet/api/system.datetime?view=net-5.0) and [TimeSpan](https://docs.microsoft.com/en-us/dotnet/api/system.timespan?view=net-5.0) if you want more basic information on their functionality.

# Local time

Getting a players local time is very easy to retrieve, simply use `WcDateTime.LocalTime` or `WcDateTime.LocalTimeUtc`. However, note that this is only the time for the local player! If you're confused about what I'm saying, then you definitely don't want to use this. The local time is different for each player, and using it recklessly will result in a desync.

If we want a timestamp that is safe to use under all circumstances, we need a timestamp that is the same for all players: a synchronised time.

# Synchronization

Synchronization of time is performed via the `WcDateTime.GetCurrentTime` method. Since retrieving a synchronised time is a process that cannot be instantly completed, a callback must be supplied which will receive the WcDateTime once it has been calculated.

Behind the scenes, the times of all players retrieved individually and collected using [WCSharp.Sync](sync.md), after which a synchronised time is decided via the supplied `DateTimeSyncMethod` enum. The following methods are supported for this:

| Name | Description |
|---|---|
| Earliest | Picks the earliest time among all players. |
| Latest | Picks the latest time among all players. |
| Average | Picks the average time of all players. |
| BestFit | Default. Picks a time that minimizes the time difference of the chosen time compared to that of all players. In practice, this means it will pick the middle player on uneven player counts, or the average of the middle two players on even player counts. |

For retrieving updated timestamps after initial calculation, the `WcDateTime.TryGetCurrentTime` method is a useful option. This will return the current synchronised WcDateTime immediately using results of earlier requests if possible.
