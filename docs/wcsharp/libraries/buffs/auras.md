# Auras

The aura system offers the following features:

* Automatically manages application and refreshing of buffs in a customisable manner
* Integrates with the buff system to allow easily customisable buffs to be applied
* Auras are bound to a specific buff to make it easier to work with

## Usage

Creating an aura requires 2 classes to be created. The first thing we need is the buff to be applied. For this, you should use AuraBoundBuff, which is a small extension of [BoundBuff](bound-buff.md) that will help ensure that the auras will stack in a manner similar to default auras. Furthermore, it will ensure that the `Stacks` property is set to the number of aura bearers applying the buff.

```csharp
using WCSharp.Buffs;
using static War3Api.Common;

public class MyBuff : AuraBoundBuff
{
	public MyBuff(unit caster, unit target) : base(caster, target)
	{
		EffectString = @"Abilities\Spells\Other\GeneralAuraTarget\GeneralAuraTarget.mdl";
		Bind(Constants.ABILITY_DEVOTION_AURA, Constants.BUFF_DEVOTION_AURA);
	}

	public override void OnApply()
	{
		BlzSetUnitArmor(Target, BlzGetUnitArmor(Target) + 5);
	}

	public override void OnExpire()
	{
		BlzSetUnitArmor(Target, BlzGetUnitArmor(Target) - 5);
	}
}
```

We don't need to set a duration here, this will be managed by the aura. For the aura itself, we'll inform C# that the aura should bind to `MyBuff` by extending it with `Aura<MyBuff>`. Then we can implement the abstract class, and create something like this:

```csharp
public class MyAura : Aura<MyBuff>
{
	public MyAura(unit caster) : base(caster)
	{
		Radius = 800;
		EffectString = @"Abilities\Spells\Human\DevotionAura\DevotionAura.mdl";
		StackBehaviour = StackBehaviour.Stack;
	}

	// Use this to create new instances of MyBuff and perform additional initialisation if necessary
	protected override MyBuff CreateAuraBuff(unit unit)
	{
		return new MyBuff(Caster, unit);
	}

	// Use this to filter on units that should be buffed by this aura
	protected override bool UnitFilter(unit unit)
	{
		return IsUnitAlly(unit, CastingPlayer);
	}
}
```

And that's it! We now have our own Devotion Aura, including having it show a buff on the unit. This particular implementation won't show a "+5" on the unit stats, but you can easily achieve this by increasing the armour via a different method than `BlzSetUnitArmor`, such as giving the unit an item ability that increases armour.

Finally, to add this aura to a unit, simply create a new instance and add it to the system:

```csharp
var unit = GetTriggerUnit();
var aura = new MyAura(unit);
AuraSystem.Add(aura);
```

## Properties

| Name | Description |
|---|---|
| Active | Inherited from IPeriodicDisposableAction. Set to false to disable and dispose. |
| Caster | The owner of the aura. |
| CastingPlayer | The player who owns the aura. This is set on creation, if you want it to automatically update, use `AuraSystem.RegisterForOwnershipChanges`. |
| Radius | The range within which units must be for the aura to be applied to them. |
| Duration | The duration in seconds of buffs applied by this aura. Defaults to 3.1. Unless you're making a pulsing aura, you will want the Duration to be greater than the SearchInterval. |
| SearchIntervalLeft | How long in seconds until this aura will next be applied to valid units in range. |
| SearchInterval | How long in seconds between applications of this aura. Defaults to 1.0. |
| StackBehaviour | The stack behaviour of buffs applied by this aura. Note that even with StackBehaviour.None, auras will still only stack once per aura instance. |
| EffectString | The path of the effect to use. Leave empty for no effect. If changed while the aura is already active, will destroy and recreate the effect. |
| EffectAttachmentPoint | The attachment point for the effect. If changed while the aura is already active, will destroy and recreate the effect at the desired attachment point. Defaults to origin. |
| EffectScale | The effect scale for the effect. If changed while the aura is already active, automatically modifies the effect. |
| Effect | The effect used by the aura. Creation of the effect should be done by setting EffectString, not by setting this property. |
| ActiveBuffsByUnit | A dictionary mapping units to active buffs. |
