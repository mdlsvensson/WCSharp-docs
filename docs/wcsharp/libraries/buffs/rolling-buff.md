RollingBuff adds the following properties:

| Name | Description |
|---|---|
| IntervalLeft | The time, in seconds, remaining until the next tick. |
| Interval | The time, in seconds, between each tick. |
| IsMainStack | Whether the current instance is the main stack (i.e. the instance that manages underlying instances). |
| Buffs | All buffs underlying the main buff instance. |

These properties are used to invoke the custom `OnTick` handler every interval.
If the target dies from actions performed in `OnTick`, `OnDeath` will automatically be called with `killingBlow = true`.

RollingBuff is a specialised buff that will manage multiple underlying buffs, but behave as a single buff. Every time a buff is added via a call to `OnStack`, it increases the `Stack` property, adds the buff to a list, and manages their duration and expiration individually (the stacked buff is not added to the buff system). When one of the underlying stacks expire, the `OnExpireStack` handler is called and `Stacks` is decreased.

The `OnTick` method for RollingBuff is only called for the main buff, not the underlying buffs.
The `OnStack` method should always invoke `base.OnStack(newStack)` when you override it.

A basic example of a TickingBuff that lasts 10 seconds and ticks each second 10 times the number of stacks applied to the target:

```csharp
using WCSharp.Buffs;
using static War3Api.Common;

public class MyRollingBuff : RollingBuff<MyRollingBuff>
{
	public MyRollingBuff(unit caster, unit target) : base(caster, target)
	{
		Interval = 1.0f;
		Duration = 10.0f;
		EffectString = @"Abilities\Spells\Human\FlameStrike\FlameStrikeDamageTarget.mdl";
	}

	public override void OnTick()
	{
		UnitDamageTarget(Caster, Target, Stacks * 10, true, false, ATTACK_TYPE_CHAOS, DAMAGE_TYPE_UNKNOWN, WEAPON_TYPE_WHOKNOWS);
	}
}
```

This behaviour is subtly different from other approaches, for example using multiple non-stacking `TickingBuff` will deal damage multiple times instead of once. Alternatively, using a single stacking `TickingBuff` will deal damage once, but loses the individual durations of each stack.

Finally, although RollingBuff does not currently have built-in support for connecting it to an in-game buff, it is not difficult to implement this yourself by giving the target an aura in the `OnApply`, and removing it in the `OnDispose`.
