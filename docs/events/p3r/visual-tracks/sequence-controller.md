# Sequence Controller

Provides control over the sequencer's state.

Along with Time Jump Controller, these tracks are useful for creating events that have branching paths.

## Operations
- **Waiting Message** - Pauses the sequencer to wait for the message ID defined in ClosedEventMessageID to finish.
**Finish Sequencer** - Ends the sequence immediately.
**Play and Jump to Sec**
**Pause**
**View Field Camera**
**View Sequencer Camera**

## Examples

## Technical Details

## C++ Structure

```c++
struct FEvtSeqControllerPayload {
public:
    enum EEvtSeqControllerOperation Operation;
    int32 ClosedEventMessageID;
    float JumpToSec;
    float CameraBlendTime;
    int32 NextTransitionLightScenarioIndex;
}
```