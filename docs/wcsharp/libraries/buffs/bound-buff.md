BoundBuff adds the following properties:

| Name | Description |
|---|---|
| IntervalLeft | The time, in seconds, remaining until the next tick. |
| Interval | The time, in seconds, between each tick. |

These properties are used to invoke the custom `OnTick` handler every interval.
If the target dies from actions performed in `OnTick`, `OnDeath` will automatically be called with `killingBlow = true`.

Additionally, BoundBuff defines a `Bind` method that can be used to bind the buff to an in-game buff, so that players can see the tooltip or the flashing when the buff is about to expire. Binding can be performed in two ways.

The first binding method is by providing it with an ability that applies a buff which it should cast. The system then retrieves a dummy using [WCSharp.Dummies](../dummies.md) and orders it to cast that ability on the target. For example: `Bind(ABILITY_MARK, BUFF_MARK, ORDER_FAERIE_FIRE);`. Optional ability level and dummy owner arguments can be supplied to this method.
The advantage of this method is that the in-game buff will have a duration, meaning it will flash when it is about to expire. However, dummies can sometimes be uncooperative if cast by CPU players. Of course, another downside is that this method requires dummies, but most of the disadvantages of dummies are negated via [WCSharp.Dummies](../dummies.md).

The second binding method is by providing it with an aura that it should give to the target. The system will then automatically hide the aura and remove both the aura ability and its in-game buff when the buff is removed (meaning there is no aura stickiness). For example: `Bind(ABILITY_MARK_AURA, BUFF_MARK);`. Optional ability level can be supplied to this method.
The advantage of this method is that no dummies are required, and the method is very efficient if the buff can be applied frequently, since the aura just needs to be added once. The downside is that auras do not have a duration, meaning the buff tooltip will not flash when it is about to expire.

A basic example of a BoundBuff using the aura binding, that lasts 10 seconds and ticks each second for decreasing amounts (starting at 100):

```csharp
using WCSharp.Buffs;
using static War3Api.Common;

public class Burn : BoundBuff
{
	public BoundBuffTest(unit caster, unit target) : base(caster, target)
	{
		Interval = 1.0f;
		Duration = 10.0f;
		EffectString = @"Abilities\Spells\Human\FlameStrike\FlameStrikeDamageTarget.mdl";
		Bind(Constants.ABILITY_BURN_AURA, Constants.BUFF_BURN);
	}

	public override void OnTick()
	{
		UnitDamageTarget(Caster, Target, 10 + (Duration * 10), true, false, ATTACK_TYPE_CHAOS, DAMAGE_TYPE_UNKNOWN, WEAPON_TYPE_WHOKNOWS);
	}
}
```
