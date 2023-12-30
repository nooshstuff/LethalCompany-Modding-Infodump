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
#### Sidenote About csproj :
- if not using `LethalCompany.Plugin.Sdk` some things will be different
	- `<Product>` instead of `<Title>`
	- `<AssemblyName>` instead of `<PluginId>`
  
---
---
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