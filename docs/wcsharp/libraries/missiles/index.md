# WCSharp.Missiles

The missile system offers the following features:

* Automatically manages any number of missiles on the whole map
* Offers various different types of missiles, such as arcing, curved, orbital or momentum based missiles
* Automatically handles things such as the target dying or hitting the edge of the map
* Can be extended with custom code on events such as impact or collision to perform any desired behaviour

## Usage

The missile system is implemented in a manner similar to the buff system. To create any new missile, you will first want to create a new class, and implement one of the missile subtypes. For example, BasicMissile:

```csharp
public class TestMissile : BasicMissile
{

}
```

Next, use the ALT+Enter combination while selecting the class name to create one of the required constructors. The one that you'll want to use will likely change depending on the type of missile that you're firing. Doing so may give you something like this:

```csharp
public class TestMissile : BasicMissile
{
	public TestMissile(unit caster, unit target) : base(caster, target)
	{
	}
}
```

Next we can fill some stuff in to create a functional missile:

```csharp
public class TestMissile : BasicMissile
{
	public TestMissile(unit caster, unit target) : base(caster, target)
	{
		Speed = 600;
		Arc = 0.3f;
		EffectString = @"Abilities\Weapons\FireBallMissile\FireBallMissile.mdl";
		EffectScale = 1.0f;
	}

	public override void OnImpact()
	{
		// Deal damage to the target!
	}
}
```

Finally, to launch this missile, all we need to do is this:

```csharp
var caster = GetTriggerUnit();
var target = GetSpellTargetUnit();
var missile = new TestMissile(caster, target)
{
	CasterHeightOffset = 50.0f
	TargetHeightOffset = 50.0f
}
MissileSystem.Add(missile);
```

Properties can be set in either the constructor or via the public properties. I personally recommend using the constructor, as that'll make it easier to re-use the missile in other segments of the code without having to re-do those assignments.
Note that, unless Caster/TargetHeightOffset are defined, the projectile will originate from/aim at the ground, which will likely look weird if fired by or directed at a unit.

As the missile is its own class, it is easily extendable and usable in various ways.  For example, instead of always having the effect scale be the same, it can be based on the level of the ability.

## Type overview

There are 5 missile types defined that each behave in a different manner. You can use any given type based on the trajectory that you want your missile to fly. Currently, the following types of missiles exist:

* [BasicMissile](basic-missile.md) - This is the most basic missile type. A simple location/unit A to location/unit B with an optional arc.
* [CurveMissile](curve-missile.md)- In this class you can combine the arc with a curve. What this means is that you can make the arc go e.g. sideways or diagonal. For example, setting Arc = 0.3f and Curve = 90 will give you a boomerang! Note that if you do not use the Curve property, you should use BasicMissile instead, as it offers the same functionality at much greater performance.
* [HomingMissile](homing-missile.md) - This is a missile that homes in on targets by travelling at a fixed speed and turning at a fixed rate. This essentially gives you a missile that you can side step to dodge, but will attempt to correct its course to ensure it hits. If it misses, it will turn around and attempt again (note that it can get stuck going in circles).
* [MomentumMissile](momentum-missile.md) - This is a missile that homes in on targets by accelerating towards the target at a fixed acceleration speed. It will attempt to make intelligent adjustments to its course by essentially "overcompensating" in the direction that the target is, compared to the direction that it is currently flying at. If it misses, it will decelerate and go in the opposite direction.
* [OrbitalMissile](orbital-missile.md) - This is a missile that will orbit the given target unit or location. The speed can be set either in units per second, or orbital period.

## Events

| Name | Description |
|---|---|
| OnImpact | Override this method if your missile has an impact effect. The Active property is automatically set to false prior to calling this method. If you do not want the missile to end, you need to set Active back to true. |
| OnPeriodic | Override this method if your missile has a periodic effect. For this to be active, the Interval property must be greater than 0. |
| OnCollision | Override this method if your missile has an effect that should trigger when colliding with another unit. For this to be active, CollisionRadius must be greater than 0. Note that there is no filter on this collision. This is called whenever it collides with anything not in the TargetsHit property. Before this method is called, the unit is added to TargetsHit. |
| OnDispose | Override this method if your missile has an effect that should trigger when it is destroyed for any reason. |

## Properties

Below is an overview of all the properties available on the various missile types. The "required" essentially details which one of these you should set on launch. It's technically never required to do so, as the system doesn't mind if a missile has e.g. 0 speed (or even negative), but this is just for easy look up to ensure that you set a new missile up properly, and remembering what important properties you might have forgotten.

| Name | Used by | Required | Description |
|---|---|---|---|
| Active | All | No | Inherited from IPeriodicDisposableAction. Set to false to disable and dispose. |
| Speed | All | Yes | The speed at which the missile will travel, expressed in units per second. |
| SpeedPerTick | All | Yes | The speed at which the missile will travel, expressed in units per tick. |
| EffectString | All | Yes | The effect string to use for the effect. Can be altered mid-flight and will be updated accordingly. |
| Curve | CurveMissile | Yes | Dictates the curve at which the arc will occur. Technically not required, but if unused, you should use BasicMissile instead for greater performance. |
| TurnRate | HomingMissile | Yes | Dictates the rate at which the missile can turn itself, expressed in degrees per second. Defaults to 0. |
| TurnPeriod | HomingMissile | Yes | Dictates the rate at which the missile can turn itself, expressed in seconds per rotation. Defaults to 0. |
| TurnVelocityRad | HomingMissile | Yes | Dictates the rate at which the missile can turn itself, expressed in degrees per second. Defaults to 0. |
| Acceleration | MomentumMissile | Yes | The acceleration speed of the MomentumMissile, in units per second. |
| Acceleration | MaximumSpeed | Yes | The maximum speed of the MomentumMissile, in units per second. Note that you cannot set the speed to higher than the maximum speed. |
| Range | OrbitalMissile | Yes | The range at which the missile will orbit from the target in units. |
| OrbitalPeriod | OrbitalMissile | Yes | Defines the orbit period in seconds. |
| OrbitalVelocity | OrbitalMissile | Yes | Defines the orbit period in radians per tick. |
| Mode | All | No | The current flight mode of the missile. Each missile type has different flight modes, typically for handling terrain height. In most cases, the default will be preferred. |
| EffectScale | All | No | The scale of the effect. Can be altered mid-flight and will be updated accordingly. Defaults to 1. |
| ImpactLeeway | All | No | Can be used to increase the distance from which the missile can impact the target. Defaults to 0. |
| Interval | All | No | Determines how frequently the missile will run the OnPeriodic method. Disabled if 0 (default). |
| CollisionSize | All | No | Will trigger OnCollision for any units that come within the given range of the missile. Disabled if 0 (default). |
| CasterLaunchZ | All | No | Functionally identical to "Art - Projectile Launch - Z". |
| TargetImpactZ | All | No | Functionally identical to "Art - Projectile Impact - Z". |
| Caster | All | No* | The unit that created this missile. |
| CastingPlayer | All | No* | The owner (of the unit) that created this missile. Does NOT automatically update if the caster changes owner. |
| CasterX | All | No* | The X coordinate from which this missile was launched. |
| CasterY | All | No* | The Y coordinate from which this missile was launched. |
| CasterZ | All | No* | The Z coordinate from which this missile was launched. |
| Target | All | No* | The unit that created this missile. |
| TargetPlayer | All | No* | The owner (of the unit) targeted by this missile. Does NOT automatically update if the target changes owner. |
| TargetX | All | No* | The X coordinate which this missile is targeting. Automatically updated while the target is alive. |
| TargetY | All | No* | The Y coordinate which this missile is targeting. Automatically updated while the target is alive. |
| TargetZ | All | No* | The Z coordinate which this missile is targeting. Automatically updated while the target is alive. |
| MissileX | All | No | The current X coordinate of the missile. |
| MissileY | All | No | The current Y coordinate of the missile. |
| MissileZ | All | No | The current Z coordinate of the missile. |
| Yaw | All | No | The current yaw of the missile in degrees. |
| Pitch | All | No | The current pitch of the missile in degrees. |
| Roll | All | No | The current roll of the missile in degrees. |
| YawRad | All | No | The current yaw of the missile in radians. |
| PitchRad | All | No | The current pitch of the missile in radians. |
| RollRad | All | No | The current roll of the missile in radians. |
| CurrentAngle | All | No | The current angle of the missile in degrees, identical to yaw, just in layman's terms. |
| SpinPeriod | All | No | The amount of seconds it takes for the missile to spin once. Defaults to 0 (default). |
| SpinVelocityRad | All | No | The velocity of the spin in radians per tick. |
| InitialAngle | HomingMissile | No | The initial angle to use for the HomingMissile. Can be left at null to use the angle to the target. |
| InitialAngle | MomentumMissile | No | The initial angle to use for the MomentumMissile. Can be left at null to use the angle to the target. |
| IsArcing | BasicMissile | No | Whether the missile is currently performing an arcing motion. Can be used to re-enable the behavior if it was disabled due to rapid target movement. |
| IsArcingOrCurving | CurveMissile | No | Whether the missile is currently performing an arcing or curving motion. Can be used to re-enable the behavior if it was disabled due to rapid target movement. |

\*Technically required, but will be specified via the constructor.
