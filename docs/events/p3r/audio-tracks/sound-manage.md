# Sound Manage

## Examples

## Technical Details

## C++ Structure

```c++
struct FEvtAdxSoundManagePayload {
public:
    struct FName EventName;
    enum EEvtSoundManageOperationType EvtAdxSoundManageOperationType;
    int32 Month;
    int32 Day;
    enum ECldTimeZone TimeZone;
    uint32 ControlId;
    float ControlValue;
    int32 ControlFrame;
}