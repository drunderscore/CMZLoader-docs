Some more advanced mods may need to send data to other gamers in the session.
This can be achieved with network messages.

## Creating a Network Message

There are two classes you need know in order to create your message:
`NetMessage` and `NetworkName`.

Your message **must** inherit the `NetMessage` type. This will implement
3 methods, which will read, write, and receive your message. It is **your job**
to correctly read and write the data. CastleMiner Z implements an internal checksum
that may fail if not correctly read.

Your message **must** also have a parameterless constructor. It can still contain
other constructors, as long as at least one of them is parameterless.

In order for your message to be identifiable and unique, you must also mark your
message with the `NetworkNameAttribute`. This will contain the name of the message.

From the XML documentation:

> The network name for this NetMessage.

> All network messages should be in the format of `namespace:key`. Where 'namespace'
> is an unique, identifiable name (like your mod title), and 'key' describes the
> contents of the message.

## Sending your Network Message

Once you have correctly created your message class, you can now send it across the game.
You can either broadcast it to all gamers in the session (including yourself), or
a specific recipient. Use the `NetworkManager` in order to send a `NetMessage`.

_Examples:_

```csharp hl_lines="2 6 27"
    // Purpose: Allow the host to change the gamemode of any gamer.
    [NetworkName( "docsmod:changegamemode" )]
    public class ChangeGamemodeMessage : NetMessage {
        public GameModeTypes Mode { get; private set; }

        public ChangeGamemodeMessage() { }

        public ChangeGamemodeMessage( GameModeTypes m ) {
            Mode = m;
        }

        public override void Read( BinaryReader stream ) {
            Mode = (GameModeTypes)stream.ReadInt32();
        }

        public override void Receive( NetworkGamer sender ) {
            if ( sender.IsHost )
                CastleMinerZGame.Instance.GameMode = Mode;
        }

        public override void Write( BinaryWriter stream ) {
            stream.Write( (int)Mode );
        }
    }

    // ...anywhere else in your code.
    NetworkManager.Send( new ChangeGamemodeMessage( GameModeTypes.DragonEndurance ) );
```
