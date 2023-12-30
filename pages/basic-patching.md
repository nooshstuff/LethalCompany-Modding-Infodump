# PATCHING CODE IN LC
### Decompile Navigation Guide:
	TODO: write about dnSpy and ILSpy to find a thing to hook to
# SETUP
## Harmony:
BepInEx comes with Harmony ready to use. Only thing that counts as setup would be having your patches applied by having the line below in your equivalent to the "Plugin.cs" (the C# file you have the `[BepInPlugin(...)]` tag in):
```cs
Harmony.CreateAndPatchAll(System.Reflection.Assembly.GetExecutingAssembly(), GeneratedPluginInfo.Identifier);
```
This applies all the patches made using Harmony. (the ones that have the `[HarmonyPatch]` tag in them)
## MonoMod:
	TODO: can LighterPatcher be used with Lethal Company?
1. get [HookGenPatcher](https://thunderstore.io/c/lethal-company/p/Evaisa/HookGenPatcher/) on ThunderStore and run the game with it on
2.  r2modman>Open Profile Folder. go to \BepInEx\plugins\MMHOOK and copy `MMHOOK_Assembly-CSharp` and paste it somewhere its not gonna be deleted or moved
3. Right click "Dependencies" under your csproj in the solution explorer. select "Add Project Reference", browse and select `MMHOOK_Assembly-CSharp`
# BASIC HOOKS
## Harmony:
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
			var dt = Time.deltaTime;
			__instance.jumpForce = 100;	// this is a float but its implicitly cast. remember to use f when you're using decimals in math
		}
	}
}
```
## MonoMod:
ROR2 DOC: https://risk-of-thunder.github.io/R2Wiki/Mod-Creation/C%23-Programming/Hooking/ 

	TODO:  Write guide from your code comments
	
```cs
using UnityEngine;

namespace NooshMod
{
	[BepInPlugin(GeneratedPluginInfo.Identifier, GeneratedPluginInfo.Name, GeneratedPluginInfo.Version)]
	public sealed class SamplePlugin : BaseUnityPlugin
	{
		private void Awake()
		{
			// Plugin startup logic
			Logger.LogInfo($"Plugin {GeneratedPluginInfo.Identifier} is loaded!");
			// Harmony
			Harmony.CreateAndPatchAll(System.Reflection.Assembly.GetExecutingAssembly(), GeneratedPluginInfo.Identifier);

			// NOW FOR MONOMOD
			On.GameNetcodeStuff.PlayerControllerB.Update += PlayerControllerB_Update_Funnybusiness;
		}

		private void PlayerControllerB_Update_Funnybusiness(On.GameNetcodeStuff.PlayerControllerB.orig_Update orig, GameNetcodeStuff.PlayerControllerB self)
		{
			orig(self); // this is like super.update() in other langauges. code's relative location to this is like doing prefix and postfix
			if (self.isSprinting) {
				float downwardsAngle = Vector3.Angle(self.playerEye.transform.forward, Vector3.down); // angle btween the two
				Logger.LogInfo($"Player downwards angle = {downwardsAngle}, Ramming Speed = {(downwardsAngle < 45.0f)}");
				if (downwardsAngle < 45.0f) {
					self.sprintMultiplier = Mathf.Lerp(self.sprintMultiplier, 10.0f, Time.deltaTime * 1f); // funny murder miners (max sprintmult is now 10)
				}
				else { self.sprintMultiplier = Mathf.Lerp(self.sprintMultiplier, 2.25f, Time.deltaTime * 1f); }
			}
			else { self.sprintMultiplier = Mathf.Lerp(self.sprintMultiplier, 1f, 10f * Time.deltaTime); }
		}
	}
}
```