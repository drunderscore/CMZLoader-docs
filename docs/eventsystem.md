A majority of CMZLoader is based on events. Events are implemented and triggered
by the loader itself, and can help tell your mod when a specific action occurs.
Some of these events can be canceled, to prevent the original task from occuring,
and some of these can be modified to change the outcome of the task.

## Event Types

There are two kinds of events: `Pre` and `Post`.

`Pre` occurs before the original task executes. This allows you to make changes _before_
the original game code runs, possibly changing it's outcome.

`Post` occurs after the original task executes. This allows you to see the changes that
were made by the original game code, or perform something additional.

## Canceling an Event

Some events inherit the `ICancelable` interface. This interface implements the `Canceled`
property. This allows for a listener of an event to **skip executing the original game code**.

**NOTE:** Events can only be canceled if the event type is `Pre`, as this will make changes
_before_ the original game code runs. Events marked `Post` will have no intended effect.

## Listening for an Event

Events can **only be listened to by mods**, meaning your base mod class.
All events have designated classes that most often end in `Event`. Listening
for an event is done by a single method inside your mod class, as long as
it meets the following criteria:

- Only has a single parameter that inherits `Event`
- Is not marked `static`
- Is marked with a `ListenerAttribute`

Method names have **no** impact on the listening of the event; they are up to you.

_Examples:_

```c#
// Purpose: Logs whenever a gamer joins or leaves the game to the console.
private Logger _logger = LogManager.GetCurrentClassLogger();

[Listener]
private void OnGamerUpdate(GamerUpdateEvent e) {
    if(e.Joined)
        _logger.Info($"{e.Gamer.Gamertag} joined the game.");
    else
        _logger.Info($"{e.Gamer.Gamertag} left the game.");
}
```

```c#
// Purpose: Randomly prevent the player from shooting any gun.
Random r = new Random();

[Listener(EventType.Pre)] // MUST be Pre in order to cancel.
private void OnShootGun(ShootGunEvent e) {
    if(r.Next(10) <= 5)
        e.Canceled = true;
}
```

```c#
private Logger _logger = LogManager.GetCurrentClassLogger();

[Listener]
private void OnInitialize(InitializationEvent e) {
    _logger.Info("Hi, the mod is starting!");
}
```
