# MomentumMissile

MomentumMissile adds the following properties:

| Name | Description |
|---|---|
| InitialAngle | The initial angle in degrees. If left at null, will default to the angle towards the target. |
| Acceleration | The acceleration in units per second. |
| MaximumSpeed | The maximum speed in units per second. |

The momentum missile is similar to the [homing missile](homing-missile.md), but instead constantly attempts to accelerate towards the target. For example, if a unit blinks behind a momentum missile that is heading straight towards him, the missile will gradually slow down, and eventually turn to fly in the opposite direction. If the unit is fast enough/the missiles acceleration is slow enough, units can also sidestep the projectile, resulting in a similar outcome.
Internally, it attempts to overcompensate its acceleration angle slightly to better adjust for changes in position by the target. This means that it will typically not accelerate/decelerate at the exact value defined by `Acceleration`, but makes it more reliable in tracking its target.

The below example is a missile that will explode upon either impacting the target or after 5 seconds, dealing 100 damage to all nearby units.

```csharp
using WCSharp.Missiles;
using WCSharp.Util;
using static War3Api.Common;

public class Momentum : MomentumMissile
{
	public Momentum(unit caster, unit target) : base(caster, target)
	{
		CasterZ = 50;
		TargetImpactZ = 50;
		Speed = 100;
		MaximumSpeed = 1000;
		Acceleration = 25;
		InitialAngle = GetRandomReal(0, 360);
		Roll = 90;
		EffectString = @"Abilities\Weapons\GlaiveMissile\GlaiveMissile.mdl";
		Interval = 5.0f;
	}
	
	public override void OnImpact()
	{
		Explode();
	}

	public override void OnPeriodic()
	{
		Explode();
	}
	
	private void Explode()
	{
		var group = CreateGroup();
		GroupEnumUnitsInRange(group, MissileX, MissileY, 300, null);
		foreach (var unit in group.Enumerate())
		{
			UnitDamageTarget(Caster, unit, 100, true, false, ATTACK_TYPE_CHAOS, DAMAGE_TYPE_UNKNOWN, WEAPON_TYPE_WHOKNOWS);
		}
		DestroyGroup(group);
		Active = false;
	}
}
```
