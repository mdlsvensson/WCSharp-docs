OrbitalMissile adds the following properties:

| Name | Description |
|---|---|
| Speed | This property behaves differently for orbital missiles. It defines a speed in units per second (negative to go clockwise). If you want to define speed in how long it takes to orbit once, use `OrbitalPeriod` instead. |
| Range | The distance at which the OrbitalMissile orbits from the origin. The speed of the missile after a range adjustment is determined by whether it was set via Speed or OrbitalPeriod. |
| OrbitalPeriod | The amount of time it takes to make one rotation in seconds. Use negative values to go clockwise. This can be used instead ofSpeed, and will ensure a consistent orbital period through Range adjustments. |
| InitialAngle | The initial angle in degrees at which this missile is created. If left at null, will use a random angle. |

The orbital missile will orbit the target at the desired range. Unlike other missiles, it cannot impact the target.

Below is a slightly more advanced example of an orbital missile. This missile completes an orbit once every 4 seconds at a range of 600, and collides with enemy units in a 150 radius. Targets hit are stored in a list and removed from TargetsHit after 1 second, allowing them to be collided with again via `OnCollision`.

```csharp
using System.Collections.Generic;
using WCSharp.Missiles;
using WCSharp.Utils.Extensions;
using static War3Api.Common;

public class Orbital : OrbitalMissile
{
	private class UnitHit
	{
		public float Age { get; set; }
		public unit Unit { get; }

		public UnitHit(unit unit)
		{
			Unit = unit;
		}
	}

	private readonly List<UnitHit> targetsHitCooldown = new List<UnitHit>();

	public Orbital(unit caster, unit target) : base(caster, target)
	{
		TargetImpactZ = 50;
		EffectScale = 1.0f;
		EffectString = @"Abilities\Weapons\GlaiveMissile\GlaiveMissile.mdl";
		CollisionRadius = 150;
		Range = 600;
		OrbitalPeriod = 4.0f;
		Interval = PeriodicEvents.SYSTEM_INTERVAL;
	}

	public override void OnCollision(unit unit)
	{
		if (IsUnitEnemy(unit, CastingPlayer))
		{
			UnitDamageTarget(Caster, Target, 100, true, false, ATTACK_TYPE_CHAOS, DAMAGE_TYPE_UNKNOWN, WEAPON_TYPE_WHOKNOWS);
			this.targetsHitCooldown.Add(new UnitHit(unit));
		}
	}

	public override void OnPeriodic()
	{
		this.targetsHitCooldown.IterateWithRemoval(cooldown =>
		{

			cooldown.Age += PeriodicEvents.SYSTEM_INTERVAL;
			if (cooldown.Age >= 1)
			{
				TargetsHit.Remove(cooldown.Unit);
				return false;
			}

			return true;
		});
	}
}
```

Please see [Utils](../shared.md) for an explanation regarding the IterateWithRemoval method.
