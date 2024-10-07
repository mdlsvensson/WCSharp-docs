!!! attention
    In order to use the dummy system, you MUST have a Dummy unit type with the unit ID of "xxxx" (2021161080). The WCSharp comes with this dummy by default. For more information on dummies, please take a look at related threads on Hiveworkshop.

The dummy system offers the following features:

* Provides a simple means of acquiring a dummy
* Recycles dummies after they are no longer in use, thus saving memory and preventing leaks
* Automatically removes spells from dummies after they've cast it

# Usage

The dummy system provides a simple way of acquiring and recycling dummies. Luckily, dummies are no longer needed for a missile system, but dummies are still very convenient for various other tasks, such as applying in-game (de)buffs to a target (e.g. have it stun a target with a custom zero damage Firebolt spell).

Using the dummy system is very straight forward, at any point you can use `var dummy = DummySystem.GetDummy(...)` to acquire a dummy unit. When you are done with the dummy, you can release it using `DummySystem.RecycleDummy(dummy, recycleTime)`. The `recycleTime` argument specifies the time (in seconds) after which the dummy is made available again for use. You may leave it out, in which case it will automatically make the dummy available for use again after 2 seconds.
