# Sound 

Plays or stops a sound.

More specifically, this track either plays with a target Cue ID from the target sound category type or stops the player with the associated sound category type. The sound category type defines which player we’ll be doing our particular sound operation on.

## Examples

Say we want to create a sound track that starts by playing Mass Destruction -Reload-, then starts playing It’s Going Down Now, and finally stops playing any music.
I’ve also got a dialogue track that will play on the first frame, followed by a selection on frame 30 and a final message on frame 80, so I want It’s Going Down Now before the second message and for the music to stop before the last message. Checking the Amicitia Wiki’s BGM list, I’ll set the cue IDs for the music playing to 26 and 128 respectively. The flow of events would look something like this:

Such a sound track would be constructed like so in the Unreal editor’s sequencer - we’ve set the Cue ID at each keyframe, and defined it’s sound type as being GlobalBGM so that it’ll pull from the BGM cue sheet.

Which in game plays like this:

## Technical Details

- The bStopWhileHighSpeed switch controls if this sound will continue playing when the event is fast forwarded.
- Events don’t stop the music that’s already playing - you’ll either have to remove it yourself by stopping the GlobalBGM player, replace it with the music of your choice or use the Sound Fade Section to gradually fade it out.
- Internally, playing and stopping a player is a function call to `UPlayAdxControl::RequestSound` and `UPlayAdxControl::StopSound` respectively. These both accept a player handle through a major and minor ID, and each sound category is assigned to the following player handles:

| Sound Category Type | Player Major ID | Player Minor ID | Cue Sheet Filepath |
| - | - | - | - |
| GlobalBGM | 0 | 0 | `Xrd777/CriData/CueSheet/BGM/bgm`
| GlobalSE | 4 | 0 | `Xrd777/CriData/CueSheet/system`
| EventVoice | 151 | User Defined (0 to 4) | `Xrd777/CriData/CueSheet/Event/[Voice ACB]`
| EventSE | 152 | 0 | `Xrd777/CriData/CueSheet/Event/[Sound Effect ACB]`
| EventSE Copy 1 | 152 | 1 | `Xrd777/CriData/CueSheet/Event/[Sound Effect ACB]`
| EventSE Copy 2 | 152 | 2 | `Xrd777/CriData/CueSheet/Event/[Sound Effect ACB]`

- If you were to set the sound category type to User ID, you can set any arbitrary player handle, assuming it exists, to either play a sound out of it’s cue sheet or to stop it. Setting it to anything else will use that assigned player handle, so the Player Major ID and Minor ID fields won’t do anything (EventVoice’s Minor ID can be freely assigned though).\
**Other notable player handles:**

| Player Major ID | Player Minor ID | Cue Sheet Filepath | 
| - | - | - |
| 210 | 0 | `Xrd777/CriData/CueSheet/BGM/bgm (copy of GlobalBGM)`
| 3 | 0 |`Xrd777/CriData/CueSheet/SingleWord/voice_singleword `
| 211 | 0 | `Xrd777/CriData/CueSheet/Configure/voice_configue`

## C++ Structure

```c++
struct FEvtAdxSoundPayload {
public:
    struct FName EventName;
    int32 PlayerMajorID;
    int32 PlayerMinorID;
    int32 CueId;
    enum EEvtSoundCategoryType EvtSoundCategoryType;
    enum EEvtAdxTrackSoundOperationType EvtAdxSoundOperationType;
    bool bStopWhileHighSpeed;
}
```