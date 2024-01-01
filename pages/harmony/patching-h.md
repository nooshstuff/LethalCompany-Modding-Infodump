# PATCHING CODE IN LC (with Harmony)
### Decompile Navigation Guide:
	TODO: write about dnSpy and ILSpy to find a thing to hook to
## Setup
BepInEx comes with Harmony ready to use. No setup.

## Basic hook & patch
DOC: https://harmony.pardeike.net/articles/patching.html

	TODO:  Write guide from your code comments

```cs
using GameNetcodeStuff;
using UnityEngine; // you would need this to do stuff like get Time.deltatime

namespace NooshMod
{
	[HarmonyPatch(typeof(PlayerControllerB))]	// if you didnt do this up here you could do it on the indivual functions. when you do this you cant patch other classes
	internal class PlayerSpeedPatch // having classes related to patches have the name Patch might be useful
	{
		[HarmonyPostfix] // theres also [HarmonyPrefix]
		[HarmonyPatch(nameof(PlayerControllerB.Update))] // every patch needs a class type and class name. this could also be a string but whatever too complicated
		public static void ApplyBigJumpValue( PlayerControllerB __instance) // double underscore is a naming convention https://harmony.pardeike.net/articles/patching-injections.html
		{
			//var dt = Time.deltaTime;
			__instance.jumpForce = 100;	// this is a float but its implicitly cast. remember to use f when you're using decimals in math
		}
	}
}
```
## Applying the patch
This could be considered a form of setup. The way your patches are applied is by having the line below in your equivalent to the "Plugin.cs" (the C# file you have the `[BepInPlugin(...)]` tag in):
```cs
Harmony.CreateAndPatchAll(System.Reflection.Assembly.GetExecutingAssembly(), GeneratedPluginInfo.Identifier);
```
This applies all the patches made using Harmony. (the ones that have the `[HarmonyPatch]` tag in them)