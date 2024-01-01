# Setup, but Without LC SDK

	TODO: Instructions for when you're not using the LC SDK
  
#### Differences in the .csproj :
- if not using `LethalCompany.Plugin.Sdk` some things will be different
	- `<Product>` instead of `<Title>`
	- `<AssemblyName>` instead of `<PluginId>`
---
## Barebones BepInEx for LC
`Plugin.cs` from [BepInEx template](https://github.com/LethalCompany/LethalCompanyTemplate/blob/main/LethalCompanyTemplate/Plugin.cs)