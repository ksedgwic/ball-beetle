# Questions and decisions

## Open

One heading per question:

    ### <the question>
    - Why it matters: what depends on the answer.
    - Options: the choices seen so far.
    - Notes: findings, with sources.

### Cameras on the robots or on the fence?

- Why it matters: it decides the computer on each robot, the camera,
  the battery, and whether anything has to be mounted at the court.
- Options:
  - On the robots.  Cole prefers this: mounting a camera at the court
    takes more effort.
  - On the back fence, about 4 m up.  The cameras see the whole court
    and the players, and the robots get simpler.
- Notes: both designs are in [ideas.md](./ideas.md).  Source:
  [conversation of 2026-11-19][conv-1119], turns 8 to 10.

### Where do the robots wait, and when do they move?

- Why it matters: the robots have to stay off the court during a
  point, and the choice sets how far they must see and how much power
  they use.
- Options:
  - Wait at the net posts and drive out when play stops.
  - Wait in the back corners, against the fence.
  - Travel the perimeter during play, to cover the run-off space
    (Cole).
  - One lap along the back fence at each changeover.
- Notes: the options can be combined.  Source:
  [conversation of 2026-11-19][conv-1119], turns 7 to 12.

### Can a camera on the robot detect a ball at 10 m?

- Why it matters: from a net post, a ball lying 6 feet short of the
  baseline is about 10 m away.  Cole would accept 7 m.
- Notes:
  - By calculation a ball is about 10 px wide at 10 m with a 62° lens
    and a 1920 px frame.  The conversation calls that the edge of
    reliable detection.  See the table in
    [hardware.md](./hardware.md).
  - Test proposed in the conversation: mount a Pi camera 60 cm above
    the ground, record balls at 6, 8 and 10 m on a court, and measure
    the detection rate at each distance.
  - Source: [conversation of 2026-11-19][conv-1119], turns 9 and 10.

### Does the motion rule tell live play from dead play?

- Why it matters: the robots may enter the court only when a point is
  not in play.
- Notes:
  - The rule: dead after 4 seconds without fast motion.  The 4 seconds
    are the assistant's proposal, not a measurement.
  - Cole's cue: a ball lying on the ground in certain parts of the
    court.
  - Test proposed in the conversation: run the detector on recorded
    match video, before any hardware is bought.
  - Source: [conversation of 2026-11-19][conv-1119], turns 7, 8
    and 10.

### Does a robot run 2 hours on one battery?

- Why it matters: 2 hours is a goal.
- Notes: the draw figures (12, 22 and 30 W) are estimates.  Measure
  the draw of a built robot.  See [hardware.md](./hardware.md).

### Can the Jetson Orin Nano be bought near its list price?

- Why it matters: at $249 two robots cost about $1,300.  At the
  reseller price of $718.88 they cost about $2,200.
- Notes: out of stock at SparkFun on 2026-11-19.

### Which language and project layout?

- Why it matters: the build, the tests and the directory structure
  depend on it.
- Options: none listed yet.  The component research comes first.
- Notes: Donkey Car and the Ultralytics library are Python.

## Decided

One heading per decision, newest first:

    ### <the decision> (<date>)
    - Reason: why this choice.
    - Passed over: the other options and why not.

(No entries yet.)

[conv-1119]: ./sources/2026-11-19-donkey-car-tennis-ball-robot.md
