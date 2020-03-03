# Logging

CMZLoader uses [NLog](https://nlog-project.org/) in order to log to file and the
in-game developer console. It is highly versatile, and easily implementable to
start logging out of the box.

This assembly exists on NuGet. Simply go to `Project > Manage NuGet Packages...` and
browse for NLog. You **do not and should not** distribute your mod with NLog.dll.
CMZLoader already does that with it's own configuration, and having multiple may cause
problems.

## Developer Console

Also attached to the logging system is the developer console. Pressing tilde (~)
at any time will bring up the developer console.

All commands and convars should be initialized in an [InitializationEvent](/eventsystem).

### Commands

Commands are input into the console with a space afterwards, that can process
the succeeding text, and execute ode based on it's input. You can use these to
do an action for a specific player, give yourself an item name, or even set
your health to that value.

_Examples:_

```csharp
// Purpose: Kills the player, only if they are in-game and the host.
DeveloperConsole.AddCommand( "killme", ( args ) => {
    CastleMinerZGame.Instance.GameScreen.HUD.KillPlayer();
}, "Kills the player", CommandFlags.InGameOnly | CommandFlags.HostOnly );
```

```csharp
// Purpose: Print the arguments passed to it, only if they are at the menu (not in game) (lambda)
private Logger _logger = LogManager.GetCurrentClassLogger();

DeveloperConsole.AddCommand( "what_did_i_say", ( args ) => {
    _logger.Info(string.Join(",", args));
}, "what did you say :o", CommandFlags.MenuOnly  );
```

```csharp
// Purpose: Demonstrate a command without a lambda.
private Logger _logger = LogManager.GetCurrentClassLogger();

DeveloperConsole.AddCommand( "woah", WoahCommand, "what did you say :o", CommandFlags.MenuOnly  );

private void WoahCommand(string[] args) {
    _logger.Info("woah!");
}
```

### Convars

Convar stands for 'console variable'. They hold a string value, which can be
interpreted as a `float`, `int`, or `bool` (and can be gotten as a string).
These are useful for options that are highly configurable by the user.

### Command Flags

Sometimes, you don't always want a command to be executed, or a convar to be modified.
Command flags prevent these from happening at different times. You can bitwise OR these
flags together to combine them. Flags like `InGameOnly` and `HostOnly` assure that
only the host can execute that command whilst they're in game.
