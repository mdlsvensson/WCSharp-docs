This is a port of W3MMD for C#, a standard for storing game metadata into replay files, which parsers can then utilise in order to display relevant or interesting information about what happened during the game.

Other sources of information on W3MMD:
- [w3gjs](https://github.com/PBug90/w3gjs)
- [w3gPlus](https://github.com/PBug90/w3gPlus)
- [wc3stats](https://wc3stats.com/docs/w3mmd)

This C# port works largely the same as the vJass and TypeScript versions. Using the `W3Mmd` class, you can create events, variables, set player flags and emit custom events. [You can find a detailed list of its methods at the automatic documentation.](https://github.com/Orden4/WCSharp/blob/master/Docs/WCSharp.W3MMD/WCSharp.W3MMD.W3Mmd.md)

Example usage:

```csharp
var killCount = W3Mmd.DefineInt("Total kills", W3MmdGoalType.High, W3MmdSuggestionType.Leaderboard);
var killEvent = W3Mmd.DefineEvent("Kills", "{0} killed {1}", "Killer", "Victim");
PlayerUnitEvents.Register(UnitTypeEvent.Kills, () =>
{
	var killer = GetKillingUnit();
	var victim = GetTriggerUnit();
	killEvent.Emit(GetUnitName(killer), GetUnitName(victim));
	killCount.Add(GetOwningPlayer(killer), 1)
});
```
