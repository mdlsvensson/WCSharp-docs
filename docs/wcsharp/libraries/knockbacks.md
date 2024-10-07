# WCSharp.Knockbacks

The knockback system provides the following features:

* Supports any number of knockbacks, potentially multiple on the same unit
* Customise the distance and duration of knockbacks
* Via "reversing the arguments", can also be used to pull enemies towards a location

## Usage

Performing a knockback on a unit is simple, all you need to do is create a new Knockback object using one of the two constructors, which specify either an angle or a position to fly towards, and add it to the system using `KnockbackSystem.Add(<knockback>);`. Each constructor also allows you to set the distance and duration of the knockback.

To calculate the angle of the knockback, you can use the `AngleBetweenPoints` methods located in `WCSharp.Utils`.

## Properties

| Name | Description |
|---|---|
| Active | Inherited from IPeriodicAction. Set to false to disable and remove. |
| Target | The target of the knockback. |
| Angle | The angle of the knockback in degrees. |
| Speed | The distance traversed per tick (0.03125). |
| SpeedDropoff | The amount of speed lost per tick (0.03125). |
| Effect1 | This effect will be spawned every 1.0 seconds on the target using Effect1AttachmentPoint. |
| Effect2 | This effect will be spawned every 0.125 seconds on the target using Effect2AttachmentPoint. |
| Effect1AttachmentPoint | The attachment point to use for Effect1. |
| Effect2AttachmentPoint | The attachment point to use for Effect2. |
