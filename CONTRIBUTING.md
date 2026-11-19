# Contributing

ball-beetle is a hobby project with a small group of contributors.
Contributions are licensed under the MIT license in
[LICENSE](./LICENSE).

## Workflow

- Work on a branch and open a pull request against `master`.
- Before the merge, rebase the branch onto the current `master` on
  your own machine, then merge with a merge commit.  Each merge is
  then one merge commit on top of `master`, and the merge commit
  names the pull request.
- Do not use the host's "Rebase and merge" or "Squash and merge"
  buttons.  They recreate the commits on the server, which drops
  commit signatures.

## Commit messages

The title says what changed.  The description says why the change is
correct.  Leave out testing notes, roadmap and attribution trailers.

## AI agents

The project is developed with AI coding agents, and contributions
made with them are welcome.

- The contributor is the author.  Read, understand and test what an
  agent wrote before you submit it; you answer for it in review.
- [AGENTS.md](./AGENTS.md) holds the instructions agents follow in
  this repository.  Most agent tools read it directly;
  [CLAUDE.md](./CLAUDE.md) imports it for Claude Code.
- Personal agent settings stay out of the repository.
  `CLAUDE.local.md` and `.claude/settings.local.json` are ignored.
- Commits carry no AI attribution trailers.  This section is the
  project's statement that agents are used.
- A change to `AGENTS.md`, `CLAUDE.md` or tracked files under
  `.claude/` changes how every contributor's agent behaves.  Put it
  in a commit of its own so the other contributors review it.
- Agents do not see each other's sessions, or notes kept on one
  machine.  What another contributor's agent needs has to be in the
  repository.
