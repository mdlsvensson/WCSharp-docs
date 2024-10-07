TickingBuff adds the following properties:

| Name | Description |
|---|---|
| IntervalLeft | The time, in seconds, remaining until the next tick. |
| Interval | The time, in seconds, between each tick. |

These properties are used to invoke the custom `OnTick` handler every interval.
If the target dies from actions performed in `OnTick`, `OnDeath` will automatically be called with `killingBlow = true`.

A basic example of a TickingBuff that lasts 10 seconds and ticks each second for decreasing amounts (starting at 100):

```csharp
using WCSharp.Buffs;
using static War3Api.Common;

public class MyTickingBuff : TickingBuff
{
	public MyTickingBuff(unit caster, unit target) : base(caster, target)
	{
		Interval = 1.0f;
		Duration = 10.0f;
		EffectString = @"Abilities\Spells\Human\FlameStrike\FlameStrikeDamageTarget.mdl";
	}

	public override void OnTick()
	{
		UnitDamageTarget(Caster, Target, 10 + (Duration * 10), true, false, ATTACK_TYPE_CHAOS, DAMAGE_TYPE_UNKNOWN, WEAPON_TYPE_WHOKNOWS);
	}
}
```

Note that [AutoBuff](auto-buff.md) will often be easier if you're only interested in performing damage or healing on the target.
