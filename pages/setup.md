# SETTING UP MODDING PROJECTS

	TODO: Explain what each of these is good for.
---
## The Unity Asset Project:
- All necessary instructions and GUID fixer provided here: 
	- https://github.com/ChrisFeline/AssetRipperGuidPatcher/blob/main/README.md
  
---
## The VS Community 2022 Project
DOC: https://github.com/Cryptoc1/lc-plugin-sdk/blob/main/README.md 
1. Create a new "Class Library" (.NET Standard 2.1)
2. Open the .csproj and replace `Microsoft.NET.Sdk` with `LethalCompany.Plugin.Sdk/1.0.0`
3. Add the following to the same PropertyGroup you just modified:

```xml
<AllowUnsafeBlocks>true</AllowUnsafeBlocks>
<RootNamespace>YOUR_ROOT_NAMESPACE</RootNamespace>
<Title>YOUR_PLUGIN_TITLE</Title>
<Description>YOUR_PLUGIN_DESCRIPTION</Description>
<PluginId>YOUR_PLUGIN_GUID</PluginId>
<Version>0.0.1</Version>
<ThunderWebsiteUrl>YOUR_GITHUB_REPO</ThunderWebsiteUrl>
```

If you want to delve deeper into how the VS project works, see [the MSBuild page](../pages/msbuild.md)

## Barebones BepInEx for LC
the LC SDK's equivalent to the `Plugin.cs` from [the template](https://github.com/LethalCompany/LethalCompanyTemplate/blob/main/LethalCompanyTemplate/Plugin.cs) is as shown below
```cs
namespace YOUR_ROOT_NAMESPACE
{
	[BepInPlugin(GeneratedPluginInfo.Identifier, GeneratedPluginInfo.Name, GeneratedPluginInfo.Version)]
	public sealed class SamplePlugin : BaseUnityPlugin
	{
		private void Awake()
		{
			Logger.LogInfo($"Plugin {GeneratedPluginInfo.Identifier} is loaded!");
		}
	}
}
```
- Take note of `Logger.LogInfo()`. You'll wanna use it for debugging.
	- It automatically converts numbers and booleans and such you put into it, so don't worry about converting them.
	- If A string begins with a `$` like `$"bla bla bla {variable}"`, anything placed in `{}` curly brackets will be automatically combined with the string, right in that spot.

---
Willis's mod which uses LC SDK: https://github.com/willis81808/LethalSettings