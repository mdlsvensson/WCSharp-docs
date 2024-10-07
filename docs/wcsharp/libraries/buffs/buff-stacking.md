The buff system can be supplied with additional parameters so that it can handle stacking buffs as desired, including things such as a stack counter and per-unit or per-player level stacking.

This is achieved by supplying the `Add` method with a `StackBehaviour`. The following stack behaviours are defined:

| Name | Description |
|---|---|
| None | Will not stack in any way |
| Stack | Will stack with all instances of itself |
| StackCaster | Will stack with all instances of itself cast by the same unit |
| StackPlayer | Will stack with all instances of itself cast by the same player |

If the stack behaviour is anything except `None`, what will happen is that it will iterate over all buffs already on the target, and find buffs with the same type as the one being applied. When it finds one, it invokes the `OnStack` method of the buff currently applied to the target. The `OnStack` method can then be used to further customise what should happen on a stack attempt. For example, this is the default implementation:

```csharp
public override StackResult OnStack(Buff newStack)
{
	Stacks += newStack.Stacks;
	Duration = Math.Max(Duration, newStack.Duration);
	return StackResult.Stack;
}
```

This simply increases the `Stacks` property by the number of stacks on the new buff, refreshes the duration, and confirms that the buffs were stacked, at which point the new buff is discarded. The following stack results are defined:

| Name | Description |
|---|---|
| Reject | The stack attempt is rejected, and the buffs will be applied separately |
| Stack | The stack attempt is successful, and the new buff is discarded |
| Consume | The stack attempt is successful, and the old buff is discarded (will be automatically disposed) |

Using these components, any stack behaviour can be achieved, some examples can be found below.

The following will create a separate stack of the buff for each unit, with a maximum of 5 stacks:

```csharp
BuffSystem.Add(buff, StackBehaviour.StackCaster)
...
public override StackResult OnStack(Buff newStack)
{
	Stacks = Math.Min(5, Stacks + newStack.Stacks);
	Duration = Math.Max(Duration, newStack.Duration);
	return StackResult.Stack;
}
```

The following will damage the target for 10 times the duration left on the previous buff, then apply the new buff:

```csharp
BuffSystem.Add(buff, StackBehaviour.Stack)
...
public override StackResult OnStack(Buff newStack)
{
	UnitDamageTarget(Caster, Target, Duration * 10, true, false, ATTACK_TYPE_CHAOS, DAMAGE_TYPE_UNKNOWN, WEAPON_TYPE_WHOKNOWS);
	return StackResult.Consume;
}
```

The following create a separate stack of the buff for each player and deal 1000 damage to the target upon reaching 10 stacks. It then disposes (removes) itself.

```csharp
BuffSystem.Add(buff, StackBehaviour.StackPlayer)
...
public override StackResult OnStack(Buff newStack)
{
	Stacks = Math.Min(10, Stacks + newStack.Stacks);
	Duration = Math.Max(Duration, newStack.Duration);
	if (Stacks == 10)
	{
		UnitDamageTarget(Caster, Target, 1000, true, false, ATTACK_TYPE_CHAOS, DAMAGE_TYPE_UNKNOWN, WEAPON_TYPE_WHOKNOWS);
		Dispose();
	}
	return StackResult.Stack;
}
```
