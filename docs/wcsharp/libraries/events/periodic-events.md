# PeriodicEvents

The periodic events system offers the following features:

* Manages all periodic events on the same timer, while ensuring that each event fires at its appropriate time, and are removed when no longer in use.
* Offers a PeriodicTrigger class to easily create and automatically manage multi-instanceable events.

## Usage

There are two ways to use the periodic events system. The first is using `PeriodicEvents.AddPeriodicEvent(func, period)`. This will create a PeriodicEvent handler that will continue to loop until the `func` returns false. This is mainly intended to be used when a single instance can handle everything, such as e.g. resource ticks in a map.

In most cases, you will want to use a PeriodicTrigger instead. This class allows you to easily create and add multiple instances of a trigger. Due to its design, it is automatically a multi-instanceable event, as each instance is independently managed by the PeriodicTrigger.

To begin using a PeriodicTrigger, we first need to create a new class that implements IPeriodicAction:

```csharp
public class TestTrigger : IPeriodicAction
{
}
```

To implement this interface, simply press ALT+Enter while you have the name of the class selected and choose to implement the interface, which will produce the following:
```csharp
public class TestTrigger : IPeriodicAction
{
	public bool Active { get; set; }

	public void Action()
	{

	}
}
```

From here, we can fill in `Action` with whatever tasks should be done on every tick of this periodic action . When we no longer want the periodic action to continue, simply set `Active` to false.

Now we just need to actually create our PeriodicTrigger to manage this periodic action. For example, the below code will, for each unit created, attach a periodic action that causes the unit to deal 10 Chaos damage to itself every second for as long as it lives.

```csharp
public class TestTrigger : IPeriodicAction
{
	public static PeriodicTrigger<TestTrigger> periodicTrigger;

	public static void Initialize()
	{
		periodicTrigger = new PeriodicTrigger<TestTrigger>(1.0f);
		PlayerUnitEvents.Register(PlayerUnitEvent.UnitIsCreated, OnUnitCreated)
	}

	private static void OnUnitCreated()
	{
		periodicTrigger.Add(new TestTrigger
		{
			Target = GetTriggerUnit()
		});
	}

	public bool Active { get; set; }
	public unit Target { get; set; }

	public void Action()
	{
		if (UnitAlive(Target))
		{
			Target.Damage(Target, 10.0f, ATTACK_TYPE_CHAOS);
		}
		else
		{
			Active = false;
		}
	}
}
```

## PeriodicDisposableTrigger

An alternative to `IPeriodicAction/PeriodicTrigger` is `IPeriodicDisposableAction/PeriodicDisposableTrigger`. These two essentially work the same, but add an additional `Dispose` method to every IPeriodicDisposableAction that is automatically invoked after `Active` is set to false. This alternative is used by, for example, [WCSharp.Missiles](../missiles/index.md) to clean up the special effect and perform other required actions.

## SmoothTrigger / SmoothDisposableTrigger

Additionally, there are "smooth" variants of the standard periodic ones. These are for when you want the actions to trigger more granularly from one another.

For example, consider if you have a PeriodicTrigger with an interval of 1 seconds. At time 0.0, you add Action1, at time 0.5, you add Action2. In this case, both actions will execute at time 1.0, 2.0, 3.0, etc. This is generally not a significant issue if your interval is small, players won't notice anyway. However, if your interval is longer, it can become more noticeable and even problematic.

For this reason, SmoothTrigger and SmoothDisposableTrigger offer a smoother alternative. In the above example, they will maintain the initial add timings, and trigger Action1 at 1.0/2.0/3.0 and Action2 at 0.5/1.5/2.5.
