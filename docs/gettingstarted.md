# Getting Started

To begin, you need the following skills:

- Knowledge of C# (or some other .NET language)
- Good reverse engineering skills
- Patience

...and the following tools:

- [Visual Studio 2019](https://visualstudio.microsoft.com/downloads/)
- [XNA 4.0 **Framework** Redistributable ](https://www.microsoft.com/en-us/download/confirmation.aspx?id=20914)
- A copy of [CMZLoader binaries](https://content.jame.xyz/cmz/loader/release/CMZLoader-Latest.zip)

## Creating & Preparing the Project

In Visual Studio, you must create the project from the template _"Class Library (.NET Framework)"_
for a .dll in your .NET language of choice.

Once the solution and project have been created, you must now add CMZLoader and XNA references.
You can right-click the project and go to `Add > Reference...`. Browse to your CastleMinerZ
directory that has CMZLoader installed, and select the following files (multiple by Ctrl + Click):

- `CastleMinerZ.exe`
- `DNA.Common.dll`
- `DNA.Steam.dll`
- `Services.Client.dll`

Once those have been added, traverse to the `Assemblies` tab of the reference manager, and search for `XNA`.
If nothing shows up, you **may not have installed the XNA Framework correctly.**

You can select all of the XNA Frameworks if you wish, but the ones you should always have are:

- `Microsoft.Xna.Framework.dll`
- `Microsoft.Xna.Framework.Game.dll`
- `Microsoft.Xna.Framework.Graphics.dll`
- `Microsoft.Xna.Framework.Xact.dll`

Be sure to click 'OK' upon finishing with the dialog. You should now have **successfully referenced all necessary assemblies.**

## Beginning Your Mod

The name and version of your mod is controlled by it's assembly attributes. These are usually pre-generated
for you, inside `AssemblyInfo.cs` (usually in the `Properties` tab). You can edit these to your liking,
however you should know what impact the following attributes have on your mod:

- `AssemblyTitleAttribute`: Controls the display name of your mod.
- `AssemblyVersionAttribute`: Controls the version of your mod.

_Example:_

```c# hl_lines="1 12"
[assembly: AssemblyTitle("Cool Docs")]
[assembly: AssemblyDescription("")]
[assembly: AssemblyConfiguration("")]
[assembly: AssemblyCompany("")]
[assembly: AssemblyProduct("Cool Docs")]
[assembly: AssemblyCopyright("Copyright © James Puleo 2020")]
[assembly: AssemblyTrademark("")]
[assembly: AssemblyCulture("")]
[assembly: ComVisible(false)]
[assembly: Guid("10e20dec-0429-4412-add0-9c6a98430230")]

[assembly: AssemblyVersion("1.0.0.0")]
[assembly: AssemblyFileVersion("1.0.0.0")]
```

Your mod is **required** to have **one class** that inherits the `Mod` class from CMZLoader.
This alone is enough to have your mod shown in the mod list.

_Example:_

```c# hl_lines="3"
namespace CoolDocs {
    public class CoolDocsMod : Mod {
        // ...
    }
}
```
