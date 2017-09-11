# Pausing Frames

There are times for frames to be busy, and times when the work they're doing is wasteful. When they're wasteful, frames should be paused so that they don't bother the user. When a frame is paused, it completes its current tasks and resource loads but doesn't run any further tasks (e.g., event handlers, promises resolving, video loops) until the frame is unpaused. 

Pausing may be initiated by conscientious script or by the browser in order to intervene on the user's behalf. If initiated by script, it can be unpaused by script. If initiated by the browser as part of an intervention, it can only be unpaused by the browser. 

The JavaScript API to pause a frame looks like:

```javascript
var frameElement = document.getElementById("hungryFrameID");
frameElement.pause();
```

And when you're ready to unpause the frame:
```javascript
frameElement.unpause();  // does nothing if frame isn't paused
```

To detect if a frame is paused:
```javascript
if (frameElement.paused) 
  alert("The frame is paused");
```

## Use Cases
* Pausing frames that violate policies (e.g., [TransferSizePolicy](https://github.com/WICG/transfer-size)). This provides a gentle, yet firm response to misbehaving frames.
* Pausing resource-intensive frames that the user isn't currently paying attention to. For instance, a carousel of frames, where some of the frames are offscreen.
* As a mechanism for browsers to automatically intervene on resource-intensive frames.


## Pausing Details
When a frame is paused, the browser will:

1. finish any currently executing script, but the rest of the frame's event queue is deferred until the frame is unpaused. This means that queued promise resolutions and event firings won't happen until the frame has unpaused.

2. pause any playing animations or video, which will resume when the frame is unpaused

