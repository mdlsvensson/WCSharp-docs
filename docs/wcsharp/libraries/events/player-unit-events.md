# PlayerUnitEvents

The PlayerUnitEvents system is a powerful system to replace the native `playerunitevent` events, with a focus on improving performance. It offers the following features:

* Automates creation of triggers and registration of events used. The user only needs to attach actions to events.
* Easily attach any number of listeners to events.
* Filter events with a dictionary on some function. For example, you can supply an integer to use with `UnitTypeEvent.Kills`, so that the code only triggers when that unit type kills something. Multiple event registrations to this will use a dictionary, so you can listen for dozens of different unit types on the same event without any change in performance.
* Automatically merges events that use the same native event for performance gains. One of the most expensive steps for events is transitioning from internal WarCraft 3 code into the actual user triggers, and this is avoided by only using a single event.
* Using some preprocessing steps, will only run what is actually in use.
* Provides numerous new events for ease of use. These events still rely on the natives behind the scenes, therefore not incurring additional performance costs. They only exist to simplify the logic.
* Do you want to filter events by something not provided by default? No problem! You can register your own custom event filters. Their performance is exactly the same as events that are provided by default.

## Improvements over v2.0

WCSharp.Events v2.1 improves the following aspects of v2.0:

* Makes events clearer and more concise. For example, what used to be `PlayerUnitEvent.UnitTypeKills` is now `UnitTypeEvent.Kills`. This new style also allows more distinct events based on the same underlying event, such as `ItemTypeEvent.IsPickedUp` and `UnitTypeEvent.PicksUpItem`.
* By having different event types (such as UnitTypeEvent, ItemTypeEvent, SpellEvent, etc), IntelliSense can provide more information on expected parameters.
* Directly supports single-source events, i.e. specific units instead of entire unit types.
* Addresses problems with registering to the same event with the same filter multiple times. WCSharp will now merge these events for you, instead of forcing you to do so.

## Usage

This system is used by registering/unregistering events and actions. This is done with the following methods:

* `PlayerUnitEvents.Register(eventType, action)` - Will invoke the given action when the given event fires.
* `PlayerUnitEvents.Register(eventType, action, filterTypeId)` - Will invoke the given action when the given event fires, filtered by the type id supplied. Explanations for what the type means in correlation to each event is typically intuitive to the name of the event, and further explanations are supplied by IntelliSense in Visual Studio.
* `PlayerUnitEvents.Unregister(eventType, action)` - Removes a previously registered action for the given event if possible.
* `PlayerUnitEvents.Unregister(eventType, filterTypeId)` - Removes a previously registered action for the given event/type filter if possible.

For those unfamiliar with C#, Actions can be either a method or a lambda function such as `() => { ... do some actions ... }`. Additionally, `eventType` refers to one of the event types. A full list is supplied below.

## Events

Hero events are triggered by a specific hero. `filterTypeId` is a unit object and must be supplied.

* HeroEvent.BecomesRevivable
* HeroEvent.CancelsRevive
* HeroEvent.FinishesRevive
* HeroEvent.LearnsSpell
* HeroEvent.Levels
* HeroEvent.StartsRevive

Hero type events are triggered by any hero of the given unit type. `filterTypeId` is a unit type id (integer). By not specifying a unit type, it will trigger for any hero regardless of unit type.

* HeroTypeEvent.BecomesRevivable
* HeroTypeEvent.CancelsRevive
* HeroTypeEvent.FinishesRevive
* HeroTypeEvent.LearnsSpell
* HeroTypeEvent.Levels
* HeroTypeEvent.StartsRevive

Item events are triggered by specific items. `filterTypeId` is an item object and must be supplied.

* ItemEvent.IsAbsorbed
* ItemEvent.IsDropped
* ItemEvent.IsPawned
* ItemEvent.IsPickedUp
* ItemEvent.IsSold
* ItemEvent.IsStacked
* ItemEvent.IsUsed

Item type events are triggered by any item of the given item type. `filterTypeId` is a item type id (integer). By not specifying an item type, it will trigger for any item regardless of item type.

* ItemTypeEvent.IsAbsorbed
* ItemTypeEvent.IsDropped
* ItemTypeEvent.IsPawned
* ItemTypeEvent.IsPickedUp
* ItemTypeEvent.IsSold
* ItemTypeEvent.IsStacked
* ItemTypeEvent.IsUsed

Player events are triggered by the actions of a specific player. `filterTypeId` can be given either a 0-indexed player id or a player object. By not specifying either, it will trigger for any player.

* PlayerEvent.DeselectsUnit
* PlayerEvent.GainsOwnership
* PlayerEvent.LosesOwnership
* PlayerEvent.SelectsUnit

Research events are triggered by a specific research. `filterTypeId` is the object editor identifier of the research. By not specifying an identifier, it will trigger for any research.

* ResearchEvent.IsCancelled
* ResearchEvent.IsFinished
* ResearchEvent.IsStarted

Spell events are triggered by abilities of the given ability type. `filterTypeId` is the object editor identifier of the ability. By not specifying an ability type, it will trigger for any ability.

* SpellEvent.Cast
* SpellEvent.Channel
* SpellEvent.Effect
* SpellEvent.EndCast
* SpellEvent.Finish
* SpellEvent.Learned

Unit events are triggered by specific unit, hero or building. `filterTypeId` is a unit object and must be supplied.

* UnitEvent.Attacks
* UnitEvent.BuysUnit
* UnitEvent.CancelsBeingConstructed
* UnitEvent.CancelsConstruction
* UnitEvent.CancelsResearch
* UnitEvent.CancelsTraining
* UnitEvent.CancelsUpgrade
* UnitEvent.ChangesOwner
* UnitEvent.Damaging
* UnitEvent.Decays
* UnitEvent.Dies
* UnitEvent.DropsItem
* UnitEvent.FinishesBeingConstructed
* UnitEvent.FinishesConstruction
* UnitEvent.FinishesResearch
* UnitEvent.FinishesTraining
* UnitEvent.FinishesUpgrade
* UnitEvent.IsAttacked
* UnitEvent.IsDamaged
* UnitEvent.IsDeselected
* UnitEvent.IsDetected
* UnitEvent.IsHidden
* UnitEvent.IsLoaded
* UnitEvent.IsRescued
* UnitEvent.IsSelected
* UnitEvent.IsSold
* UnitEvent.Kills
* UnitEvent.Loads
* UnitEvent.PawnsItem
* UnitEvent.PicksUpItem
* UnitEvent.ReceivesOrder
* UnitEvent.ReceivesPointOrder
* UnitEvent.ReceivesTargetOrder
* UnitEvent.ReceivesUnitTypeOrder
* UnitEvent.Rescues
* UnitEvent.SellsItem
* UnitEvent.SellsUnit
* UnitEvent.SpellCast
* UnitEvent.SpellCastOn
* UnitEvent.SpellChannel
* UnitEvent.SpellChannelOn
* UnitEvent.SpellEffect
* UnitEvent.SpellEffectOn
* UnitEvent.SpellEndCast
* UnitEvent.SpellEndCastOn
* UnitEvent.SpellFinish
* UnitEvent.SpellFinishOn
* UnitEvent.StacksItem
* UnitEvent.StartsConstruction
* UnitEvent.StartsResearch
* UnitEvent.StartsTraining
* UnitEvent.StartsUpgrade
* UnitEvent.Summons
* UnitEvent.UsesItem

Unit type events are triggered by units of the given unit type. `filterTypeId` is a unit type id (integer). By not specifying a unit type, it will trigger for any unit, regardless of unit type.

* UnitTypeEvent.Attacks
* UnitTypeEvent.BuysUnit
* UnitTypeEvent.CancelsBeingConstructed
* UnitTypeEvent.CancelsBeingTrained
* UnitTypeEvent.CancelsConstruction
* UnitTypeEvent.CancelsResearch
* UnitTypeEvent.CancelsTraining
* UnitTypeEvent.CancelsUpgrade
* UnitTypeEvent.ChangesOwner
* UnitTypeEvent.Damaging
* UnitTypeEvent.Decays
* UnitTypeEvent.Dies
* UnitTypeEvent.DropsItem
* UnitTypeEvent.FinishesBeingConstructed
* UnitTypeEvent.FinishesBeingTrained
* UnitTypeEvent.FinishesConstruction
* UnitTypeEvent.FinishesResearch
* UnitTypeEvent.FinishesTraining
* UnitTypeEvent.FinishesUpgrade
* UnitTypeEvent.IsAttacked
* UnitTypeEvent.IsCreated
* UnitTypeEvent.IsDamaged
* UnitTypeEvent.IsDeselected
* UnitTypeEvent.IsDetected
* UnitTypeEvent.IsHidden
* UnitTypeEvent.IsLoaded
* UnitTypeEvent.IsRescued
* UnitTypeEvent.IsSelected
* UnitTypeEvent.IsSold
* UnitTypeEvent.IsSummoned
* UnitTypeEvent.Kills
* UnitTypeEvent.Loads
* UnitTypeEvent.PawnsItem
* UnitTypeEvent.PicksUpItem
* UnitTypeEvent.ReceivesOrder
* UnitTypeEvent.ReceivesPointOrder
* UnitTypeEvent.ReceivesTargetOrder
* UnitTypeEvent.ReceivesUnitTypeOrder
* UnitTypeEvent.Rescues
* UnitTypeEvent.SellsItem
* UnitTypeEvent.SellsUnit
* UnitTypeEvent.SpellCast
* UnitTypeEvent.SpellCastOn
* UnitTypeEvent.SpellChannel
* UnitTypeEvent.SpellChannelOn
* UnitTypeEvent.SpellEffect
* UnitTypeEvent.SpellEffectOn
* UnitTypeEvent.SpellEndCast
* UnitTypeEvent.SpellEndCastOn
* UnitTypeEvent.SpellFinish
* UnitTypeEvent.SpellFinishOn
* UnitTypeEvent.StacksItem
* UnitTypeEvent.StartsBeingConstructed
* UnitTypeEvent.StartsBeingTrained
* UnitTypeEvent.StartsConstruction
* UnitTypeEvent.StartsResearch
* UnitTypeEvent.StartsTraining
* UnitTypeEvent.StartsUpgrade
* UnitTypeEvent.Summons
* UnitTypeEvent.UsesItem

## Defining custom events

Custom events are created using `PlayerUnitEvents.AddCustomEvent`. For example, the custom event equivalent of `UnitTypeEvent.Kills` would be: `AddCustomEvent(EVENT_PLAYER_UNIT_DEATH, "SomeIdentifier", () => GetUnitTypeId(GetKillingUnit()))`. You can then use "SomeIdentifier" to register any number of events to this custom event. Absolute worth it if you want to register multiple events to the same type of filer! There is no performance difference between an event predefined and one that is custom defined.

* `PlayerUnitEvents.Register(identifier, action, filterTypeId)` - The same as above, but the identifier refers to a custom event that was added using `AddCustomEventFilter`.
* `PlayerUnitEvents.Unregister(identifier, filterTypeId)` - The same as above, but the identifier refers to a custom event that was added using `AddCustomEventFilter`.
Due to quirks of the C# to Lua conversion, you cannot refer directly to WC3 natives when adding custom event filters. They must be enclosed in either a method or a lambda.

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
