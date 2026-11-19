# Ideas

What the project could be and do.  An idea does not have to be
practical to be listed here; say what it is and what it would need.

The entries filed on 2026-11-19 come from one
[conversation][conv-1119] between Cole and an AI assistant.  The goals
are Cole's.  The design ideas are the assistant's proposals unless
marked "(Cole)".  None has been tested.

## Goals

What the finished thing should do.  All from Cole,
[2026-11-19][conv-1119].

- Collect tennis balls around a court while a match is played, as a
  ball boy does.
- Two machines per court.
- Stay off the court during a point, except possibly along the net.
- Tell when a point is not in play.  A ball lying on the ground in
  certain parts of the court is one sign.
- Sweep its field of view over the area around the court during play,
  by a routine that covers the area well.
- Travel the perimeter of the court, or follow a similar learned
  routine, so the run-off space is covered.
- Run 2 hours on a battery.
- See a ball about 10 m away with a camera on the machine.  That is
  far enough to see, from the net, a ball lying 6 feet short of the
  baseline.  7 m would be acceptable.
- Cameras on the machine are preferred.  A camera mounted at the court
  takes more effort.

## Ideas

One heading per idea:

    ### <idea in a few words>
    - What: one or two sentences.
    - Needs: hardware, software or knowledge it depends on.
    - Source: who proposed it, or a link.
    - Status: open | adopted | dropped (reason).

### Detect balls and steer at them, without behavior cloning

- What: a detector finds balls in the camera image, and a controller
  steers at the nearest one.  Donkey Car's usual method, copying
  recorded driving, is passed over.
- Reason given: balls lie in different places every time, so copying
  recorded laps would need hours of driving and still generalize
  badly.  It also does not pick a ball up.
- Needs: a ball detector and a steering controller; see
  [software.md](./software.md).
- Source: [conversation of 2026-11-19][conv-1119], turn 2.
- Status: open.

### Roller intake and hopper

- What: a powered foam roller across the front bumper spins inward and
  feeds a hopper.  The robot drives into the ball; there is no
  gripper.  Players take balls from the hopper.
- Needs: roller, motor, hopper.  In the last 20 to 30 cm the ball is
  below the camera's view, so the robot drives straight for about half
  a second after the tracked ball leaves the bottom of the frame.
- Source: [conversation of 2026-11-19][conv-1119], turns 2, 4 and 8.
- Status: open.

### Search by rotating at waypoints

- What: with no ball in view, rotate 360°.  If there is still none,
  drive to the next waypoint on a coarse grid (centre of each service
  box, then each baseline corner) and rotate again.  Three or four
  waypoints cover a doubles court of about 24 x 11 m.
- Needs: a way for the robot to find the waypoints.  The conversation
  does not say how.
- Against: it was proposed before the goal of staying off the court
  during a point was stated.
- Source: [conversation of 2026-11-19][conv-1119], turn 6.
- Status: open.

### Cameras on the fence, robots only fetch

- What: one 4K camera per end, about 4 m up on the back fence, with a
  lens of about 100° tilted down to cover its half of the court.  Both
  feed one computer in a box at the fence.  The cameras find the
  balls, judge whether a point is live, and track the robots by a tag
  on each robot's roof.  Each robot waits in a back corner of its end,
  against the fence, and uses its own camera only for the last 2 m.
- Reason given: a camera 15 cm off the ground is blocked by players,
  sees about 7 m, and cannot tell whether a point is live.  Most stray
  balls end up behind the baselines, so trips from a back corner are
  short.
- Needs: mounting and power at the fence, and a link from the fence
  box to the robots.  The conversation does not name the link.
- Against: mounting a camera takes more effort (Cole).
- Source: [conversation of 2026-11-19][conv-1119], turns 8 and 9.
- Status: open.  Cole prefers cameras on the machine.

### Robots wait at the net posts, cameras on a mast

- What: one robot at each net post, just outside the sideline.  Each
  covers its own side of the court lengthwise, at both ends.  It
  carries two cameras back to back on a mast about 60 cm tall, one
  facing each end, both angled about 30° in toward the court.  When
  play stops it drives out, picks up the ball and returns to its post.
- Reason given: players do not run at the net posts during points, and
  a ball 6 feet short of the baseline is about 10 m from the post.
- Needs: detection at 10 m, which the conversation puts at the edge of
  what is reliable; a computer on each robot that runs the detector.
- Source: [conversation of 2026-11-19][conv-1119], turn 10.
- Status: open.

### Perimeter lap at changeovers

- What: at each changeover (every odd game, 90 seconds) each robot
  drives one lap along the back fence on its side and collects the
  balls that rolled beyond camera range.
- Source: [conversation of 2026-11-19][conv-1119], turn 10.
- Status: open.

### Roam the perimeter during play

- What: the robots also travel the perimeter during play, to cover the
  run-off space.  (Cole)
- Needs: about twice the average power of a robot that waits, by the
  assistant's estimate (30 W against 22 W), and so a larger battery.
- Source: [conversation of 2026-11-19][conv-1119], turns 7, 11 and 12.
- Status: open.

### Play state from motion

- What: a point is live while anything on the court moves fast, such
  as a ball in flight or a player sprinting.  It is dead after 4
  seconds without fast motion; balls resting inside the lines confirm
  it.  Robots enter the court only when play is dead and leave when
  fast motion starts again.  The server bouncing the ball counts as
  fast motion.
- Needs: a camera view of the court.  It can be developed on recorded
  match video before any hardware is bought.
- Source: [conversation of 2026-11-19][conv-1119], turns 8 and 10.
  Cole's cue, a ball lying in certain parts of the court, is in
  turn 7.
- Status: open.

[conv-1119]: ./sources/2026-11-19-donkey-car-tennis-ball-robot.md
