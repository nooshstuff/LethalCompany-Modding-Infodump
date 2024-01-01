# PATCHING CODE IN LC (with MonoMod)
### Decompile Navigation Guide:
	TODO: write about dnSpy and ILSpy to find a thing to hook to

## Setup
	TODO: can LighterPatcher be used with Lethal Company?
1. get [HookGenPatcher](https://thunderstore.io/c/lethal-company/p/Evaisa/HookGenPatcher/) on ThunderStore and run the game with it on
2.  r2modman>Open Profile Folder. go to \BepInEx\plugins\MMHOOK and copy `MMHOOK_Assembly-CSharp` and paste it somewhere its not gonna be deleted or moved
3. Right click "Dependencies" under your csproj in the solution explorer. select "Add Project Reference", browse and select `MMHOOK_Assembly-CSharp`

## Basic hook & patch
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
			// ... irellevent code removed

			// MONOMOD HOOK
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

## Applying the patch
It's done through the `On.GameNetcodeStuff.PlayerControllerB.Update += PlayerControllerB_Update_Funnybusiness;` line. Nothing else is needed to have the patch apply.

	TODO: explain some basic stuff about MonoMod works