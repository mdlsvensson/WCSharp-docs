# Home

WCSharp is a powerful set of libraries for Warcraft III map development with C#. Write your map logic in C# and transpile it to Lua code that the game engine can understand.

**Why C#?**

- **Better Code:** C# is natively object-oriented, enabling cleaner, more maintainable code.
- **Type Safety:** Static typing prevents bugs during development, without the need for EmmyLua annotations.
- **IDE:** Write Warcraft III map logic in Visual Studio.
- **Built in Systems:** WCSharp simplifies Warcraft III modding by providing pre-built systems for common tasks.

## Quickstart

!!! info
    Full installation instructions are available [here](getting-started.md).

1. [Download the WCSharp template](https://github.com/Orden4/WCSharp/wiki/WCSharp-template).
2. Open the solution and update NuGet packages.
3. Set your Warcraft III executable path in `Launcher/app.config`.
4. Move the included `Blizzard.j` and `common.j` files to `C:\Users\[Username]\My Documents\Warcraft III\JassHelper`.
5. Run the `Launcher` project.