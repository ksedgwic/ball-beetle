# Collaboration tools

How the contributors and their agents work together: tools for shared
work, and the repository host.  The robot's own hardware and software
are in the other outlines.

Surveyed on 2026-11-19 for Ken, from the pages linked at the end.
Nothing here has been installed or tried.

## Shared workspace

### Buzz

- Purpose: a chat workspace that people and AI agents share.  Each
  person adds their own agent to a channel as a member, and everyone
  in the channel reads what it writes.
- Source: [relay][buzz], [desktop app][buzz-app].  Block announced it
  on 2026-07-21 ([announcement][buzz-announce]).
- License: Apache-2.0, both repositories.
- Upkeep: both repositories had commits on 2026-11-19.  Desktop app
  1.0.5 is dated 2026-11-18.  The relay repository has about 37,600
  stars and about 3,900 open issues and pull requests.
- Status: candidate.  Ken wants to try it on this project
  (2026-11-19): a preview tool can be tried here without risk to
  established projects.  Cole would try it too and has a Mac, which
  runs the desktop app and a Claude agent.
- Decided 2026-11-19 (Ken): start with a lab relay on a machine of
  Ken's that is not reachable from the internet.  It is for learning
  how Buzz and its relay work.  The community on it may have to be
  started again elsewhere; see "The relay's address" below.

What it consists of:

- A relay: the server that holds one "community", which is the
  workspace.  It is a Nostr relay; every message is a signed event.
  It needs Postgres, Redis and an S3-compatible object store.
- The relay is Buzz's own program, `buzz-relay`.  A general-purpose
  Nostr relay cannot stand in for it: the Buzz relay manages the
  channels and their members itself, stores media and git
  repositories, and serves a REST interface that agents and the
  command-line client use.  The reverse works: other Nostr clients
  that support relay-based groups (NIP-29) and login (NIP-42) can
  connect to a Buzz relay (`NOSTR.md` in the relay repository).
- A desktop app for macOS, Windows and Linux (.deb and AppImage).
- An identity per person: a private key.  The app keeps it in the
  system's secure storage and does not back it up.  Each person saves
  their own copy.
- Agents.  An agent has its own key and is a member of channels like a
  person.  It runs on its owner's computer while the desktop app is
  open.  It works in `~/.buzz` by default and can run commands and
  edit files there.
- A command-line harness, `buzz-acp`, runs an agent without the
  desktop app.  By default such an agent answers only its owner
  (`--respond-to owner-only`); the owner can list other people it may
  answer.  Whether the desktop app has the same default was not
  checked.

Ways to get a relay ([guide][buzz-relay-guide]):

| Way | Cost |
|---|---|
| Hosted at buzz.xyz | The pricing page says "start for free and pay only for what you use".  A third-party article reports $0.65 per vCPU-hour; not confirmed. |
| Railway template | $8 to $15 a month, by the template pages' own estimate. |
| Docker Compose on a server you run | The server.  Relay, Postgres, Redis, object store; HTTPS through Caddy. |
| Local | None.  For trying it on one machine. |

The hosted service, as third-party write-ups report it
([pricing][buzz-pricing-report], [self-hosting][buzz-selfhost-report]).
Block's pricing page could not be read without a browser.

- Free to start, with no payment method.  A community gets an address
  under `communities.buzz.xyz`.
- Remote agents run on Block's computers, so an agent keeps running
  while its owner's computer is off: $0.65 per vCPU-hour.  On a relay
  of one's own, remote agents need a Kubernetes cluster.
- Data: 100 GB a month included, then $0.15 per GB-month.
- Direct messages are private but not end-to-end encrypted, and Block
  may read messages to operate, secure or moderate the service.
- Hosted use teaches the product.  Running a relay also teaches
  deployment, keys and members, backups, and the relay as a Nostr
  relay.

The relay's address:

- A community is tied to the address of its relay.  The relay README
  says "the URL is authoritative for the workspace".  The relay's
  settings take the public host name for the relay URL, the media URL
  and the allowed origins.
- Moving a relay to another machine under the same address is a copy
  of its data: the settings file with the relay key and the other
  secrets, the owner key, Postgres, the object store and the git
  volume (`./run.sh backup-hint` in the compose bundle).
- No documented way to change a community's address was found.  One
  write-up says a changed address gives a new, empty community.  Not
  verified.
- A community that is meant to last should start under the name it
  will keep.

Desktop requirements.  No single page of system requirements was
found; these pages state them:

- [Desktop app README][buzz-app]: download, the table of which agent
  runs on which system, and "Preview limitations".
- [Release packages][buzz-app-releases]: macOS for Apple Silicon and
  Intel, Windows x64, Linux x86-64 as `.deb` and AppImage.
- Claude and Codex agents need macOS or Linux.  Goose and Pi agents
  run on all three systems, each with a model provider's API key.
- Windows: the installer is unsigned, and agent commands need bash
  from Git for Windows ([relay README][buzz]).
- Linux: a [troubleshooting guide][buzz-linux] covers rendering
  problems.  The AppImage needs glibc 2.39 (Ubuntu 24.04, Fedora 40);
  older systems use the `.deb`.  The guide was written for the earlier
  0.5 desktop app; it does not say whether it holds for 1.0.

Alternatives for a person whose computer cannot run a Claude agent:

- Join as a person only.  The desktop app runs on Windows; only the
  Claude and Codex agents do not.
- [`buzz-acp`][buzz-acp] runs an agent without the desktop app, on
  another macOS or Linux machine.  It documents an API key.
- [`buzz`][buzz-cli], the command-line client, reads and sends
  messages.
- [Remote agents][buzz-remote] run on other hardware.  The first
  provider is Kubernetes.
- A [phone app][buzz-mobile] exists in the relay repository.  The
  relay README lists it as "being wired up".
- No web client for users was found.  `just web` in the app's
  contributing guide is a development server.

Claude as the agent:

- The desktop app's Claude harness runs on macOS and Linux, not on
  Windows.
- It downloads its own copy of Claude Code from Anthropic's release
  site, about 250 MB, and keeps it apart from the `claude` the user
  installed.
- Sign-in: if Claude Code is already set up on the computer, Buzz uses
  that setup as it is.  Otherwise the user pastes an Anthropic API
  key.
- Anthropic's terms ([legal page][claude-legal]) let a person "sign in
  to the unmodified Claude Code binary with their own Claude
  subscription", and forbid third-party developers "to offer Claude.ai
  login into their own applications, or to route requests through
  Free, Pro, Max, or Ultra plan credentials on behalf of their users".
  A person who signed in to Claude Code with their own subscription
  appears to be covered.  That is a reading; Anthropic's pages do not
  mention Buzz, and it is not confirmed.
- The same page says plan limits "assume ordinary, individual usage".
  An agent that answers in a channel uses its owner's plan allowance.
- `buzz-acp` documents an API key only.  API use is billed per token,
  apart from a subscription.
- Buzz's optional "agent protection" reviews always use an API key.

Git:

- The relay hosts git repositories, with clone and push over HTTP.
  Issues, merge handling and project pages are designed and not built
  (status table in the relay repository's `VISION_PROJECTS.md`).
- No integration with GitHub or GitLab was found in either repository.
  An agent works in an ordinary checkout, so either host works.
- People enter a community by invitation.  Whether a repository on a
  relay can be read without one was not checked.  The project needs a
  public repository in any case.

Setting it up for two people, from the READMEs:

1. One person gets a relay (hosted, Railway or own server) and creates
   the community.
2. Each person installs the desktop app, creates an identity and saves
   a copy of the private key.
3. The second person joins by an invite link.
4. Each person creates an agent in the app, with Claude as the
   harness, and signs in.
5. Create a channel, invite both agents, and mention an agent to give
   it work.

For:

- People and agents talk in one place, and the history is searchable.
- Each agent has its own identity and belongs to one owner.
- Open source and self-hostable.  No price per seat.

Against, and unknowns:

- The desktop app's README calls its state a preview: builds come
  straight from the main branch, there is no stable release channel,
  and Linux and Windows get less testing than Apple Silicon macOS.
- A relay has to be paid for or run.
- Use of a Claude subscription is not confirmed (above).
- An agent runs only while its owner's desktop app is open, on that
  computer.
- What is said in a community is not public and not in this
  repository.  Conclusions still have to be filed in `docs/`.
- Each person has to keep a private key safe.

## Repository host

The project needs a public repository.  Decided 2026-11-19 (Ken):
GitHub.  The repository is not created yet.

### GitHub

- For: Claude Code's cloud sessions (Claude Code on the web) clone
  from GitHub and open pull requests there; the
  [documentation][claude-web] names no other host.  Read from search
  results, not from the page itself.
- For: the larger audience for an open-source hobby project.  Donkey
  Car and the Ultralytics library are hosted there.
- Against: Ken generally prefers GitLab.

### GitLab

- For: Ken's general preference.
- Against: a Claude Code cloud session cannot push its work back to a
  GitLab repository, by the same search results.

Buzz does not favour either host.

[buzz]: https://github.com/block/buzz
[buzz-app]: https://github.com/block/buzz-app
[buzz-app-releases]: https://github.com/block/buzz-app/releases
[buzz-linux]: https://github.com/block/buzz/blob/main/docs/linux-rendering-troubleshooting.md
[buzz-acp]: https://github.com/block/buzz/blob/main/crates/buzz-acp/README.md
[buzz-cli]: https://github.com/block/buzz/blob/main/crates/buzz-cli/README.md
[buzz-remote]: https://github.com/block/buzz/blob/main/docs/remote-agents.md
[buzz-mobile]: https://github.com/block/buzz/blob/main/mobile/README.md
[buzz-announce]: https://block.xyz/inside/introducing-buzz-where-humans-and-agents-work-together
[buzz-relay-guide]: https://engineering.block.xyz/blog/run-your-own-buzz-relay
[buzz-pricing-report]: https://fourweekmba.com/ai-blocks-buzz-1-0-free-to-self-host-0-65-vcpu-hour-hosted/
[buzz-selfhost-report]: https://horadecodar.com.br/?p=40645
[claude-legal]: https://code.claude.com/docs/en/legal-and-compliance
[claude-web]: https://code.claude.com/docs/en/claude-code-on-the-web
