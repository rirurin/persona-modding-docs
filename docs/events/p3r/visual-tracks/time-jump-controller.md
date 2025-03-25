# Time Jump Controller

Used to jump between points in the sequencer.

Time jump controllers always(?) have two keyframes - the source frame to jump off from and the destination frame to go to.

## Operations
**Time Jump**
**Time Jump Marker**
**Skip and Time Jump**


## Examples

## Technical Details

## C++ Structure

```c++
struct FEvtSeqTimeJumpControllerPayload {
public:
    enum EEvtSeqTimeJumpControllerOperation Operation;
}

```