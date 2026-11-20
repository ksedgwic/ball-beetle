# Questions and decisions

## Open

One heading per question:

    ### <the question>
    - Why it matters: what depends on the answer.
    - Options: the choices seen so far.
    - Notes: findings, with sources.

### Push the balls to the side, or pick them up?

- Why it matters: it decides the front of the robot, whether there is
  an intake and a hopper, and where the balls end up.
- Options:
  - Herd: push stray balls to a good spot at the side of the court
    (Ken).
  - Collect: a roller intake feeds a hopper, and players take balls
    from the hopper.  The assistant proposed it when Cole asked about
    picking balls up.
- Notes: both are in [ideas.md](./ideas.md).  Sources:
  [Ken's conversation][herd-1119], turn 1;
  [Cole's conversation][conv-1119], turns 1, 2 and 8.

### What is the project called?

- Why it matters: the repository, a package name and any domain or
  organisation handle use it.  Renaming is cheap while the project
  has two people.
- Working name: ball-beetle (Ken, 2026-11-19).  It is a plain
  descriptive name, used until a larger group picks the real one.  A
  dung beetle pushes a ball, and the robot may get a hard shell.
- Options from [Ken's conversation][herd-1119], turn 2, with the
  assistant's remarks:
  - Khepri: the Egyptian scarab god, who rolled the sun across the
    sky.  Short and unusual.  One of the assistant's two picks.
  - Kelpie: an Australian herding dog.  Describes herding; fairly
    rare as a software name.  The other pick.
  - Heeler, Collie: herding dogs.
  - Rollie: slang for a pill bug, which has a hard shell.  Probably
    not unique.
  - dungbot: accurate; may not be taken seriously.
  - Deuce, Let: short; likely to collide with other names.
  - Fuzzherder: descriptive; probably free.
  - Burro, Mule: a small working animal; a nod to Donkey Car.
  - Sisyphus: used by Sisyphus Industries for kinetic sand tables.
  - scarab-something (Ken).
- Notes:
  - The assistant on ball-beetle: friendly and literal, a bit generic.
  - Avoid names that start with "Tenni-": Tennibot is an existing
    product.
  - Before settling: search GitHub and PyPI for the name, and check
    the domain or organisation handle.  None of these has been
    checked.

### Use Buzz as the shared workspace?

- Why it matters: it would be where the contributors and their agents
  talk.  It needs a relay, a preview desktop app on each person's
  computer, and a private key per person.
- Options: use it, try it for a while, or work through the repository
  and direct messages only.
- Notes:
  - The survey is in [collaboration.md](./collaboration.md).
  - Ken wants to try it on this project (2026-11-19).
  - Cole would try it and has a Mac (2026-11-19).  A Mac runs the
    desktop app and a Claude agent, so the point that was open is
    closed.
  - Decided 2026-11-20 (Ken): the relay runs on a server of his with
    a public address, under an https address.  The first plan, a lab
    relay reached through ssh forwards, was dropped because the
    desktop app accepts only `wss://` and `https://` addresses.
  - Still open: whether Buzz stays in use after the trial.  The
    community's address cannot change later.

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
- Notes:
  - Out of stock at SparkFun on 2026-11-19, by the conversation.
  - Seen 2026-11-20: the list price is $399 since July 2026.
    SparkFun has it in stock at $399.00 and Amazon sells it at
    $399.00.  So it can be bought at list, and the list price is 60 %
    above the figure the estimate used.  Details and sources are in
    [hardware.md](./hardware.md).
  - Still open: buy now, or wait for the successor NVIDIA announced
    for the first half of 2027.

### How does a camera on a 60 cm mast reach the computer?

- Why it matters: the longest camera cable seen that fits the
  computer is 500 mm.
- Options: a lower mast, the computer mounted higher in the robot, or
  a kit that carries the camera signal over an HDMI cable.
- Notes: see "Camera cable" in [bom.md](./bom.md).  Nothing is
  tested.

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

### The public repository is hosted on GitHub (2026-11-19)

- Decided by Ken.
- Reason: Claude Code's cloud sessions clone from GitHub and open
  pull requests there, and the audience for this kind of project is
  on GitHub.  Buzz does not favour either host.
- Passed over: GitLab, which Ken generally prefers.
- Details: "Repository host" in
  [collaboration.md](./collaboration.md).

[conv-1119]: ./sources/2026-11-19-donkey-car-tennis-ball-robot.md
[herd-1119]: ./sources/2026-11-19-tennis-ball-herding-robot.md
