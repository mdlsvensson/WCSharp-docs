# CurveMissile

The CurveMissile expands on the [BasicMissile](basic-missile.md) by providing an additional Curve property. This property adds an additional rotation to the arc of the projectile, allowing it to perform significantly more complex 3D trajectories. For example, setting the curve to `90` or `-90` will make it act like a boomerang, approaching the target from a horizontal arc instead of a vertical arc.

There are two special methods, `DisableArc` and `ReactiveArc`, which control the arcing behaviour (assuming the projectile has an arc that is not 0). If major changes in the targets position are encountered, the projectile can decide to disable its arc. If desired, you can re-enable it, although it may look strange if you do not handle it properly. In particular, this is useful when redirecting a missile to another target.

Below is a slightly advanced example to illustrate the complexity of missile patterns that you can create with this. This example will create 5 missiles that spread out behind the caster in a semi-circle. After 0.3 seconds, they will all change their direction to head straight to the target over 0.5s. This mimics [Li-Ming's Magic Missile](https://www.youtube.com/watch?v=6SqmKqFUwnM&t=1m4s) ability in Heroes of the Storm.

```csharp
using WCSharp.Missiles;
using WCSharp.Util;
using static War3Api.Common;

public class Curving : CurveMissile
{
	public static void Initialise()
	{
		PlayerUnitEvents.Register(PlayerUnitEvent.SpellEffect, LaunchMissile, Constants.ABILITY_CURVING_MISSILE);
	}

	private static void LaunchMissile()
	{
		for (var i = 0; i < 5; i++)
		{
			var missile = new Curving(GetTriggerUnit(), GetSpellTargetX(), GetSpellTargetY())
			{
				Curve = -90 + (45 * i)
			};
			MissileSystem.Add(missile);
		}
	}

	public Curving(unit caster, float targetX, float targetY) : base(caster, targetX, targetY)
	{
		CasterZ = 50;
		TargetImpactZ = 50;
		Speed = -400;
		Arc = 1.0f;
		Interval = 0.3f;
		EffectString = @"Abilities\Weapons\Dryadmissile\Dryadmissile.mdl";
	}

	public override void OnPeriodic()
	{
		DisableArc();
		Speed = Util.DistanceBetweenPoints(MissileX, MissileY, TargetX, TargetY) * 2;
		Interval = 0;
	}
}
```
