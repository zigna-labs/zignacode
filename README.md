<p align="center">

![zignacode](assets/icon-128.png)

</p>

<h1 align="center">zignacode</h1>

<p align="center">
  <b>Run your coding agents in parallel — each in its own git worktree — and review everything from one keyboard-first cockpit.</b>
</p>

<p align="center">
  English | <a href="README.ja.md">日本語</a>
</p>

<!-- TODO: screenshot or short demo video of the dashboard (DAG + cards) with agents running in parallel -->

> **Beta.** zignacode is in early beta and Windows-only for now. Things may change or break between releases.

zignacode turns one commander agent (Claude Code, Codex, OpenCode, …) into a team. You describe the work in a plan; zignacode dispatches sub-agents in parallel, each in an isolated git worktree, verifies their results, and merges them back — while you watch it happen on a live dashboard.

- **Parallel only when it pays off** — zignacode does not fan out for the sake of it. You choose how eagerly it delegates and who writes the code; small changes stay with the commander, and splitting that would not save time is flagged before anything starts. See [Parallel only when it pays off](#parallel-only-when-it-pays-off).
- **Real parallelism, zero collisions** — every sub-agent gets its own worktree and branch. No file-write conflicts, no build locks, no half-finished edits in your working copy.
- **Plan → dispatch → verify → merge** — tasks run as a dependency graph. A step with a `[verify: …]` command is merged only after the command passes; a reviewer step starts only after the steps it depends on are merged.
- **Live dashboard** — a DAG view and per-agent cards show who is working, waiting, blocked, or done, with diffs one click away.
- **Your tools, untouched** — zignacode does not write hooks, MCP servers, or settings into your global tool configuration (`~/.claude.json`, `~/.codex/`, …). Integration is attached only to agents launched from inside zignacode.
- **Terminals that survive** — close the window and your terminals (and the agents running in them) keep going. Reopen and you are back where you left off.
- **Bring your own agents** — works with the agent CLIs you already use, signed in the way you already sign in to them.
- **Small** — one installer (about 42 MB download, about 120 MB installed). No Node.js or Bun required.

## Parallel only when it pays off

Running more agents is not the same as getting done sooner. Every sub-agent costs start-up time, tokens, and a merge — so zignacode lets you decide how much to delegate, and pushes back when splitting does not help.

**In every mode, work that depends on other work, or that touches the same files, never runs in parallel.** Only steps that are independent in the plan run side by side.

- **Delegation mode** — how much to delegate and who decides on parallelism:
  - `off` — no sub-agents; the commander works alone.
  - `opportunistic` (let zignacode decide) — if running the plan in parallel would not save enough time to pay for starting the agents, zignacode runs its steps one at a time instead.
  - `orchestration` (follow the plan) — the plan's independent steps run in parallel as written, even when zignacode thinks it is not worth it (you still get the warning).
- **Task size** — work is split so that each sub-agent can finish well within its time budget, not to hit a target number of agents. There is no cap on the number of tasks; how many run at once is set by `maxWorkers`.
- **Who writes the code** — one number decides it. With delegation on, a change of up to *N* lines (30 by default, configurable; 0 means "always delegate") is written by the commander itself, and anything bigger goes to a sub-agent. With delegation `off`, the commander always writes it.
- **Auto-merge** — turn it on or off in the settings. When it is on, a step whose `[verify: …]` command passes is merged back automatically; when it is off, you merge each one yourself.
- **Plan check** — before dispatch, zignacode estimates how much time the parallel split would save. If it is less than the cost of starting the agents, it warns you (and, in `opportunistic`, runs the steps one at a time).
- **Overlap guard** — steps that touch the same files are queued one after another automatically, so parallel agents never fight over a file.
- **Dependencies** — a step marked `[after: N]` waits until the steps it depends on are done.

## Requirements

- **Windows 10 / 11 (x64)**. macOS and Linux are not supported yet.
- **git**
- **At least one agent CLI**, installed and signed in:
  - [Claude Code](https://docs.anthropic.com/en/docs/claude-code)
  - [Codex CLI](https://github.com/openai/codex)
  - [OpenCode](https://opencode.ai)
  - [pi](https://github.com/badlogic/pi-mono) <!-- TODO: confirm link -->
  - [oh-my-pi (omp)](https://github.com/can1357/oh-my-pi) <!-- TODO: confirm link -->
  - [Antigravity CLI (agy)](https://antigravity.google) <!-- TODO: confirm link -->

## Install

1. Download `Zignacode_x.y.z_x64-setup.exe` from the [latest release](../../releases/latest).
2. Run it. The beta installer is **not code-signed yet**, so Windows may show *"Windows protected your PC"*. Click **More info → Run anyway**.
3. Start **Zignacode** from the Start menu.

## First five minutes

1. Click **＋** in the left pane and pick a git repository (any drive). System folders such as `C:\Windows`, Program Files, ProgramData, AppData, drive roots and your home folder are refused on purpose.
2. In the center terminal, start your commander agent — e.g. `claude` or `codex`.
3. Ask it for something that splits into independent pieces, for example:
   *"Plan and dispatch this with zignacode: add input validation to the signup form, and in parallel write tests for the date utils. Review both when they're done."*
4. Watch the right-hand dashboard: sub-agents start side by side, each in its own worktree. Verified steps merge back automatically (auto-merge is on by default); the reviewer runs after them.
5. Open any card to read the diff, or use **merge** on a card to merge it yourself.

<!-- TODO: link to docs once a docs page exists (plan format, roles, settings, keyboard) -->

## Known limitations (beta)

- Windows only.
- The installer is unsigned (see *Install*).
- Upgrading over an older beta can leave unused files in the install folder; they are harmless.

## Feedback

Found a bug or have an idea? Please open an [issue](../../issues). <!-- TODO: decide feedback channels (Issues only, or also Discord, etc.) -->

## License

zignacode is free to use during the beta. It is proprietary software; the source code is not published.
See [LICENSE](LICENSE) for the terms. <!-- TODO: owner to write the binary license / terms of use -->
