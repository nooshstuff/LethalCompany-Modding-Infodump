# TIPS 4 USING YOUR IDE
This is about VS Community. For now....

---
### COOL CMD TIP!!
- In VSC, go to `View>Terminal` or use Ctrl+\` to get a sweet terminal in VSC
- Whenever I say something like `cmd>` it means put the text block after it into your command line and run it
---
### Making a zip for ThunderStore
- cmd> `dotnet publish -c Release`
	- it'll tell you where it put it (ends in `/bin/Releases/`)
	- you can put this or the dll you get from just building (Ctrl+Shift+B) in r2modman to test your mod by going into `Settings>Import local mod` and selecting the zip/dll
---
### Autofill / IntelliSense
- I cheat a lot while writing code in IDEs. If the IDE is aware a function or class exists it'll autofill it for you.
- They wont autofill for something that isn't currently accessible (for example, not imported yet)
- One trick, if you know the class's name! Write it out, right click the code that now has a warning (red squiggly underline) on it, and use it's offers to fix it to either:
	- Import the class into your current C# file (best for if you're going to be referencing that class multiple times)
	- Replace it with the full path of the class (this is better for if you're only ever referencing this class a single time in that file)
- The autofill will autofill the names of function arguments and other things like that for you.  When relevant I'll be using the names it provides for recognizability
---