# Donkey Car tennis ball robot

A conversation between Cole and Claude, an AI assistant, on
2026-11-19.  It is kept here as source material for the outlines.

The assistant's answers have not been checked.  The two prices it
calls current listings were looked up that day.  Its other numbers
are estimates, and nothing was tested.  The wording is unchanged; only
the line breaks differ.

## 1. Cole

Can you train https://docs.donkeycar.com/ one of these to pick up
tennis balls

## 2. Claude

I can't train it myself, since training needs your car's camera and
your driving data. I can write all the code, though, and I'd skip
Donkey's usual training method for this.

**Why not behavior cloning:** Donkey normally learns by copying how
you drive laps. Balls end up in different places every time, so you'd
need hours of demo driving and it still wouldn't generalize well. It
also doesn't solve the actual pickup, because the car has no way to
grab anything.

**What I'd build:**
- **Hardware:** Use the Jetson Nano version of the car so it can run a
  detector in real time. Mount a powered foam roller intake across the
  front bumper, spinning inward, that feeds a small hopper. A ball the
  car drives into gets sucked in, so precise grabbing isn't needed.
- **Vision:** Use YOLO (v8n or similar) for detection. "Sports ball"
  is already a COCO class, so it works on tennis balls out of the box.
  Fine-tune it with a few hundred photos from your own court to make
  it reliable.
- **Control:** Add a custom Donkey part that steers toward the closest
  detected ball (largest bounding box) with a simple proportional
  controller. It slows down as the box grows and keeps the roller
  running. When no ball is in view, it spins slowly in place to
  search.

Steering at whatever the detector sees takes about 100 lines and will
work on day one. Behavior cloning would take weeks and still miss
balls.

Want me to write the Donkey part and the YOLO fine-tuning script?

## 3. Cole

Tell me more about the vision

## 4. Claude

Here's the full vision pipeline, piece by piece.

**Camera.** Donkey's default 160x120 frames are too small to see a
ball more than a few meters away. Run the camera at 640x480 and feed
the detector a 416px input. Use a CSI camera (IMX219) with a lens
around 120° wide. A full fisheye squashes balls near the edges, and
120° is wide enough to find them without that distortion.

**Compute.** If you're buying new, get the Jetson Orin Nano. YOLOv8n
exported to TensorRT at FP16 runs at 60+ FPS on it, which leaves room
for tracking. The original Nano gets maybe 20 FPS, which works but
makes the steering laggy.

**Model.** Start from YOLOv8n pretrained on COCO, then fine-tune it as
a single "tennis_ball" class:
- Drive the car around your court and record 300 to 500 frames from
  its own camera. Your training images then match what the car sees:
  same height, angle, and motion blur.
- Get different lighting into the set: full sun, shade, late
  afternoon, and balls sitting on lines.
- Add hard negatives, meaning frames with no balls but plenty of
  things that look like one: yellow leaves, court lines, shoes, your
  ball hopper. This is what stops false chases.
- Label in Roboflow, train on Colab with Ultralytics (about an hour),
  and export a TensorRT engine for the Jetson.

**Turning detections into driving.**
- **Steering:** The ball's horizontal offset from the center of the
  frame is the steering error.
- **Distance:** The camera sits at a fixed height over flat ground, so
  a ball's distance follows from how low it sits in the frame. This is
  more reliable than box size, which jitters. Calibrate it once with a
  tape measure.
- **Target lock:** Use ByteTrack (built into Ultralytics) to give each
  ball an ID, and stay on the current target until you've collected
  it. Otherwise the car wobbles between two balls that are about
  equally close.
- **Blind spot:** In the last 20 to 30 cm, the ball drops below the
  camera's view. When the tracked ball leaves the bottom of the frame,
  drive straight for about half a second so the roller can pick it up.

Since the training frames have to come from your car, the first step
is writing a Donkey part that records frames while you drive. Want me
to start there?

## 5. Cole

How far away could it see a ball

## 6. Claude

With the setup I described, it could only see about 1 meter, which is
too short. My 416px input recommendation was a mistake.

**The math.** YOLO needs a ball to be about 10 pixels wide to detect
it reliably. A tennis ball is 6.7 cm across. With a 120° lens and a
416px input, a ball shrinks below 10 px at around 0.8 to 1.3 m. The
wide lens spreads too few pixels over too much scene.

**The fix.** Use a standard IMX219 lens (about 62° wide), capture at
1280x720, and run the detector at a 1280 input. A ball stays above 10
px out to about 7 meters, and maybe 9 m once the model is fine-tuned
on your court. YOLOv8n at 1280 should still manage around 25 to 30 FPS
on an Orin Nano with TensorRT, which is enough.

| Setup | Detection range |
|---|---|
| 120° lens, 416 input | ~1 m |
| 120° lens, 1280 input | ~2.5 m |
| 62° lens, 1280 input | ~7 m |

**Covering the court.** A doubles court is about 24 x 11 m, so even 7
m won't cover it from one spot. Give the car a search routine instead:
when nothing is in view, rotate 360°. If that still turns up nothing,
drive to the next waypoint on a coarse grid (center of each service
box, then each baseline corner) and rotate again. Three or four
waypoints cover a full court.

The narrower lens also means a ball near the bumper falls out of view
sooner. ByteTrack plus the half-second straight drive at the end
already handles that.

## 7. Cole

It would need a routine such that it's span of vision optimally sweeps
the area around the court mid play. And 2 hours battery life. It would
need to avoid walking on the court during a point except possibly
against the net. However, it could detect that a point is not in play
if it sees a ball on the ground in certain parts of the court. Two
"ball boy" machines could travel the perimeter of the court, or a
similar learned routine.

## 8. Claude

I'd move the vision off the robots and put it on the fence. A camera
15 cm off the ground gets blocked by players and only sees about 7 m.
It also can't see far enough to tell whether a point is live. Two
fence cameras can see everything, and then the robots only have to
fetch.

**Fence cameras**
- Mount one 4K camera per end, about 4 m up on the back fence, with a
  ~100° lens tilted down to cover that half from the back fence to the
  net. Both cameras feed one Orin Nano in a box at the fence.
- Run two streams per camera. A downscaled 30 FPS stream judges play
  state. Full-resolution tiles every half second find balls, because
  balls lying still don't need high frame rates.
- Balls near the net appear smallest, only about 6 px wide. To catch
  them, compare each frame against a reference shot of the empty
  court, then confirm with YOLO.
- Put an AprilTag on each robot's roof. The same cameras then track
  the robots, so the robots need no localization of their own.

**Play state**
- **Live:** anything fast-moving on court, like a ball in flight or a
  player sprinting.
- **Dead:** 4 seconds with no fast motion. Balls resting inside the
  lines confirm it.
- Robots go onto the court only when play is dead. They retreat to the
  fence the moment fast motion starts again, and the server bouncing
  the ball counts.

**Robots**
- Park instead of patrolling. Each robot waits in a back corner of its
  end, against the fence. Most stray balls end up behind the
  baselines, so trips from there are short, and parked robots save
  battery and stay out of the area where players run. Net balls get
  fetched along the net between points.
- Swap Donkey's car-style steering for a differential-drive
  (tank-style) chassis so the robot can turn in place along the fence.
  Donkey supports this.
- Use a Pi 5 instead of a Jetson. The fence box sends the robot its
  target, so the robot's own camera only handles the last 2 m, where
  the ball looks big.
- After each pickup, the robot returns to its corner, and players take
  balls from its hopper.

**Battery**
- **Robot:** a 3S 5000 mAh LiPo (55 Wh). It's parked most of the time
  and averages around 12 W, which gives roughly 4 hours.
- **Fence box:** the Orin Nano and two cameras draw about 25 W. On a
  150 Wh power bank that's 5 to 6 hours.

Everything else depends on the play-state detector, so that's the
first thing to build. I can write it to run on recorded match video,
so you can test it before buying any hardware.

## 9. Cole

The distance needed to see would be about 10 meters if the camera was
on the machine, in order to reach the ball at 6 feet short of the
baseline from the net. But 7m would be okay. Mounting a camera takes
more effort.

## 10. Claude

That works if the robots wait at the net posts. Players don't run
there during points, and from the post, a ball 6 feet short of the
baseline is about 10 m away.

**Getting to 10 m**
- Capture at 1920x1080 through the standard 62° lens. A ball is still
  about 10 px wide at 10 m, right at the edge of reliable detection.
- Run YOLOv8n on the full 1080p frame. On an Orin Nano with TensorRT,
  I'd expect roughly 15 FPS (my estimate, not tested). That's enough,
  since the balls it's hunting are lying still.
- Raise the camera on a mast about 60 cm tall. At 15 cm, a ball 10 m
  away sits almost on the horizon, and distance estimates are off by
  meters. At 60 cm you get about 10 pixels per meter of distance,
  which is accurate enough to drive to.

**Layout**
- Put one robot at each net post, just outside the sideline. Each one
  covers its own side of the court lengthwise, at both ends.
- Give each robot two cameras mounted back to back, one facing each
  end, both angled about 30° in toward the court. It sees both halves
  without turning, and the Orin Nano has two camera ports.
- The cameras also judge play state with the same rule as before. Fast
  motion means the point is live. Four seconds with no fast motion
  means it's dead, and that's when the robot drives out, picks up the
  ball, and comes back to its post.
- **Changeovers:** these come every odd game and run 90 seconds. Each
  robot uses that time for one lap along the back fence on its side,
  collecting balls that rolled beyond camera range. That lap is your
  perimeter patrol.

**Battery**
- An Orin Nano in 15 W mode, two cameras, and occasional driving
  average about 22 W, so 2 hours needs at least 44 Wh.
- Use a 4S 5000 mAh LiPo (74 Wh), which gives about 3 hours. Power the
  drive from the same pack through a motor driver rated for 4S, since
  Donkey's stock speed controller may not handle that voltage.

The first thing to test is the 10 m detection. Mount a Pi camera at 60
cm height, record a few balls at 6, 8, and 10 m on your court, and
I'll write the script that measures detection rate at each distance.

## 11. Cole

I think you'd want them roaming the perimeter as well to cover run-off
space. What's the cost.

## 12. Claude

Before answering, the assistant ran three web searches: "Jetson Orin
Nano Super developer kit price", "Donkey Car kit price" and "IMX219
camera module Jetson price".

About $1,300 for two robots, assuming you can get the Jetson at list
price.

| Part (per robot) | Cost |
|---|---|
| Jetson Orin Nano Super | $249 |
| 2x IMX219 cameras | $40 |
| Differential-drive chassis, motors, 4S motor driver | ~$150 |
| Roller intake motor and foam roller | ~$40 |
| 4S 6000 mAh LiPo | ~$80 |
| SSD, mast, wiring, voltage regulator | ~$60 |
| **Per robot** | **~$620** |

Add one shared LiPo charger (~$50), which brings two robots to about
$1,300.

**Where the numbers come from.** The Jetson's $249 retail price and
the $19.95 Arducam IMX219 are current listings. The rest are my
estimates for typical hobby parts. The Jetson is the risk: SparkFun
has it out of stock and warns price adjustments may be necessary due
to increased industry-wide component costs and tariffs, and one Newegg
reseller lists it at $718.88. At reseller prices, the total rises to
about $2,200.

**Changes for roaming.** Roaming the fence line means driving all the
time, so average power roughly doubles to about 30 W. That's why the
table has a 6000 mAh pack instead of the 5000: it gives about 3 hours,
which covers your 2 hours with margin. I also dropped the $237 Donkey
Car kit. Its car-style chassis gets replaced anyway, and you'd only
keep its software, which is free.

Pages the assistant cited for the prices:

- <https://www.sparkfun.com/nvidia-jetson-orin-nano-developer-kit.html>
- <https://www.sparkfun.com/arducam-imx219-visible-light-fixed-focus-camera-module-for-nvidiar-jetson-agx-orin-orin-nano-orin-nx.html>
- <https://www.sparkfun.com:443/products/22098>
- <https://www.newegg.com/nvidia-jetson-orin-nano-super-developer-kit/p/N82E16813190033>
