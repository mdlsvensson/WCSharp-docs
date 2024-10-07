**Use of this package requires that the compilers IsExportMetadata property is set to true.**

Although probably not directly useful to the average map maker, the sync system is a powerful backbone for [WCSharp.SaveLoad](save-load/index.md) and [WCSharp.DateTime](date-time.md).

The sync system makes it trivial to send generic and complex data structures between players. For example, [WCSharp.SaveLoad](save-load/index.md) loads saves from local data, and then has to synchronise this between players. Originally, sync data is limited to strings of 255 characters. Combined with there not being a standard conversion format, this often led to cumbersome custom methods for syncing data.

The first giant improvement to this that is made by the sync system is that it uses the [WCSharp.JsonConvert](json-convert/index.md) package to (de)serialize generic and complex data structures, removing the process of defining custom methods of sending/parsing data for the user. Furthermore, it automatically splits up the data into packets, similar to how the internet works, so that it can send data of any size.

The sync system works as a basic subscription model and only has 3 methods:

* **Subscribe<T>(Action<T> handler)** - Subscribes the given handler to receive messages of type T whenever they are received
* **Unsubscribe<T>(Action<T> handler)** - Unsubscribes the given handler to receive messages of type T whenever they are received
* **Send<T>(T message)** - Initiates a sync process, synchronising the given message to be received by all players.

As each subscription is bound to a specific type, it means that different systems can easily subscribe to the different messages without any conflict occurring. Note that the types of the `Subscribe/Unsubscribe` and `Send` must match exactly, it does not account for derived/inherited types.

As an example use case of the sync system, the below example illustrates how it is used to synchronise timestamps in [WCSharp.DateTime](date-time.md):

```csharp
public void Run()
{
	// collect current time in seconds...

	// construct message
	var message = new DateTimeSyncMessage
	{
		PlayerId = GetPlayerId(GetLocalPlayer()),
		Seconds = seconds
	};

	// subscribe to DateTimeSyncMessage messages and send it
	SyncSystem.Subscribe<DateTimeSyncMessage>(HandleDateTimeSyncMessage);
	SyncSystem.Send(message);
}

private void HandleDateTimeSyncMessage(DateTimeSyncMessage message)
{
	// handle the received message
}
```
