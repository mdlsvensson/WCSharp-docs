# Basic Missile

The BasicMissile only implements the most basic behaviour for a missile and is generally very lightweight. The only unique property is `Arc`, which mirrors the in-game projectile arc.

There are two special methods, `DisableArc` and `ReactiveArc`, which control the arcing behaviour (assuming the projectile has an arc that is not 0). If major changes in the targets position are encountered, the projectile can decide to disable its arc. If desired, you can re-enable it, although it may look strange if you do not handle it properly. In particular, this is useful when redirecting a missile to another target.

A basic example of a missile that deals 100 damage to its target upon impact:

```csharp
using WCSharp.Missiles;
using static War3Api.Common;

public class Arcing : BasicMissile
{
	public Arcing(unit caster, float targetX, float targetY) : base(caster, targetX, targetY)
	{
		CasterZ = 50;
		TargetImpactZ = 50;
		Speed = 600;
		Arc = 0.3f;
		EffectScale = 3.0f;
		EffectString = @"Abilities\Spells\Undead\DeathCoil\DeathCoilMissile.mdl";
	}

	public override void OnImpact()
	{
		UnitDamageTarget(Caster, Target, 100, true, false, ATTACK_TYPE_CHAOS, DAMAGE_TYPE_UNKNOWN, WEAPON_TYPE_WHOKNOWS);
	}
}
```
