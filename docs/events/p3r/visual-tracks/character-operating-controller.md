---
sidebar_position: 2
---

# Character Operating Controller

Allows you to override the footstep noise for the target character.

Since the path leading to the stairway is carpet, an operating controller track is added to each of the SEES members footsteps to set their footstep sounds to carpet. The concrete option is also used, though dynamic is currently unused.

## Examples

## Technical Details

## C++ Structure

```c++
struct FEvtAdxSoundFadePayload {
public:
    struct FName EventName;
    enum EEvtAdxSoundFadeType Type;
    int32 Time;
    float TargetVolume;
}
```