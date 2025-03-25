# Sound Fade

Either fades in, fades out or stops the BGM. 

The time setting determines how many frames it takes for the sound transition to happen.

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