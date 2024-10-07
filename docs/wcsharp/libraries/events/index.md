# WCSharp.Events

WCSharp.Events is a set of systems designed to simplify and improve working with events in Warcraft III by having them handle the creation of triggers for you. Additionally, these systems are aimed for high performance and will bundle together events where possible.

Currently, there are two systems defined:

* [PeriodicEvents](periodic-events.md) - This system allows for the easy addition of periodic events and triggers in a natural way, allowing you to create any number of concurrent instances for actions.
* [PlayerUnitEvents](player-unit-events.md) - This system is designed as a complete replacement of Warcraft III's playerunitevent types. Events using the same underlying Warcraft III event are bundled together, and it allows for constant time filtering on any given event value.
