---
sidebar_position: 1
---

# First Time Setup

## Compiling the Project
This will allow you to copy assets from the Zentools dump into the Unreal project through the file explorer and have it show up in the Unreal Content Browser

When opening the project for the first time or when updating it, you’ll need to compile the custom code. Right click on P3R.uproject, then select **Generate Visual Studio project files**:

![](/assets/images/events/p3r/explorer_xopfyq0vom.png)

Then open **P3R.sln** and at the top of the Visual Studio window and click on **Local Windows Debugger** or press F5

![](/assets/images/events/p3r/devenv_il2Kl9BKUk.png)

This will compile the code, then open the P3R Unreal project. You’ll see that it works if you see Xrd777 Edit on the top bar after all the built in Unreal options.

![](/assets/images/events/p3r/UE4Editor_GuOiTZhp3B.png)

After this, you can open the UProject without opening VS until you need to get an update from the repo.

:::tip[Only build the event directory]
If you’re working on many events, your project may end up referencing several thousand assets, which will take a while to cook on each build. It’s possible to exclude certain folders. At the top menu, click on Edit, then Project Settings. This will open the Project Settings menu, and in the Project > Packaging page, there’ll be an option for **Directories to never cook**:

By default, these two directories will be included in the list. You can add extra entries to it so that during packaging, these folders won’t be cooked.
:::

## Preparing an Event
### Event Structure
The structure of an event consists of the following:
- A Level Sequence file that contains all the visual tracks that play within a sequence. These follow the naming convention `LS_Event_[Type]_[Major]_[Minor]_C`.
- An audio sequence containing all the audio tracks for the sequence, named `LS_Event_[Type]_[Major]_[Minor]_C_Sound`. This is referenced in the main level sequence as a subsequence track.
- The level for the sequence. This contains all the objects that are referenced in the level sequence and an event manager of the type AtlEvtEventManager. Named `LV_Event_[Type]_[Major]_[Minor]_C` Inside the level sequence, this is named ATLUS_EventManager and stores tracks that aren't tied to a particular character, such as dialogs or sequence control.
In the PIE preview, the event manager handles in loading all the background levels and interpreting dialogue tracks to put a message or bustup on screen:\
*Opening the event in editor*\
*Running in PIE*

- A BMD file (message script) that contains a set of dialog messages, which are called from a dialog track in the level sequence. Named `BMD_Event_[Type]_[Major]_[Minor]_C`
- Some events will also have a script file inside of a Script subfolder, named `BF_Scr_Event_[Type]_[Major]_[Minor]_C`


### Importing Event Assets

It’s possible to import all the event assets by copying the event contents from the Zentools dump into the Unreal project in the file explorer, then duplicating the assets to uncook it. Note that especially with the level sequence, you may get a lot of missing reference warnings that appear in the editor’s Output Log due to objects that don’t exist in the project. 

We'll need to have valid references to every object that a given event's level sequence makes reference to. You can get an idea as to what objects exist in the sequence by opening the level sequence JSON in FModel and checking the list of Possessables in MovieScene_0 (type MovieScene). 

LS_Event_Cmmu_002_050_C
```json
// ...
{
	"Type": "MovieScene",
	"Name": "MovieScene_0",
	"Outer": "LS_Event_Cmmu_002_050_C",
	"Class": "UScriptClass'MovieScene'",
	"Properties": {
  	"Possessables": [
    	{
      	"Tags": [],
      	"Guid": "E0647725-49E16313-B79435B8-99E9E477",
      	"Name": "ATLUS_EventManager",
      	"PossessedObjectClass": {
        	"ObjectName": "BlueprintGeneratedClass'BP_AtlEvtEventManager_C'",
        	"ObjectPath": "/Game/Xrd777/Blueprints/Events/BP_AtlEvtEventManager.2"
      	},
      	"ParentGuid": "00000000-00000000-00000000-00000000"
    	},
    	{
      	"Tags": [],
      	"Guid": "2FD087DD-464B22EA-D5053D8B-EC4F3315",
      	"Name": "ATLUS_Camera",
      	"PossessedObjectClass": {
        	"ObjectName": "Class'CineCameraActor'",
        	"ObjectPath": "/Script/CinematicCamera"
      	},
      	"ParentGuid": "00000000-00000000-00000000-00000000"
    	},
    	{
      	"Tags": [],
      	"Guid": "7A79DB6E-47353209-9ACB50B8-F3E3F68C",
      	"Name": "CameraComponent",
      	"PossessedObjectClass": {
        	"ObjectName": "Class'CineCameraComponent'",
        	"ObjectPath": "/Script/CinematicCamera"
      	},
      	"ParentGuid": "2FD087DD-464B22EA-D5053D8B-EC4F3315"
    	},
    	{
      	"Tags": [],
      	"Guid": "FC74842C-42DE422F-A36A99A1-FF951EB1",
      	"Name": "P3 Hero",
      	"PossessedObjectClass": {
        	"ObjectName": "BlueprintGeneratedClass'BP_Event_PC0001_C'",
        	"ObjectPath": "/Game/Xrd777/Blueprints/Events/Characters/Player/Event_PC0001/BP_Event_PC0001.3"
      	},
      	"ParentGuid": "00000000-00000000-00000000-00000000"
    	}
// ...
```
*List of possessables in the Movie Scene object as seen in FModel*

The repo currently contains valid references for player character event blueprints, so these will appear in the sequencer menu in white. Any objects that don't exist in the project will appear with red text instead.

*This shows that the character blueprint for the event NPC exists, but it’s animation blueprint is currently missing.*

If the character blueprint is also missing from the project, then it’s object won’t exist in the level when it gets uncooked:

*These missing character blueprints…*\
*…don’t appear in the list of actors in the event level*

Any missing character blueprints or animation blueprint objects can be replicated by finding the possessable that matches that name, then [recreate the missing blueprint](https://github.com/Dmgvol/UE_Modding/blob/main/AdvancedModding/BpReplication.md)
The repo should contain an up to date list of the blueprints used as more events are edited, so this should only be needed when working on events that others haven’t tried before.
### Sequence Directors
### Checking an Event's References
### Importing Maps
### Importing Character Models and Animations
### Tip: Removing common prefixes/suffixes from asset names
Setting it to Suffix mode would get rid of everything starting at the first match until the end of the string, so SK_SC0101_BaseSkelton_AnmCommon_Anim_Armature_A_SC0101_CMA0001_POSE_Neutral.

If you have a large collection of cooked assets in the directory that you uncooked by duplicating all of them, the uncooked copies may likely contain a suffix like _2 on the end of their name. This can be removed by setting the Remove mode to Suffix, then the Regex to Match to _2$

[This website](https://regexr.com/) contains a live preview to test your regex expression on before running it in editor, and contains a cheat sheet to reference on how to construct new regexes.

### Character Blueprints
### Props and Field Objects
