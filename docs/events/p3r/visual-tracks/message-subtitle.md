# Message Subtitle
A variation of the Dialogue track for in-engine cutscenes

## Examples

## Technical Details

## C++ Structure

```c++
struct FEvtMessageSubtitlePayload {
public:
    struct FName EventName;
    bool bUseMessage;
    int32 MessageMajorID;
    int32 MessageMinorID;
    int32 MessageSubID;
    int32 MessagePageID;
    class UBmdAsset* BmdAsset;
    bool bMessageRefAffected;
    int32 DrawFrame;
    bool bPauseSequencer;
    bool bWithSelect;
    int32 SelectID;
    int32 SelectMessageMajorID;
    int32 SelectMessageMinorID;
    int32 SelectMessageSubID;
    int32 SelectResponceToLocalDataID;
}
```