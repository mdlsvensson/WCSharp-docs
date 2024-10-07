AutoBuff adds the following properties:

| Name | Description |
|---|---|
| IntervalLeft | The time, in seconds, remaining until the next tick. |
| Interval | The time, in seconds, between each tick. |
| DamagePerInterval | The damage to apply on each tick. Set to negative to heal. The damage dealer is the caster if the caster is alive, otherwise the target itself. |
| AttackType | The attack type to use when dealing damage. |
| DamageType | The damage type to use when dealing damage. |

These properties are used to invoke the custom `OnTick` handler every interval, as well as perform the desired amount of damage/healing. Note that `OnTick` triggers before the automatic damage/healing is performed, so you can adjust the amount per tick in `OnTick` if desired.

If the target dies either from actions performed in `OnTick` or by the automatic damage/healing step, `OnDeath` will automatically be called with `killingBlow = true`.

A basic example of an AutoBuff that lasts 10 seconds and ticks each second for increasing amounts (starting at 100):

```csharp
using WCSharp.Buffs;
using static War3Api.Common;

public class MyAutoBuff : AutoBuff
{
	public MyAutoBuff(unit caster, unit target) : base(caster, target)
	{
		Interval = 1.0f;
		Duration = 10.0f;
		EffectString = @"Abilities\Spells\Human\FlameStrike\FlameStrikeDamageTarget.mdl";
		DamagePerInterval = 90;
		AttackType = ATTACK_TYPE_CHAOS;
	}

	public override void OnTick()
	{
		DamagePerInterval += 10;
	}
}
```
