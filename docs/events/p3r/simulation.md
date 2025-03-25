---
sidebar_position: 3
---

# Event Simulation
## Required Assets
The in editor preview is handled by the AtlEvtEventManager blueprint, which exists inside of every event level in the game. In level sequences, this is named ATLUS_EventManager and stores tracks that aren't tied to a particular character, such as dialogs or sequence control
In order for the event to load, EvtPreDataAsset from `/Game/Xrd777/Field/Data` needs to be imported, since this contains information such as what maps to load in when calling the event. (this currently doesn't exist in the repo, but it's just a data table so it's easy to import)
Additionally, if the dialog BMD calls a bustup function, the event manager will call BustupExistDataAsset from `/Game/Xrd777/UI/Tables/` to get bustup parameter info

Both of these assets can be uncooked by duplicating it in the Unreal editor. Once this is done, the PIE preview should be able to run.

## Bustup Images

Bustup images are simple to do - for the character that you want to have a portrait of showing in the PIE preview, import all the bustup textures for that character into the editor, and you should see that bustup show up.

## Displaying Messages
The widget that displays the message text references the font FOT Skip Std B, which isn’t available in the repo. If you start the PIE preview without it, the text won’t show up properly:

You’ll have to import that font yourself into /Xrd777/Font, with the font face asset named FOT-SKIPSTD-B and the font asset named FOT-SKIPSTD-B_Font:

:::tip
This step really isn't needed, I can just use Roboto instead
:::