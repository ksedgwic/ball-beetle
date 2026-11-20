# Bill of materials (draft)

What has to be bought or made for the build in the "Cost estimate"
of [hardware.md](./hardware.md): two robots that carry their own
cameras and roam the perimeter, and one shared charger.  Where
hardware.md has a more capable and a less capable candidate, this
list takes the more capable one.

The list is loose on purpose.  A line names a function and the
candidate seen so far.  No line is chosen and nothing has been
bought.  "Open choices" below lists the decisions that change lines.

Prices and stock were read from the sellers' pages on 2026-11-20.
Several Amazon prices that day were sale prices.  Shipping and tax
are left out.  The sums were computed when filed.

The last column says how a line is obtained:

- buy: a named part, in stock at one seller or more.
- pick: parts on sale would do, but none is a clear fit yet.
- make: no part on sale does the job.  It has to be designed and
  built from stock material.

## Vision unit, hand-carried

The computer and the cameras, built first as a unit carried by hand
on a court.  It records video and runs the detector without a drive.
Ken proposed it on 2026-11-20: the vision parts are easier to get than
the mechanical ones, and much of the vision work can be tested
without a robot.  It has no mast; its height is decided later.  The
computer and the cameras move to the robot afterwards.

| Line | Candidate | Qty | Price each | How |
|---|---|---|---|---|
| Computer | Jetson Orin Nano Super developer kit | 1 | $399.00 | buy |
| Camera | Arducam IMX219, B0191 | 2 | $19.95 | buy |
| Camera cable | 15-pin to 22-pin, 15 cm, in the camera's box | 2 | included | buy |
| Storage | 128 GB microSD, SanDisk Extreme | 1 | $39.99 | buy |
| Battery | Anker 25,000 mAh 165 W power bank, A1695 | 1 | $91.19 | buy |
| Battery to computer | USB-C PD to 5.5 x 2.5 mm barrel, fixed 12 V | 1 | $7.95 | buy |
| Camera bracket and handle | none named | 1 | not priced | make |

The priced lines come to $578.03.  For setup the unit also needs a
display with a DisplayPort input or a DisplayPort to HDMI adapter, a
keyboard and a USB flash drive.  These are assumed to be on hand.

### Hardest to get, vision unit

1. Computer.  The only line with a supply risk; see "Computer" below.
2. Battery power for the computer.  The parts are in stock, but the
   combination is not proven.  See "Power from a power bank" below.
3. Storage.  Cards are in stock, but their prices are rising.

### Notes on the vision unit

- Camera cable: Arducam's [product page][arducam-b0191] lists a 15 cm
  15-to-22-pin cable and a 15 cm 22-to-22-pin cable in the box, and
  sells the module at $15.99.  SparkFun's page does not list the
  contents.
- Storage: the [SanDisk Extreme 128 GB][amz-sd128] is $39.99, sold by
  Amazon, in stock.  Memory card and flash drive prices are up about
  124 % from 2025 ([Tom's Hardware][th-flash]; only the headline was
  read).  An NVMe SSD (see "Storage" below) can be added for long
  recordings.
- Power from a power bank: the kit cannot be powered through its
  USB-C port, only through the barrel jack, 9 to 20 V
  ([NVIDIA forum][nv-usbc]).  A USB-C PD power bank therefore needs a
  trigger cable that asks it for a fixed voltage.
  [JetsonHacks][jh-vmount] suggests a 15 V cable; Adafruit's 15 V
  cable is out of stock, and its [12 V cable][ada-12v] ($7.95) is in
  stock.  Many power banks do not offer 12 V.  The
  [Anker A1695][anker-a1695] lists 12 V at 3 A (36 W) on each USB-C
  port, which is above the kit's 25 W mode.  The
  [UGREEN 25,000 mAh 145 W][amz-ugreen] is $59.98, but its output
  voltages were not read.  One user on NVIDIA's forum could not power
  the kit from a power bank ([thread][nv-bank-fail]); no report of a
  working setup was found.  The 4S LiPo of the robot build is the
  other choice.
- Camera bracket: no bracket for two of these 25 x 24 mm boards was
  found on sale.  The [Waveshare IMX219-83][ws-imx219-83] ($52.99,
  stock not shown) has two IMX219 sensors 60 mm apart on one board,
  with cables.  It would replace both cameras and the bracket, and
  fixes the spacing.
- Setup: the kit has a DisplayPort output, no HDMI
  ([user guide][nv-layout]).  From JetPack 7.2 there is no microSD
  image: an installer is written to a USB flash drive and installs to
  the microSD card or an NVMe drive ([quick start][nv-quick]).  A kit
  with firmware older than 36.0 needs a [firmware update][nv-fw]
  first; without an Ubuntu PC this is done by booting the JetPack
  5.1.3 microSD image once.  Which firmware a new kit ships with is
  not checked.

## Per robot

| Line | Candidate | Qty | Price each | How |
|---|---|---|---|---|
| Computer | Jetson Orin Nano Super developer kit | 1 | $399.00 | buy |
| Camera | Arducam IMX219, B0191 | 2 | $19.95 | buy |
| Camera cable | 22-pin to 15-pin, 500 mm | 2 | $2.50 | buy |
| Storage | 500 GB NVMe SSD, M.2 2280 | 1 | $89.99 to $129.99 | buy |
| Battery | 4S LiPo, about 6000 mAh | 1 | $35.19 to $41.49 | buy |
| Power to the computer | fused lead from the pack, or a regulator | 1 | $44.95 for the regulator | pick |
| Drive | 2 gearmotors with encoders, wheels, caster, motor driver | 1 set | $194.71 | pick |
| Base plate and motor mounts | none named | 1 | not priced | make |
| Intake | roller and gearmotor | 1 | $58.93 | make |
| Hopper | none named | 1 | not priced | make |
| Mast, about 60 cm | none named | 1 | not priced | make |
| Wiring, connectors, switch, fuse | none named | 1 set | not priced | pick |

## Shared

| Line | Candidate | Qty | Price each | How |
|---|---|---|---|---|
| Charger | HOTA D6 Pro | 1 | $99.99 | buy |

## Cost

- The priced lines come to about $820 per robot with the cheaper
  candidates.  They come to about $1,110 with the dearer SSD and
  battery, the HDMI camera kits, the regulator and the goBILDA kit
  with the MDD10A driver.
- Two robots and the charger: about $1,750 to $2,330.  The lines
  marked "not priced" come on top.
- The [conversation's][conv-1119] estimate was about $620 per robot
  and about $1,300 for two.  The differences: the computer is $399,
  not $249; the SSD costs $90 to $130, more than the $60 allowed for
  all small parts; the drive is $195 to $372, not $150; the intake is
  $59, not $40.  The battery is cheaper: $35 to $41, not $80.

## Hardest to get

1. Intake and hopper.  No part on sale does the job.  A roller has to
   be built from a paint-roller cover or from wheels on a shaft, and
   the hopper has to be designed.
2. Chassis.  No kit of the right size that takes a 4S pack was found
   near $150.  A drive built from parts needs a base plate and motor
   mounts made for it.
3. Computer.  It can be bought today, but it is the dearest line, its
   price rose 60 % in July 2026, and the sellers limit or warn about
   stock.
4. Camera link up the mast.  The longest cable found is 500 mm, which
   is shorter than a 60 cm mast needs.

## Notes on each line

### Computer

- NVIDIA's list price is $399 ([price table][nv-faq]).  It was $249
  until July 2026 ([report][cnx-jetson]).
- [SparkFun][sf-orin]: $399.00, in stock.  The page says availability
  may be limited at times.
- [Amazon][amz-orin]: $399.00, sold by Amazon, delivery in 10 to 14
  days.
- [Seeed Studio][seeed-orin]: $449.00, one per account.
- [Newegg][ne-orin]: out of stock.
- The kit includes a wireless module and a 19 V power supply.  It
  has no SSD and no microSD card ([user guide][nv-layout], Seeed's
  page).

### Camera

- [SparkFun][sf-imx219]: $19.95, in stock.  [DigiKey][dk-imx219]:
  $21.19, 51 in stock.
- The module has a 15-pin connector.  The computer's two camera
  connectors have 22 pins ([user guide][nv-layout]), so each camera
  needs a 22-pin to 15-pin cable.
- Field of view: Arducam gives 62.2° horizontal and 48.8° vertical.
  SparkFun's page says 62.2° diagonal.  The focal length on the same
  page, 3.04 mm, and the sensor's width, 3.68 mm, give 62°
  horizontal (computed when filed), which agrees with Arducam.

### Camera cable

- [Adafruit][ada-cable]: 22-pin to 15-pin, 500 mm, $2.50, in stock.
  No longer cable of this kind was found at Adafruit, Waveshare or
  Amazon.  The 1 m and 2 m cables seen have 15 pins at both ends.
- A camera on a 60 cm mast needs more than 500 mm unless the computer
  sits high in the robot.
- Arducam sells a kit that carries the camera signal over an HDMI
  cable ([Amazon][amz-hdmi], $13.99, in stock).  Its
  [documentation][arducam-hdmi] names the Orin Nano and the IMX219 as
  supported.  One [forum report][nv-hdmi-fail] describes a different
  camera failing through such a kit with 1.3 m of cable.  Not tested
  here.

### Storage

- The carrier takes an M.2 2280 NVMe drive and a 2230 drive
  ([user guide][nv-layout]).  By the
  [carrier board specification][nv-carrier] it takes single-sided
  drives only.  Whether the drives below are
  single-sided is not checked.
- Kingston NV3 500 GB: $89.99 at [Amazon][amz-nv3] (list $137.99),
  $119.99 and $129.99 at two other sellers.
- SSD prices rose about fourfold in a year, with a further rise
  forecast ([Notebookcheck, 2026-10-25][nbc-ssd]).
- The computer also boots from a microSD card.  Price not checked.

### Battery

- [OVONIC 4S 6000 mAh][amz-ovonic]: $35.19 at Amazon, in stock, Deans
  connector.  [CNHL 4S 6200 mAh][cnhl-6200]: $41.49, EC5 connector.
- None of the 6000 mAh packs read has an XT60 connector.

### Power to the computer

- The carrier's power jack accepts 9 to 20 V
  ([carrier board specification][nv-carrier]) through a 5.5 x 2.5 mm
  barrel plug.  A 4S pack gives
  12.8 to 16.8 V, which is inside the range.
- NVIDIA staff on a 4S pack and this kit: "you can use it directly if
  the voltage level won't be out of the range during the whole usage
  process" ([forum][nv-4s]).
- If a regulator is wanted: [Pololu D36V50F9][pololu-4094], 9 V, 5 A,
  $44.95, 239 in stock.  Its 12 V version needs 13.3 V in, which a 4S
  pack does not hold to the end of its charge.

### Drive

- Built from parts, $194.71: two [Pololu 37D 30:1 gearmotors with
  encoders][pololu-4752] ($60.95 each, 41 in stock), a bracket pair
  ($11.95), two 100 mm wheels ($8.86 each) with adapters ($7.75
  each), a ball caster ($5.95) and a [Cytron MDD10A][rs-mdd10a] motor
  driver ($21.69 at RobotShop, in stock; 5 to 30 V, 10 A per
  channel).
- [goBILDA Recon chassis kit][gb-recon]: $349.99, four 12 V motors
  with encoders, 3.5 kg, no motor driver.  Its size is not on the
  page.
- [Waveshare UGV02][ws-ugv02]: $149.99 with a motor driver.  It is
  23 cm wide, carries 4 kg, and is built for a 3S pack of 18650
  cells.  Whether it runs from a 4S pack is not checked.
- Donkey Car supports differential drive
  ([actuators page][dk-act]).  Its drive types `DC_TWO_WHEEL_L298N`
  and `DC_TWO_WHEEL` name the L298N, TB6612FNG, L293D, L9110S and
  DRV8833 drivers.  The MDD10A is not named; whether it works with
  one of the two types is not checked.
- The page says a Jetson Nano has two PWM pins, and that a PCA9685
  board can supply the signals.  The Orin Nano's pins are not
  checked.

### Intake and hopper

- Roller and motor, $58.93: a 14 in paint-roller cover
  ([Walmart][wm-roller], $19.98) and a [Pololu 37D 19:1
  gearmotor][pololu-4741] (530 rpm, $38.95, 79 in stock).  The shaft,
  end plugs and bearings have to be made.
- Soft wheels stacked on a shaft need no made parts for the roller:
  [AndyMark compliant wheels][am-wheels], $8.40 to $11.00 each.
- Other builds, for comparison.  A [student report][ucf] uses a
  bought wire-cage collector that picks balls up by rolling over
  them, with no motor, at $40.99.  The [Berkeley Ballbot][ballbot]
  report describes two rollers with a 24 cm opening.  Another
  [build][definity] publishes a parts list of $1,264.22.  Only these
  figures were read from the three.

### Charger

- [HOTA D6 Pro][amz-hota]: $99.99 at Amazon, in stock, mains input.
  $149.99 at another seller.
- [SkyRC B6 neo2][amz-b6]: $51.99, in stock.  It needs a separate DC
  supply, not priced.

## Not in the conversation's list

Added when filed.  None is priced or checked.

- A cutoff or alarm that stops the pack from discharging too far.
- One battery connector type for the packs, the robots and the
  charger.
- A gamepad, to drive the robot while it records training frames.
- A PWM board between the computer and the motor driver, if the
  computer's own pins are not enough.

## Open choices that change the list

- Herd or collect (see [questions.md](./questions.md)).  Herding
  drops the intake and the hopper and adds a pusher at the front,
  which also has to be made.
- One robot first, or two.
- The computer: buy at $399 now, or wait.  NVIDIA announced a
  successor, the Jetson Orin Nano 2, for the first half of 2027, with
  no price ([JetsonHacks, 2026-08-26][jh-0826]).  A Raspberry Pi 5
  with 8 GB is $180.00 at SparkFun, in stock; hardware.md lists it
  for the fence-camera design only.
- Mast height and camera link: a lower mast, the computer mounted
  higher, or the HDMI kit.
- SSD or microSD card.
- Drive built from parts, or a chassis kit.
- The computer fed straight from the pack, or through a regulator.

## Sellers that could not be read

B&H, Arrow, Micro Center, Mouser, Arducam's own store and Cytron's
own store refused or timed out.  Their prices and stock are not
known.  A later reading the same day loaded Arducam's product page
for the camera, and B&H's page, where ordering was paused that day.

[conv-1119]: ./sources/2026-11-19-donkey-car-tennis-ball-robot.md
[nv-faq]: https://developer.nvidia.com/embedded/faq#jetson-prices
[cnx-jetson]: https://www.cnx-software.com/2026/07/22/nvidia-increases-the-price-of-jetson-modules-and-devkits-by-up-to-101/
[sf-orin]: https://www.sparkfun.com/nvidia-jetson-orin-nano-developer-kit.html
[amz-orin]: https://www.amazon.com/dp/B0BZJTQ5YP
[seeed-orin]: https://www.seeedstudio.com/NVIDIAr-Jetson-Orintm-Nano-Super-Developer-Kit-Bundle.html
[ne-orin]: https://www.newegg.com/nvidia-jetson-orin-nano-super-developer-kit/p/N82E16813190033
[nv-layout]: https://docs.nvidia.com/jetson/orin-nano-devkit/user-guide/latest/hardware_layout.html
[jh-0826]: https://jetsonhacks.com/2026/08/26/jetsonhacks-august-2026-newsletter/
[sf-imx219]: https://www.sparkfun.com/arducam-imx219-visible-light-fixed-focus-camera-module-for-nvidiar-jetson-agx-orin-orin-nano-orin-nx.html
[dk-imx219]: https://www.digikey.com/en/products/detail/sparkfun-electronics/SEN-28170/26769976
[ada-cable]: https://www.adafruit.com/product/5820
[amz-hdmi]: https://www.amazon.com/dp/B06XDNBM63
[arducam-hdmi]: https://docs.arducam.com/Camera-Extension-Solution/HDMI-Extension-Kit/
[nv-hdmi-fail]: https://forums.developer.nvidia.com/t/imx477-dual-camera-fails-through-csi-to-hdmi-extender-nvbufsurfacefromfd-failed-works-fine-with-short-cable-how-to-lower-csi-signal-rate/373963
[amz-nv3]: https://www.amazon.com/dp/B0DBR9RZLV
[nbc-ssd]: https://www.notebookcheck.net/SSD-prices-explode-by-303-in-a-year-as-AI-fuels-NAND-flash-shortage.1433261.0.html
[amz-ovonic]: https://www.amazon.com/dp/B07JQH775P
[cnhl-6200]: https://chinahobbyline.com/products/cnhl-racing-series-6200mah-14-8v-4s-90c-lipo-battery-with-ec5-plug
[nv-carrier]: https://developer.nvidia.com/downloads/assets/embedded/secure/jetson/orin_nano/docs/jetson_orin_nano_devkit_carrier_board_specification_sp.pdf
[nv-4s]: https://forums.developer.nvidia.com/t/powering-jetson-orin-nano-from-lipo-battery/294257
[pololu-4094]: https://www.pololu.com/product/4094
[pololu-4752]: https://www.pololu.com/product/4752
[rs-mdd10a]: https://www.robotshop.com/products/cytron-10a-5-30v-dual-channel-dc-motor-driver
[gb-recon]: https://www.gobilda.com/recon-chassis-kit/
[ws-ugv02]: https://www.waveshare.com/ugv02.htm?sku=25076
[dk-act]: https://docs.donkeycar.com/parts/actuators/
[wm-roller]: https://www.walmart.com/ip/Wooster-Brush-RR642-14-Paint-Roller-Cover-14-Inch/846410358
[pololu-4741]: https://www.pololu.com/product/4741
[am-wheels]: https://www.andymark.com/products/compliant-wheels
[ucf]: https://www.ece.ucf.edu/seniordesign/su2023fa2023/g10/files/SD2%20120%20PAGE.pdf
[ballbot]: https://www2.eecs.berkeley.edu/Pubs/TechRpts/2012/EECS-2012-157.pdf
[definity]: https://github.com/kwong09/definity-robot-project
[amz-hota]: https://www.amazon.com/dp/B0BCJRDSRG
[amz-b6]: https://www.amazon.com/dp/B0H3TTSHNQ
[arducam-b0191]: https://www.arducam.com/b0191-arducam-imx219-visible-light-fixed-focus-camera-module-nvidia-jetson-nano-raspberry-pi-compute-module.html
[amz-sd128]: https://www.amazon.com/dp/B0G8LLXFJH
[th-flash]: https://www.tomshardware.com/pc-components/storage/memory-cards-and-flash-drives-prices-rocket-124-percent-some-products-peak-at-261-percent-jump-increases-from-2025-driven-by-ai-chip-shortage-across-a-range-of-formats-and-capacities
[nv-usbc]: https://forums.developer.nvidia.com/t/powering-the-nano-orin/366665
[jh-vmount]: https://jetsonhacks.com/2024/05/23/v-mount-battery-to-power-nvidia-jetson-electronics-projects/
[ada-12v]: https://www.adafruit.com/product/5450
[anker-a1695]: https://www.anker.com/products/a1695-anker-power-bank-25000mah-165w
[amz-ugreen]: https://www.amazon.com/dp/B0BJQ7F16T
[nv-bank-fail]: https://forums.developer.nvidia.com/t/unable-to-use-power-bank-for-jetson-orin-nano/255939
[ws-imx219-83]: https://www.waveshare.com/imx219-83-stereo-camera.htm
[nv-quick]: https://docs.nvidia.com/jetson/orin-nano-devkit/user-guide/latest/quick_start.html
[nv-fw]: https://docs.nvidia.com/jetson/orin-nano-devkit/user-guide/latest/update_firmware.html
