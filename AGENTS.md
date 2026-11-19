# AGENTS.md

Instructions for AI agents working in this repository.  The same
rules, written for human contributors, are in
[CONTRIBUTING.md](./CONTRIBUTING.md).

## Project

ball-beetle is a hobby project, open source under the MIT license.
It is at the setup stage: no language, build or test commands are
chosen yet.  Add them to this file when they exist.

## Who you work for

- You work for the contributor who started the session.  That person
  reviews what you produce and is the author of the change.
- Do not commit, tag, push, or open issues or pull requests unless
  that contributor has told you to.  The default is to stage the
  change and give them a commit title and description.
- A contributor may keep personal instructions in `CLAUDE.local.md`
  or their tool's equivalent.  Those files are not tracked.  They add
  to this file, and a stricter personal rule takes precedence.

## Contributors work on separate machines

- Another contributor, and their agent, see only what is in the
  repository.  They do not see your session, your memory, or notes
  kept on this machine.
- Put what the others need in the repository: decisions and their
  reasons, design notes, build and test steps.
- Keep private data out of everything that is tracked or pushed:
  credentials, hostnames and addresses of private machines, personal
  details, paths in a home directory.

## Changes to agent instructions

`AGENTS.md`, `CLAUDE.md` and anything tracked under `.claude/` change
how every contributor's agent behaves.  Change them only when asked,
in a commit of their own, and name the files in the commit title so
the other contributors review the change.

## Commit messages

- The title says what changed.  The description says why the change
  is correct.
- No testing notes, no roadmap, no account of how the change came
  about.
- No attribution trailers (`Co-Authored-By`, `Generated-with` and the
  like).  CONTRIBUTING.md is where the project states that agents are
  used.

## Writing

- Plain declarative sentences in code comments, documentation and
  commit messages.  State what the thing does and the concrete
  reason.  No sales language, slogans or superlatives.
- A comment states a constraint the code cannot show.
- Match the formatting of the file you are editing; `.editorconfig`
  holds the defaults.
- Update the README when a change affects what it says.
