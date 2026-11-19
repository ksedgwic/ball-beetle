# Software components

One section per function the software has to provide.  Under it, one
entry per candidate library, framework or tool.

The entries filed on 2026-11-19 come from one
[conversation][conv-1119] between Cole and an AI assistant.  The
conversation gives no links for the software and says nothing about
licenses or upkeep.  Those fields read "not checked" until someone
looks.

## Entry form

    ### <name>
    - Purpose: what it would do in the project.
    - Source: link to the repository or documentation.
    - License: and whether it can be combined with this project's MIT
      license.
    - Platform: language, operating system, hardware it runs on.
    - Upkeep: last release, and whether it is maintained.
    - For: reasons to use it.
    - Against: reasons not to, and unknowns.
    - Status: candidate | chosen | dropped (reason).

## Robot framework

### Donkey Car

- Purpose: the software that runs the robot.  The camera, the drive
  and each custom step are "parts" of a Donkey Car program.
- Source: <https://docs.donkeycar.com/>
- License: not checked.
- Platform: Python.  The car is built on a Raspberry Pi or a Jetson
  Nano.
- Upkeep: not checked.
- For: free.  Ken also names it as a possible base
  ([2026-11-19][herd-1119]).  Custom parts can record camera frames
  while a person drives, and steer at a detected ball.  It supports
  differential drive, by Cole's conversation; not checked.
- Against: its usual training method, behavior cloning, copies
  recorded driving and does not fit this task (see
  [ideas.md](./ideas.md)).  Its default camera frames of 160x120 are
  too small to show a ball more than a few metres away.
- Status: candidate.

## Ball detection

### YOLOv8n (Ultralytics)

- Purpose: find tennis balls in camera frames.
- Source: no link in the conversation.
- License: not checked.  The filer recalls that the Ultralytics code
  is under AGPL-3.0.  If so, that has to be weighed against this
  project's MIT license before the library is adopted.
- Platform: Python.  On a Jetson the model is exported to a TensorRT
  engine with 16-bit floats.
- Upkeep: not checked.
- For: the model pretrained on the COCO data set has a "sports ball"
  class, so it detects tennis balls without training.  It can be
  fine-tuned to a single "tennis_ball" class.
- Against: by the conversation's rule of thumb a ball has to be about
  10 px wide to be detected reliably.  That limits the range; see
  [hardware.md](./hardware.md).
- Status: candidate.

Training plan from the conversation:

- Record 300 to 500 frames from the robot's own camera on the court,
  so the training images match what the robot sees: the same height,
  angle and motion blur.  The first step is a Donkey Car part that
  records frames while a person drives.
- Vary the light: full sun, shade, late afternoon.  Include balls
  lying on lines.
- Add frames without balls that show things easily mistaken for one:
  yellow leaves, court lines, shoes, a ball hopper.
- Label the frames, train for about an hour, export the engine.

### Tools named for training and export

- Roboflow: labeling the frames.  A hosted service; terms not checked.
- Google Colab: training with the Ultralytics library.  A hosted
  service; terms not checked.
- TensorRT: the engine format the model runs in on a Jetson.  License
  not checked.
- Status of all three: candidate.

## Tracking

### ByteTrack

- Purpose: give each detected ball an identity from frame to frame, so
  the robot stays on one ball until it has collected it.  Without that
  the robot wavers between two balls that are about equally close.
- Source: built into the Ultralytics library, by the conversation.
- License: not checked.
- Status: candidate.

## Robot position

### AprilTag

- Purpose: a printed tag on each robot's roof.  Cameras on the fence
  find the tags, so the robots need no position finding of their own.
- License: not checked.
- Status: candidate, for the fence-camera design only.

Not covered in the conversation: how a robot that carries its own
cameras knows where it is on the court.  The waypoint search, the
return to a net post and the perimeter lap all need that.

## Play-state detection

No library is named.  The rule is in [ideas.md](./ideas.md), "Play
state from motion".  For the fence-camera design the conversation
adds:

- Two streams per camera.  A reduced stream at 30 frames per second
  judges play state.  Full-resolution tiles every half second find the
  balls; balls lying still do not need a high frame rate.
- Balls near the net are about 6 px wide.  Compare each frame with a
  picture of the empty court, then confirm with the detector.

## Driving at a ball

A custom Donkey Car part, about 100 lines by the assistant's estimate.

- Steering: the ball's horizontal offset from the centre of the frame
  is the steering error, fed to a proportional controller.
- Target: the closest ball.  The first answer takes the largest
  bounding box; the later one uses the distance estimate and the
  tracker.
- Distance: the camera is at a fixed height over flat ground, so a
  ball's distance follows from how low it sits in the frame.  That is
  steadier than the size of the bounding box.  Calibrate once with a
  tape measure.
- Speed: slow down as the ball gets close.  Keep the roller running.
- No ball in view: turn slowly in place to search.
- Last 20 to 30 cm: the ball is below the camera's view.  When the
  tracked ball leaves the bottom of the frame, drive straight for
  about half a second.

[conv-1119]: ./sources/2026-11-19-donkey-car-tennis-ball-robot.md
[herd-1119]: ./sources/2026-11-19-tennis-ball-herding-robot.md
