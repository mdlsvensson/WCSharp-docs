# Homing Missile

HomingMissile adds the following properties:

| Name | Description |
|---|---|
| TurnRate | The rate at which the missile can turn in degrees per second. |
| InitialAngle | The initial angle in degrees. If left at null, will default to the angle towards the target. |

The homing missile is similar to the [momentum missile](momentum-missile.md), but instead maintains a constant speed and constantly adjusts its angle such that it attempts to fly towards the target in a straight line. It is limited by this according to its `TurnRate`.
Note that homing missiles can end up infinitely circling their target, as they do not try to account for the turning circle towards the target when adjusting their angle. For that reason, I recommend creating homing missiles with properties such as a large collision radius, a timed explosion or an increased `ImpactLeeway`.

A basic example of a homing projectile that will home towards the target and explode upon colliding with the first enemy unit in a 250 radius:

```csharp
using WCSharp.Missiles;
using static War3Api.Common;

public class Homing : HomingMissile
{
	public Homing(unit caster, unit target) : base(caster, target)
	{
		CasterZ = 50;
		TargetImpactZ = 50;
		Speed = 800;
		TurnRate = 90;
		Roll = 90;
		EffectScale = 1.0f;
		EffectString = @"Abilities\Weapons\GlaiveMissile\GlaiveMissile.mdl";
		CollisionRadius = 250;
	}

	public override void OnCollision(unit unit)
	{
		if (IsUnitEnemy(unit, CastingPlayer))
		{
			UnitDamageTarget(Caster, Target, 100, true, false, ATTACK_TYPE_CHAOS, DAMAGE_TYPE_UNKNOWN, WEAPON_TYPE_WHOKNOWS);
			Active = false;
		}
	}
}
```
