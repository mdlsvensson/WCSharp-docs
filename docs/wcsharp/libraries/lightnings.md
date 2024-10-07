# WCSharp.Lightings

**For basic information on lightning effects, please consult [this thread](https://www.hiveworkshop.com/threads/beginners-guide-to-lightning-effects.220370/) on Hiveworkshop.**

The lightning system offers the following features:

* Customise various properties of each lightning such as the duration, fade behaviour and colour
* Automatically manages any number of lightning effects
* Automatically follows moving units, if supplied
* Supports both lightning effects that last a pre-determined duration, or until cancelled.

## Usage

The lightning system is similar in use to the Knockback system, in that you simply need to create a new Lightning object, and then add it to the system using `LightningSystem.Add(<lightning>)`.

The lightning system however, features a lot more customisable properties, many of which you'll want to set before adding it to the system. For example, a typical use of the lightning system will look like this:

```csharp
var lightning = new Lightning("AFOD", caster, target)
{
	Duration = 1.0f,
	FadeDuration = 0.5f,
	CasterHeightOffset = 50f,
	TargetHeightOffset = 50f,
};
LightningSystem.Add(lightning);
```

Note that, if Caster/TargetHeightOffset are not specified, the lightning will be directly on the ground, which will often look strange.

## Properties

| Name | Description |
|---|---|
| Active | Inherited from IPeriodicDisposableAction. Set to false to disable and dispose. |
| Caster | The caster of the lightning. Setting this means that the lightning will follow the caster when the caster moves. |
| CasterX | The X coordinate from which this lightning was fired. While Caster is alive, this will automatically be updated. |
| CasterY | The Y coordinate from which this lightning was fired. While Caster is alive, this will automatically be updated. |
| CasterHeightOffset | The height that this lightning should originate from. By default, this is the ground. |
| Target | The target of the lightning. Setting this means that the lightning will follow the target when the target moves. |
| TargetX | The X coordinate that this lightning is targeting. While Target is alive, this will automatically be updated. |
| TargetY | The Y coordinate that this lightning is targeting. While Target is alive, this will automatically be updated. |
| TargetHeightOffset | The height that this lightning should aim at. By default, this is the ground. |
| Red | The red color of this lightning. Setting this to 0 means all red will be removed from the lightning. |
| Green | The green color of this lightning. Setting this to 0 means all green will be removed from the lightning. |
| Blue | The blue color of this lightning. Setting this to 0 means all blue will be removed from the lightning. |
| Alpha | The alpha (transparency) of this lightning. Setting this to 0 means the lightning is invisible. |
| Duration | The duration that this lightning should last in total. |
| FadeDuration | The length of time over which the lightning will fade. |
