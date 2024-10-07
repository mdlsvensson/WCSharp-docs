# Buff Dispelling

The buff system can handle dispelling of buffs on a target with matching on the `IsBeneficial` and `BuffTypes` properties. Buff types is a list of strings, allowing you to define buffs with multiple types that will be matched on via the various different `BuffSystem.Dispel` options. Additionally, the `OnDispel` handler can be used to create more complex dispel interactions, such as dispel resistance or special actions. All dispelled buffs are also returned, allowing for the dispel to interact with the results as well. To showcase the possibilities, a number of examples are outlined below.

For a buff to be fully dispelled, it should set its `Stacks` property to 0, and then the system will automatically dispose the buff afterwards. For reference, below is the base implementation of `OnDispel`:

```csharp
public int OnDispel(unit dispeller, int dispelCharges)
{
	// determine how many stacks can be dispelled
	var stacksToDispel = Math.Min(Stacks, dispelCharges);
	// reduce stacks
	Stacks -= stacksToDispel;
	// return number of dispel charges used
	return stacksToDispel;
}
```

The below example will dispel up to 3 buffs, and heal the dispelled unit for 100 for each dispelled buff stack:

```csharp
var life = GetUnitState(target, UNIT_STATE_LIFE);
foreach (var dispel in BuffSystem.Dispel(target, caster, false, 3, "Magic"))
{
	life += dispel.BuffStacksDispelled * 100;
}
SetUnitState(target, UNIT_STATE_LIFE, life);
```

The below buff has a 25% chance to resist dispels:

```csharp
public override int OnDispel(unit dispeller, int dispelCharges)
{
	for (var i = 0; i < dispelCharges; i++)
	{
		if (GetRandomInt(1, 4) != 1)
		{
			Stacks--;

			if (Stacks == 0)
			{
				// no stacks left, return number of loops that were needed
				return i;
			}
		}
	}
	
	// All dispel charges were consumed
	return dispelCharges;
}
```

The below buff will deal 1000 damage to the target when it is dispelled:

```csharp
public override int OnDispel(unit dispeller, int dispelCharges)
{
	UnitDamageTarget(Caster, Target, 1000, true, false, ATTACK_TYPE_CHAOS, DAMAGE_TYPE_UNKNOWN, WEAPON_TYPE_WHOKNOWS);
	
	// Base method is fine for updating stacks/calculating dispel charges consumed
	return base.OnDispel();
}
```
