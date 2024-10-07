PassiveBuff is the simplest buff. It is intended as a simple apply-expire buff that does nothing over its duration, and as such comes with no added baggage.

The below example is a simple aura that increases the targets armour by 5 when applied, and decreases it by 5 when it is disposed.

```csharp
using WCSharp.Buffs;
using static War3Api.Common;

public class MyPassiveBuff : PassiveBuff
{
	public MyBuff(unit caster, unit target) : base(caster, target)
	{
		Duration = 10.0f;
		EffectString = @"Abilities\Spells\Human\DevotionAura\DevotionAura.mdl";
	}

	public override void OnApply()
	{
		BlzSetUnitArmor(Target, BlzGetUnitArmor(Target) + 5);
	}

	public override void OnDispose()
	{
		if (UnitAlive(Target))
		{
			BlzSetUnitArmor(Target, BlzGetUnitArmor(Target) - 5);		
		}
	}
}
```
