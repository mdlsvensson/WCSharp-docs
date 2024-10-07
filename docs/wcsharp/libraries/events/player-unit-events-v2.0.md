---
status: deprecated
---

# PlayerUnitEvents v2.0

!!! warning
	This version of WCSharp.Events has been deprecated. For the v2.1+ version, see [PlayerUnitEvents v2.1](player-unit-events.md).

## Description

The PlayerUnitEvents system is a powerful system to replace the native `playerunitevent` events, with a focus on improving performance. It offers the following features:

* Automates creation of triggers and registration of events used. The user only needs to attach actions to events.
* Easily attach any number of listeners to events.
* Filter events with a dictionary on some function. For example, you can supply an integer to use with `UnitPlayerEvent.UnitTypeKills`, so that the code only triggers when that unit type kills something. Multiple event registrations to this will use a dictionary, so you can listen for dozens of different unit types on the same event without any change in performance.
* Automatically merges events that use the same native event for performance gains. One of the most expensive steps for events is transitioning from internal WarCraft 3 code into the actual user triggers, and this is avoided by only using a single event.
* Using some preprocessing steps, will only run what is actually in use.
* Provides numerous new events for ease of use. These events still rely on the natives behind the scenes, therefore not incurring additional performance costs. They only exist to simplify the logic.
* Do you want to filter events by something not provided by default? No problem! You can register your own custom event filters. Their performance is exactly the same as events that are provided by default.

## Usage

This system is used by registering/unregistering events and actions. This is done with the following methods:

* `PlayerUnitEvents.Register(PlayerUnitEvent, action)` - Will invoke the given action when the given event fires.
* `PlayerUnitEvents.Register(PlayerUnitEvent, action, filterTypeId)` - Will invoke the given action when the given event fires, filtered by the type id supplied. Explanations for what the type means in correlation to each event is typically intuitive to the name of the event, and further explanations are supplied by IntelliSense in Visual Studio.
* `PlayerUnitEvents.Register(identifier, action, filterTypeId)` - The same as above, but the identifier refers to a custom event that was added using `AddCustomEventFilter`.
* `PlayerUnitEvents.Unregister(PlayerUnitEvent, action)` - Removes a previously registered action for the given event if possible.
* `PlayerUnitEvents.Unregister(PlayerUnitEvent, filterTypeId)` - Removes a previously registered action for the given event/type filter if possible.
* `PlayerUnitEvents.Unregister(identifier, filterTypeId)` - The same as above, but the identifier refers to a custom event that was added using `AddCustomEventFilter`.

For those unfamiliar with C#, Actions can be either a method or a lambda function such as `() => { ... do some actions ... }`.

Custom events are created using `PlayerUnitEvents.AddCustomEventFilter`. For example, the custom event equivalent of `PlayerUnitEvent.UnitTypeKills` would be: `AddCustomEventFilter(EVENT_PLAYER_UNIT_DEATH, "SomeIdentifier", () => GetUnitTypeId(GetKillingUnit()))`. You can then use "SomeIdentifier" to register any number of events to this custom event. Absolute worth it if you want to register multiple events to the same type of filer! There is no performance difference between an event predefined in `PlayerUnitEvent` and one that is custom defined.

**IMPORTANT: Due to quirks of the C# to Lua conversion, you cannot refer directly to WC3 natives when adding custom event filters. They must be enclosed in either a method or a lambda.**

These **WILL** work:

```csharp
PlayerUnitEvents.AddCustomEventFilter(EVENT_PLAYER_UNIT_TRAIN_START, "UnitTypeStartsBeingTrained", () => GetTrainedUnitType());
PlayerUnitEvents.AddCustomEventFilter(EVENT_PLAYER_UNIT_TRAIN_START, "UnitTypeStartsBeingTrained", MyFilterMethod);

static int MyFilterMethod()
{
	return GetTrainedUnitType();
}
```

This **WILL NOT** work:

```csharp
PlayerUnitEvents.AddCustomEventFilter(EVENT_PLAYER_UNIT_TRAIN_START, "UnitTypeStartsBeingTrained", GetTrainedUnitType);
```

## Events

The following events are currently supported by default:

* HeroTypeBecomesRevivable
* HeroTypeCancelsRevive
* HeroTypeFinishesRevive
* HeroTypeLearnsSpell
* HeroTypeLevels
* HeroTypeStartsRevive
* ItemTypeIsDropped
* ItemTypeIsPawned
* ItemTypeIsPickedUp
* ItemTypeIsSold
* ItemTypeIsStacked
* ItemTypeIsUsed
* PlayerDeselectsUnitType
* PlayerSelectsUnitType
* ResearchIsCancelled
* ResearchIsFinished
* ResearchIsStarted
* SpellCast
* SpellCastOnUnitType
* SpellChannel
* SpellChannelOnUnitType
* SpellEffect
* SpellEffectOnUnitType
* SpellEndCast
* SpellEndCastOnUnitType
* SpellFinish
* SpellFinishOnUnitType
* SpellLearnedByHeroType
* UnitTypeAttacks
* UnitTypeCancelsBeingConstructed
* UnitTypeCancelsBeingTrained
* UnitTypeCancelsConstruction
* UnitTypeCancelsResearch
* UnitTypeCancelsTraining
* UnitTypeCancelsUpgrade
* UnitTypeChangesOwner
* UnitTypeDamages
* UnitTypeDecays
* UnitTypeDies
* UnitTypeDropsItem
* UnitTypeFinishesBeingConstructed
* UnitTypeFinishesBeingTrained
* UnitTypeFinishesConstruction
* UnitTypeFinishesResearch
* UnitTypeFinishesTraining
* UnitTypeFinishesUpgrade
* UnitTypeIsAttacked
* UnitTypeIsCreated
* UnitTypeIsDamaged
* UnitTypeIsDeselected
* UnitTypeIsDetected
* UnitTypeIsHidden
* UnitTypeIsLoaded
* UnitTypeIsRescued
* UnitTypeIsSelected
* UnitTypeIsSold
* UnitTypeIsSummoned
* UnitTypeKills
* UnitTypeLoads
* UnitTypePawnsItem
* UnitTypePicksUpItem
* UnitTypeReceivesOrder
* UnitTypeReceivesPointOrder
* UnitTypeReceivesTargetOrder
* UnitTypeReceivesUnitTypeOrder
* UnitTypeRescues
* UnitTypeSellsItem
* UnitTypeSellsUnitType
* UnitTypeSpellCast
* UnitTypeSpellChannel
* UnitTypeSpellEffect
* UnitTypeSpellEndCast
* UnitTypeSpellFinish
* UnitTypeStacksItem
* UnitTypeStartsBeingConstructed
* UnitTypeStartsBeingTrained
* UnitTypeStartsConstruction
* UnitTypeStartsResearch
* UnitTypeStartsTraining
* UnitTypeStartsUpgrade
* UnitTypeSummons
* UnitTypeUsesItem
