# Hardware components

One section per function the hardware has to provide.  Under it, one
entry per candidate part.

The entries filed on 2026-11-19 come from one
[conversation][conv-1119] between Cole and an AI assistant.  Two
prices were read from listings that day and are marked "listed".
Every other number is the assistant's estimate.  Nothing has been
bought or measured.

Prices and stock marked "seen 2026-11-20" were read from the sellers'
pages that day.  [bom.md](./bom.md) lists a part for each line of the
cost estimate below, with the prices seen.

## Entry form

    ### <part name>
    - Purpose: what it would do in the project.
    - Source: link to the vendor page or datasheet.
    - Cost: price, and the date it was seen.
    - Interfaces: power, connectors, buses, mounting.
    - For: reasons to use it.
    - Against: reasons not to, and unknowns.
    - Status: candidate | chosen | dropped (reason).

## Computer

### Jetson Orin Nano Super developer kit

- Purpose: run the ball detector, on the robot or in a box at the
  fence.
- Source: [SparkFun][sf-orin], [Newegg][ne-orin].
- Cost: $399 list since July 2026, by [NVIDIA's price table][nv-faq];
  $249 before that ([report of the increase][cnx-jetson]).  Seen
  2026-11-20: SparkFun $399.00, in stock; Amazon $399.00, sold by
  Amazon, delivery in 10 to 14 days; [Seeed Studio][seeed-orin]
  $449.00, one per account; Newegg out of stock.
- Earlier reading: the conversation of 2026-11-19 gives $249 list,
  SparkFun out of stock with a notice that the price may change
  because of component costs and tariffs, and one Newegg reseller at
  $718.88.
- Interfaces: two camera ports.  The power estimates assume its 15 W
  mode.
- For: estimated speed of YOLOv8n with TensorRT: 60 or more frames per
  second at a 416 px input, 25 to 30 at 1280 px, about 15 on a full
  1080p frame.  None of the three is measured.
- Against: the price is 60 % above the figure in the cost estimate
  below.  SparkFun's page says that availability may be limited at
  times.  NVIDIA announced a successor, the Jetson Orin Nano 2, for
  the first half of 2027, with no price
  ([JetsonHacks, 2026-08-26][jh-0826]).
- Status: candidate.

### Jetson Nano (original)

- Purpose: as above.  The Donkey Car has a version built on it.
- Against: about 20 frames per second, which makes steering lag
  (estimate).  NVIDIA lists the Jetson Nano developer kit as end of
  life, and the module as available through January 2027
  ([lifecycle page][nv-life], read 2026-11-20).
- Status: candidate.  The conversation moved on to the Orin Nano.

### Raspberry Pi 5

- Purpose: the robot's computer when the detector runs in a box at the
  fence.  The robot's own camera then handles only the last 2 m.
- Status: candidate, for the fence-camera design only.

## Camera

The conversation's rule of thumb: the detector needs a ball about
10 px wide, and a tennis ball is 6.7 cm across.

| Lens | Detector input | Range |
|---|---|---|
| about 120° | 416 px | about 1 m (0.8 to 1.3 m) |
| about 120° | 1280 px | about 2.5 m |
| about 62° | 1280 px (capture 1280x720) | about 7 m, perhaps 9 m after fine-tuning |
| about 62° | 1920 px (capture 1920x1080) | about 10 m |

These are calculations, not measurements.  Recomputed when filed, with
`width_px = image_width_px * 0.067 / (2 * distance_m * tan(view_angle / 2))`:
the four rows give 10 px at 0.8 m, 2.5 m, 7.1 m and 10.7 m.

### IMX219 camera module, standard lens (about 62°)

- Purpose: find balls from the robot.
- Source: [Arducam IMX219 for Jetson, at SparkFun][sf-imx219].
- Cost: $19.95, listed 2026-11-19.
- Interfaces: CSI ribbon cable.
- For: the range in the table above.
- Against: a ball 10 px wide at 10 m is at the edge of reliable
  detection.  A ball near the bumper leaves the view sooner than with
  a wide lens.
- Not checked: whether the 1280x720 and 1920x1080 sensor modes keep
  the full 62° view.  Some IMX219 modes read only the centre of the
  sensor, which narrows the view and makes a ball larger in pixels
  than the table assumes.
- Status: candidate.

### IMX219 camera module, wide lens (about 120°)

- Against: range of 1 to 2.5 m.  It was the assistant's first
  recommendation, which the assistant withdrew.  A full fisheye lens
  also squashes balls near the edges of the frame.
- Status: candidate.  The conversation moved to the 62° lens.

### 4K camera, lens of about 100° (on the fence)

- Purpose: one per end, about 4 m up on the back fence, for the
  fence-camera design.
- For: sees its half of the court, the players and the robots.
- Against: a ball near the net is only about 6 px wide.  The
  conversation proposes comparing each frame with a picture of the
  empty court and confirming with the detector.  Mounting takes more
  effort (Cole).
- Cost: no part named, no price.
- Status: candidate, for the fence-camera design only.

### Mast, about 60 cm

- Purpose: raise the robot's cameras so that distance can be read from
  how low a ball sits in the frame.
- For: at 15 cm a ball 10 m away sits almost on the horizon, and the
  distance estimate is off by metres.  At 60 cm, one metre of distance
  is about 10 px in the frame at 10 m (1920 px capture, 62° lens).
- Against: the longest camera cable seen on 2026-11-20 that fits the
  computer is 500 mm.  See "Camera cable" in [bom.md](./bom.md).
- Status: candidate.

## Chassis and drive

### Donkey Car kit

- Purpose: chassis, steering and speed controller for the Donkey Car
  software.
- Source: <https://docs.donkeycar.com/>
- Cost: $237, as quoted in the conversation.  The listing is not
  identified.
- Against: car-style steering cannot turn in place.  The stock speed
  controller may not take a 4S battery.
- Status: candidate.  The conversation's cost estimate leaves the kit
  out and keeps only the software, which is free.

### Differential-drive chassis, motors and a 4S motor driver

- Purpose: turn in place, for example along the fence.
- Cost: about $150 (estimate).  No part named.
- For: the Donkey Car software supports differential drive
  ([actuators page][dk-act], read 2026-11-20).
- Against: no kit of the right size that takes a 4S pack was seen
  near $150 on 2026-11-20.  The parts seen are in
  [bom.md](./bom.md).
- Status: candidate.

## Ball intake

### Foam roller, motor and hopper

- Purpose: a powered roller across the front bumper spins inward and
  feeds a hopper.
- Cost: about $40 for roller and motor (estimate).  The hopper is not
  priced.  No part named.
- Status: candidate.

## Battery and power

Estimated average draw.  None is measured.

| Configuration | Average draw |
|---|---|
| Robot with a Pi 5, parked most of the time | about 12 W |
| Robot with an Orin Nano in 15 W mode, two cameras, occasional driving | about 22 W |
| The same robot roaming the fence line all the time | about 30 W |
| Fence box: Orin Nano and two cameras | about 25 W |

The goal of 2 hours needs 44 Wh at 22 W and 60 Wh at 30 W.

| Pack | Energy | Run time | Cost |
|---|---|---|---|
| 3S 5000 mAh LiPo | 55 Wh | about 4 h at 12 W | not given |
| 4S 5000 mAh LiPo | 74 Wh | about 3 h at 22 W | not given |
| 4S 6000 mAh LiPo | 89 Wh | about 3 h at 30 W | about $80 (estimate) |
| 150 Wh power bank, for the fence box | 150 Wh | 5 to 6 h at 25 W | not given |

- One LiPo charger shared by the robots: about $50 (estimate).
- The drive motors run from the same pack, through a motor driver
  rated for 4S.
- The 89 Wh figure was computed when filed (14.8 V x 6 Ah).  The
  conversation gives only the run time.

## Small parts

- SSD, mast, wiring and a voltage regulator: about $60 per robot
  (estimate).

## Cost estimate

From the conversation, for robots that carry their own cameras and
roam the perimeter.

| Part, per robot | Cost |
|---|---|
| Jetson Orin Nano Super | $249 (list) |
| 2 IMX219 cameras | $40 (listed at $19.95 each) |
| Differential-drive chassis, motors, 4S motor driver | about $150 |
| Roller intake motor and foam roller | about $40 |
| 4S 6000 mAh LiPo | about $80 |
| SSD, mast, wiring, voltage regulator | about $60 |
| Per robot | about $620 |

- Two robots and one shared charger: about $1,300.
- With the Jetson at the reseller price of $718.88: about $2,200.
- With the prices seen on 2026-11-20: about $1,750 to $2,330, and
  some lines not priced.  See [bom.md](./bom.md).

[conv-1119]: ./sources/2026-11-19-donkey-car-tennis-ball-robot.md
[sf-orin]: https://www.sparkfun.com/nvidia-jetson-orin-nano-developer-kit.html
[nv-faq]: https://developer.nvidia.com/embedded/faq#jetson-prices
[cnx-jetson]: https://www.cnx-software.com/2026/07/22/nvidia-increases-the-price-of-jetson-modules-and-devkits-by-up-to-101/
[seeed-orin]: https://www.seeedstudio.com/NVIDIAr-Jetson-Orintm-Nano-Super-Developer-Kit-Bundle.html
[jh-0826]: https://jetsonhacks.com/2026/08/26/jetsonhacks-august-2026-newsletter/
[nv-life]: https://developer.nvidia.com/embedded/lifecycle
[dk-act]: https://docs.donkeycar.com/parts/actuators/
[ne-orin]: https://www.newegg.com/nvidia-jetson-orin-nano-super-developer-kit/p/N82E16813190033
[sf-imx219]: https://www.sparkfun.com/arducam-imx219-visible-light-fixed-focus-camera-module-for-nvidiar-jetson-agx-orin-orin-nano-orin-nx.html
