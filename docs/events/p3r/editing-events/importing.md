# Importing Assets

Importing an existing game's event.

It’s possible to import all the event assets by copying the event contents from the Zentools dump into the Unreal project in the file explorer, then duplicating the assets to uncook it. Note that especially with the level sequence, you may get a lot of missing reference warnings that appear in the editor’s Output Log due to objects that don’t exist in the project. 

## Determining Object References

**We'll need to have valid references to every object** that a given event's level sequence makes reference to. You can get an idea as to what objects exist in the sequence by opening the level sequence JSON in FModel and checking the **list of Possessables in MovieScene_0** (type MovieScene). 

```json
// From LS_Event_Cmmu_002_050_C
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

The repo currently contains valid references for player character event blueprints, so these will appear in the sequencer menu in white. Any objects that don't exist in the project will appear with red text instead.

If the character blueprint is also missing from the project, then it’s object won’t exist in the level when it gets uncooked:

Any missing character blueprints or animation blueprint objects can be replicated by finding the possessable that matches that name, then recreate the missing blueprint
The repo should contain an up to date list of the blueprints used as more events are edited, so this should only be needed when working on events that others haven’t tried before.

## Sequence Directors

Events that contain sequence director blueprints (you’ll be able to identify one if a cooked level sequence also spawns in a SequenceDirector blueprint) can't be directly re-exported to run in game since cooked blueprints suck. If you try to delete a level with sequence director in the editor it crashes. The only way to get around this at the moment would be to replicate the sequence director blueprint. If you see an even that uses a sequence director, please tell me.

## Importing Maps

My workflow for importing maps (e.g I wanted to import the map for Iwatodai Dorm) consists of importing the textures in first, then copying all the material instances from the Zentools dump into the project through the file explorer, then duplicating to uncook them and deleting the cooked copy. There’ll be a lot of these MIs, which when duplicated at once will have a common suffix to their name, so it’s recommended to follow this guide to be able to remove that suffix from it.

Then, import the static meshes in the same way as the material instances, however I usually don’t try to uncook these since I don’t need to make edits to these and they’ll show up in their respective level as is.

As for the map’s level asset itself - Most maps, in particular maps that the player can normally access in game will contain a level script blueprint that makes it impossible to copy and uncook in the editor without it crashing. In that case, you can create a new level with the original levels actors by opening the map asset in FModel, export it as a JSON, then in the Unreal Editor, right clicking on the folder that the map belongs in and selecting Add Level from JSON.

:::note
This isn’t a technique to make custom maps that can be played in game - my process doesn’t load in some types of level assets. This is just for the purposes of having the map show in the PIE preview.
TODO: Correct this? I've done this before?
:::

## Importing Character Models and Animations

Export the character’s "BaseSkelton" mesh - this can be done in FModel by right clicking on the target character's BaseSkelton mesh and clicking Save Model, which will export a PSK.

My personal setup involves using Blender 4.1 and a version of the [io_psa_psk_psa blender plugin](https://github.com/DarklightGames/io_scene_psk_psa/issues/55#issuecomment-2028908831) that supports multiple file drag and drop so you can drag all the animation files into the mesh at once (from reports I’ve seen, this isn’t a feature in the main build of the plugin? The issue for it’s still open so I guess not lol).
Import the BaseSkelton PSK, which will import an armature named after the SK’s filename. The scale of these models is 100x Blender's scale, so if the preview looks odd at first, zoom out and you should be able to see the shape of the character from the bones.
In the scene collection, the armature should be renamed to Armature, so that when it’s imported into Unreal, it’s not treated as the root bone, which would make it incompatible with SKEL_HUMAN.

Then open the action editor by selecting Dope Sheet, then Action Editor.

The PSAs will be imported as a collection of actions, and you'll be able to see each one in this menu:

Select an animation to preview in the viewport. From my experience, this is needed for the FBX exporter to export the animations, otherwise it'll just export the armature with no anims.

Export the mesh to an FBX with the following settings - Change Up in transform to Z Up to match Unreal’s coordinate system, and turn off Add Leaf Bones to stop Blender’s exporter making extra bones, which would again be problematic for Unreal’s importer.

Drag the exported FBX into the Unreal editor at an appropriate location for the animations. When importing a skeletal mesh with animations, each animation will be added as a new animation sequence asset.
In the skeletal mesh import menu, set the target skeleton to SKEL_HUMAN. 

:::note
(I’ll need to do further testing on this, but trying to import Aigis anims prompted me to make new bones for SKEL_HUMAN. I assume that the skeleton’s supposed to have every bone in it as is for human characters?)
Additionally, some characters may use other skeletons, such as Koromaru using SKEL_ANIMAL. You can check which skeleton a skeletal mesh targets by looking at the Skeleton property for the BaseSkelton in FModel.
:::

## Costume, Face and Hair Meshes

The process is similar to what was described above, it’s just that you won’t be including any animations with it. So for example with PC0002_C002, one of Yukari’s costume meshes, it’s just a matter of importing the costume mesh as a PSK, renaming the Armature to Armature, then exporting it as an FBX with the export settings shown above

:::tip[Removing common prefixes/suffixes from asset names]
In some situations, such as when duplicating many cooked material instances to uncook them or when importing a skeletal mesh with animations, you can end up in a situation where the file name has a bunch of stuff in front or at the end of the filename that makes it not match the in game filename:

If you right click on an empty part of the folder, an option will appear to Remove prefix/suffix by regex

Which will spawn the following menu, allowing you to remove any part of the filename before or after the matching regex for all files in that folder

In this example, I want to get rid of SK_SC0101_BaseSkelton_AnmCommon_Anim_Armature_ from the filenames of all these assets, so I set the remove mode to Prefix and the matching regex to A_\w{2}\d{4} , where \w{2} matches two of any letter and \d{4} matches 4 of any number. The regex will look for the first result that matches those requirements, which in this string will be `SK_SC0101_BaseSkelton_AnmCommon_Anim_Armature_A_SC0101_CMA0001_POSE_Neutral` 
and get rid of anything before it, so `SK_SC0101_BaseSkelton_AnmCommon_Anim_Armature_A_SC0101_CMA0001_POSE_Neutral`, 
where the red text is removed:

 Setting it to Suffix mode would get rid of everything starting at the first match until the end of the string, so SK_SC0101_BaseSkelton_AnmCommon_Anim_Armature_A_SC0101_CMA0001_POSE_Neutral.

If you have a large collection of cooked assets in the directory that you uncooked by duplicating all of them, the uncooked copies may likely contain a suffix like _2 on the end of their name. This can be removed by setting the Remove mode to Suffix, then the Regex to Match to _2$

This website contains a live preview to test your regex expression on before running it in editor, and contains a cheat sheet to reference on how to construct new regexes.
:::

## Character Blueprints

## Props and Field Objects