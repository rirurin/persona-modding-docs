# Character Prop Attach

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