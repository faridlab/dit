# DIT — Done in Git

**Plan it. Prove it. Done in Git.**

Project management that lives in your repo, built for developers and the AI
agents working beside them. Tasks, docs, plans and API proofs are Markdown
files in git. Every change is one commit. One local binary serves the
terminal and the browser: no server to run, no account to create.

https://github.com/user-attachments/assets/9584d803-032e-406c-bd07-8b27d5a93692

<details>
<summary><b>Watch the full five-minute tour</b></summary>

https://github.com/user-attachments/assets/c0d8a26e-35c0-49c5-971b-5082a2fe9d86

</details>

This repository publishes DIT's releases: the macOS app, the command-line
binaries and the installer. It holds no source code.

## Why DIT

When AI agents and developers share a codebase, the plan usually lives in a
chat thread. Two sessions pick the same task. A screen is built on an endpoint
that doesn't exist yet. Every agent burns tokens grepping its way around the
code.

Git already gives you what a tracker builds from scratch, so DIT uses git as
the database:

| You need | Git already has it |
|---|---|
| Change history | `git log`, `git blame` |
| Who changed what | every commit has an author |
| Sync and offline work | every clone is the whole workspace |
| Review | pull requests |
| Permissions | who can push |
| Backup | every clone, again |

Issues, docs, flows and API scenarios are plain Markdown in your repository,
readable in any editor and diffable in any pull request. A local index makes
queries fast; it is rebuilt from git at any time and is never the truth.

## The core three

### `dit flow`: orchestrate people and AI agents

One plan that every session, human or agent, reads before it starts and
writes to as it works.

- **Lanes and claims.** Each work stream is a lane. `dit claim` marks a task
  as yours, and a claim expires when a session goes quiet, so nothing stays
  locked by an agent that stopped.
- **Readiness is computed, never typed.** `dit ready --lane frontend` lists
  what can start right now. `blocked_by` gates work; `fed_by` draws a handoff
  and blocks nothing.
- **The whole picture in one command.** `dit flow show` prints every issue by
  stage and lane, who holds what, and the critical path. The browser draws it
  as a diagram.
- **Nobody has to be asked.** A question on an issue lands in that lane's
  `dit inbox`.
- **Every agent reads the same rules.** `dit ai init` writes the agent guide,
  and `dit ai spec` prints it from the binary you run, so the guide never
  drifts from the tool.

```console
$ dit ready --lane frontend
#6   frontend  Place order button and confirmation screen

held until proven (blockers are through; the seam is not):
#7   frontend  Pay by card step
     needs pay-intent: not proven on local
```

### `dit morse`: prove your API seams before you build on them

"Done on the board, 404 on staging" stops here. Morse keeps API scenarios in
your docs, pinned to the OpenAPI spec they were written against.

- **Declarative chains, no scripts.** Steps call operations from the spec,
  check `expect`, and `capture` values for the next step. A scenario is a
  fence in any Markdown document, reviewed in pull requests like code.
- **Proof is recorded, and only on green.** `dit morse sync <scenario> --env
  <name>` fires the chain and, only when every step passes, records where and
  when it was proven. When the spec moves, the proof goes stale and says so.
- **Seams gate work.** Mark an issue `needs_scenarios=place-order`, and with
  `proof: required` it stays out of `dit ready` until that scenario holds on
  the environment that lane uses.
- **Run it in the browser.** Pick an environment, press Run, and see every
  step checked.
- **Safe in a shared repo.** Nothing fires on its own; requests go only to
  hosts your machine allowed; environments and secrets stay in a gitignored
  file. Bring existing work in with `dit morse import curl` or
  `dit morse import postman`.
- **Catch the 404 before it ships.** `dit code api` reads every path your
  frontend calls against the specs and lists the calls no spec describes.

### `dit code`: give agents a map, not a grep

A code map of TypeScript, Rust and Kotlin, derived from git at HEAD: who
imports a file, what it uses, how two files connect, where a question is
answered. One call, a fraction of the tokens.

```console
$ dit code users useResourceList     # who breaks if this changes
$ dit code path SerpaShell tokenStore
$ dit code where token refresh
```

Four questions an agent asks before a change, answered with grep, graphify
and `dit code` on a 6,371-file React and TypeScript app:

| Question | grep | graphify | dit code |
|---|---:|---:|---:|
| Who imports `useResourceList`? | 1,889 tokens | 1,598 tokens, 28 of 30 | **286 tokens, 30 of 30** |
| What does `PayrollRunsPage.tsx` import? | 358 tokens, unresolved | 481 tokens, 13 of 16 | **201 tokens, 16 of 16** |
| How does `SerpaShell` reach `tokenStore`? | can't answer | 37 tokens | **21 tokens** |
| Where is token refresh handled? | 5,283 tokens | 1,609 tokens | **154 tokens** |

- **The cheapest correct answer on 4 of 4 questions**, and 34× fewer tokens
  than grep on the question asked in words.
- **Never stale.** Every command brings the map up to HEAD first, in 0.15 s
  when nothing changed (graphify's update took 84 s).
- **Zero cost until asked.** No agent hook is installed, so it adds 0 tokens
  to calls that don't use it.
- **Works in any git repository**, with no setup.

<sub>Measured 2026-09-27 with dit 0.9.0. Tokens are bytes ÷ 4 for every tool;
an answer that misses part of the result cannot win, however small.</sub>

## Everything else, built in

**Work**
- A board with one column per status; drag a card and a commit lands.
- Home: what changed, what waits on you, and a one-line capture bar.
- Search in words or DQL (`status != done AND lane = frontend`), saved as views.
- Relations both ways, drawn as a graph: what an issue waits on, what waits on
  it, its epic, and every `[[#6]]` link.
- Epics with progress derived from their children.

**Plan**
- Timeline: every change from git history, and the board as it stood on any
  date or release.
- Roadmap of epics and releases.
- Gantt: drag a bar and the dates change in the file, as a commit, with the
  critical path drawn.

**Docs**
- Pages from 11 templates: BRD, PRD, SRS, FSD, business flow, TSD, data
  model, API contract, ADR, test plan and release notes.
- Edited in place in the browser, saved as Markdown commits, linked to issues.
- Images attached in plain git.

**Git-native**
- A merge driver that merges two edits to one issue field by field. Both
  changes land; only a real clash asks a person.
- `dit sync` fetches, rebases and pushes.
- `dit doctor` checks everything that silently breaks a workspace.
- Works offline. Every clone is the whole workspace.

**One binary**
- The terminal and the browser work on the same workspace at once.
- `dit ui --all` serves every workspace on your machine; on macOS a menu bar
  app starts it.
- The browser UI stays on `127.0.0.1` and opens with a per-session token.

## Quick start

```bash
mkdir my-tracker && cd my-tracker   # any git repo becomes the workspace
dit init                             # git init, merge driver, README
dit issue new "Fix the login flow" -P p1
dit list "status != done"
dit ui                               # the board, flows, docs and Morse in your browser
```

## Install

**macOS app (menu bar), with Homebrew:**

```bash
brew install --cask faridlab/tap/dit
```

This installs `DIT.app` and puts `dit` on your `PATH`.

**Command line, macOS or Linux:**

```bash
curl -fsSL https://github.com/faridlab/dit/releases/latest/download/install.sh | bash
```

The installer downloads the release binary for your platform and checks its
SHA-256 digest.

**By hand:** download the archive for your platform from the
[latest release](https://github.com/faridlab/dit/releases/latest), check it
against its `.sha256` file, and put `dit` on your `PATH`.

| Platform | Archive |
|---|---|
| macOS, Apple silicon | `dit-aarch64-apple-darwin.tar.gz` |
| macOS, Intel | `dit-x86_64-apple-darwin.tar.gz` |
| Linux, x86_64 | `dit-x86_64-unknown-linux-gnu.tar.gz` |
| Linux, arm64 | `dit-aarch64-unknown-linux-gnu.tar.gz` |
| macOS app | `DIT-macos.zip` |

## Update

```bash
dit upgrade          # the command line installed by the installer or by hand
brew upgrade --cask dit
```

---

**#DIT · #weDitit** — Plan it. Prove it. Done in Git.
