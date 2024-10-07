# Buffs

The buff system offers the following features:

* Customise various properties of each buff such as duration, effect and tick interval.
* Automatically manages buffs on any number of targets.
* Customisable behaviour for stacking of buffs
* Supports various events such as OnTick or OnExpire.
* Supports dispelling based on custom buff types.
* Optionally handles periodic damage/healing ticks.
* Optionally handles binding to in-game buffs (e.g. for tooltips).

# System

The buff system has several features for easily interacting with buffs contained in the system:

* When adding buffs to the system, the `Add` method can be supplied with additional information to handle stacking as desired for that specific buff. For more information, see [buff stacking](buff-stacking.md).
* All buffs that are on a specific unit can be efficiently retrieved using the `GetBuffsOnUnit` method.
* The `RegisterForOwnershipChanges` method can be used to register for events to automatically update `CastingPlayer` and `TargetPlayer` when a unit switches ownership.
* The `Dispel` method can be used in conjunction with properties on buffs to create complex dispel behaviours. For more information, see [buff dispelling](buff-dispelling.md).

# Type overview

There are 5 base buffs defined that behave differently. These implement some more advanced behaviour, so you can use one of the complexity that meets your requirements. These are as follows:

* [PassiveBuff](passive-buff.md): Basic buff with no special features.
* [TickingBuff](ticking-buff.md): This buff adds an interval and event for handling tick behaviour.
* [AutoBuff](auto-buff.md): In addition to ticks, this buff can also automatically handle damage/healing. This buff corresponds to the buffs in version 1 of the buff system.
* [RollingBuff](rolling-buff.md): This is a specialised buff to manage multiple sub-buffs (stacks), each with their own duration, while only the "top-level" buff ticks.
* [BoundBuff](bound-buff.md): This buff implements special logic to handle connecting it to an in-game buff, which can be applied either via [WCSharp.Dummies](../dummies.md) or via an in-game aura. Besides the binding mechanic, it is functionally identical to the [TickingBuff](ticking-buff.md).

# Events

| Name | Description |
|---|---|
| OnApply | Executes immediately upon application of the buff. |
| OnDeath | Executes immediately after Target dies. |
| OnDispel | Executes when an attempt is made to dispel the target. |
| OnDispose | Executes when the buff is removed for any reason whatsoever. |
| OnExpire | Executes when the buff expires by reaching the end of its duration. Does not trigger when the buff is removed via a dispel or target dies. |
| OnTick | Executes every given interval. Present on most, but not all, buffs. |

# Properties

| Name | Description |
|---|---|
| Active | Inherited from IPeriodicDisposableAction. Set to false to disable and dispose. |
| Caster | The unit that applied the buff. |
| CastingPlayer | The owner of the caster. Automatically set on application. Does NOT automatically update if the caster changes owner. |
| Target | The target to which this buff is attached. |
| TargetPlayer | The owner of the target. Automatically set on application. Does NOT automatically update if the target changes owner. |
| Duration | The remaining duration before this buff expires. |
| IsBeneficial | Whether this buff is beneficial or detrimental to the target. |
| BuffTypes | The buff types, used primarily for dispelling. e.g. magic, physical, undispellable, etc. |
| Stacks | The number of stacks of this buff currently active on the target. Defaults to 1. |
| EffectString | The path of the effect to use. Leave empty for no effect. If changed while the buff is already active, will destroy and recreate the effect. |
| EffectAttachmentPoint | The attachment point for the effect. If changed while the buff is already active, will destroy and recreate the effect at the desired attachment point. Defaults to origin. |
| EffectScale | The effect scale of the missile. If modified mid-flight, automatically modifies the missile. |
| Effect | The effect being used by the missile. Creation of the effect should be done by setting <see cref="EffectString"/>, not by setting this property. |
