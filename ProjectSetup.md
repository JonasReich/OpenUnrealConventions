<sub>[Home](../README.md) / Project Setup </sub>

# Project Setup

## Source Control Ignore Files

Depending on your VCS strategy the exact setup will differ.
The most common setups either place the UE4 project root as depository root or place the UE4 project one level below the repo root directory, so working files, etc can be submitted in the same directory.

These reccommendations are only relevant from the unreal project root downwards.

1. Configure your ignore file before submitting anything
1. Consider configuring the ignore file as a whitelist instead of a blacklist. For this approach you have to ignore everything at the beginning and selectively unignore the files you need
1. Ignore all intermediate or generated files (esp. Intermediate/, Saved/ and Build/)
1. Consider ignoring binaries as well (Binaries/)
1. Ignore all local config files of your development tools and IDEs (e.g. .vs/)

```python (not really, but the highlighting matches)
# Sample .gitignore contents:

# UE4 generated files
Binaries/
Intermediate/
Saved/

# C++ IDE files for Visual Studio and Rider
.vs/
*.sln
.idea/
```

Remember: This is the bare minimum to get started that will prevent accidental submits of generated files, but depending on your specific project setup and workflows, you will likely need to add additional entries.

## Plugins and Target Platforms

By default a lot of plugins are enabled for your editor. Depending on your dev hardware this will cause noticeable delays when launching the editor. This is unfortunate when you consider that you will probably only ever need a fraction of plugins that are enabled by default.

We reccommend to switch off the following plugins for new projects:

* Plugins for platforms you do not target (see project settings below), esp.
    * Mobile devices (Android, iOS)
    * VR (SteamVR, OpenVR, Oculus)
    * Linux, OSX
* Plugins for Input Hardware you do not intend to support (esp. Leap Motion)
* Online Subsystems if you are developing an offline / single player game
* IDE integrations you don't plan to use, e.g. XCode, Rider, KDevelop, CLion, etc

## Project Settings

Many of the project settings will change over time and only really need to be configured once you start interacting with the respective editor or runtime systems. However, there are a few settings that should be set for every single UE4 project from the get-go:

* Project Info + Legal
    * Project Name
    * Company Name
    * Copyright Notice, which will be used for newly created source files
* Supported Platforms
* Target Hardware
* Garbage Collection
    * Enable Actor Clustering
* General Settings
    * Disable Default Blueprint Ticks ("Can Blueprints Tick by Default")
* Rendering
    * Depending on your target platforms, you will have to pick different settings
    * Always consider the following settings and make deliberate decisions:
        * Do you render in forward (e.g. for mobile or VR) or deferred mode (deferred is default)
        * Do you want to bake static lighting?
            * On some platforms it may be cheaper
            * For some games, e.g. open world games it may not be feasible, disable here to reduce shader complexity
        * Do you want to use [physical lighting units](https://docs.unrealengine.com/en-US/BuildingWorlds/LightingAndShadows/PhysicalLightUnits/index.html)? This is enabled by default after 4.26 ("Extend default luminance range in Auto Exposure settings")
* User Interface
    * Set Render Focus Rule to Never
* Widget Designer (Team)
    * Consider changing Default Compiler Options to prevent property bindings and Tick and only enable them for certain sub-folders
    * Change Default Root Widget to None

For international dev teams using non US-keyboard layouts
* Gameplay Debugger
    * Activation Key (We reccomended to use ``Num /`` or ``Num ,``)
* Input
    * Console Key (We reccommend to use either ``Caret ^``, which is in the same location as ``~`` on US keyboards, ``F12`` or ``Home``)

### TL/DR

```ini
;---------------------
; DefaultGame.ini
;---------------------
[/Script/EngineSettings.GeneralProjectSettings]
CopyrightNotice=Copyright (c) 2021 Jonas Reich
ProjectName=Sample Project
CompanyName=Jonas Reich

;---------------------
; DefaultEngine.ini
;---------------------
[/Script/Engine.RendererSettings]
r.DefaultFeature.AutoExposure.ExtendDefaultLuminanceRange=True

[/Script/HardwareTargeting.HardwareTargetingSettings]
TargetedHardwareClass=Desktop
AppliedTargetedHardwareClass=Desktop
DefaultGraphicsPerformance=Maximum
AppliedDefaultGraphicsPerformance=Maximum

[/Script/Engine.UserInterfaceSettings]
RenderFocusRule=Never
```

## Default Editor Settings

The editor settings can be set to default and shared across the entire team. It's reccommended you do this immediately after setting up a new workspace or by manually editing the ini files.

* Appearance
    * Use Small Tool Bar Icons
    * Set "Asset Editor Open Location" to ``Last Docked Window or New Window``
* Experimental
    * Blueprint Break on Exceptions
    * Base Classes to Allow Recompiling During Play in Editor
* Keyboard Shortcuts
    * If you changed Ingame Console Key (see above), set the same key for Editor
* Live Coding
    * Enable Live Coding for all C++ Projects
* Loading & Saving
    * Consider Disabling AutoSave
* Miscellaneous
    * Disable "Automatically Compile Newly Added C++ Classes"
* Performance
    * Show Frame Rate and Memory
    * Consider disabling "Disable realtime vieweports by default in Remote Sessions" if you work often with Remote Desktop (check if your hardware supports this!)
* Tutorials
    * Disable All Tutorial Alerts (for experienced devs)
* Blueprint Editor
    * Auto Cast Object Connections
    * Disable Spawn Default Blueprint Nodes
    * Consider Disabling "Hide Constrction Script Components in Details View"
    * Allow Explicit Impure Node Disabling
    * Save on Compile: ``On Success Only``

### TL/DR

```ini
;---------------------
; DefaultEditor.ini
;---------------------
[/Script/UMGEditor.UMGEditorProjectSettings]
DefaultCompilerOptions=(bAllowBlueprintTick=True,bAllowBlueprintPaint=True,PropertyBindingRule=PreventAndWarn,Rules=(None))
DefaultRootWidget=None

;---------------------
; DefaultEditorSettings.ini
;---------------------
[/Script/UnrealEd.EditorPerformanceSettings]
bShowFrameRateAndMemory=True
bDisableRealtimeViewportsInRemoteSessions=False

[/Script/IntroTutorials.EditorTutorialSettings]
bDisableAllTutorialAlerts=True

;---------------------
; DefaultEditorPerProjectUserSettings.ini
;---------------------
[/Script/LiveCoding.LiveCodingSettings]
bEnabled=True

[/Script/UnrealEd.EditorExperimentalSettings]
bBreakOnExceptions=True
;Consider adding base classes for which to allow recompile during PIE:
+BaseClassesToAllowRecompilingDuringPlayInEditor=/Script/Engine.Actor

[/Script/EditorStyle.EditorStyleSettings]
bUseSmallToolBarIcons=True
AssetEditorOpenLocation=LastDockedWindowOrNewWindow

[/Script/UnrealEd.EditorPerProjectUserSettings]
bAutomaticallyHotReloadNewClasses=False

[/Script/BlueprintGraph.BlueprintEditorSettings]
bAutoCastObjectConnections=True
bShowInheritedVariables=True
bShowAccessSpecifier=True
bSpawnDefaultBlueprintNodes=False
bHideConstructionScriptComponentsInDetailsView=False
SaveOnCompile=SoC_SuccessOnly
bAllowExplicitImpureNodeDisabling=True
```

