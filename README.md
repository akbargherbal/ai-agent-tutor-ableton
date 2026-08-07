# Ableton GUI Grounding

An AI agent that can control Ableton Live 12 on your behalf — Current scope is to make a proof of concept for an interactive AI tutor, that could give the user hands-on tutorials through OpenCode.

## How it works

There are two ways the agent reaches into Ableton:

1. **Direct UI control (primary).** The agent reads and clicks Ableton's actual on-screen controls, the same way a person would, using Windows' built-in accessibility features. After every click, it re-checks the screen to confirm the click worked.
2. **MCP (secondary).** Some things aren't reachable through the UI at all — loading a plugin from the browser, or changing a parameter deep inside a device. For those, the agent talks to Ableton through a small companion tool running inside it.

## What's in this repo

- **Automation scripts** in `scripts/` — drive Ableton.
- **Introspection scripts** — dump Ableton's entire on-screen control tree to a file, so you can search it and find the right control names.
- **`orchestrate.sh`** — runs one automation task and automatically takes a labeled screenshot of the result.
- **`take_shot.sh`** — just takes a screenshot.
- **`ABLETON_AGENT_POLICY.md`** — [PLACEHOLDER].

## Setup

- Windows 10/11, with Ableton Live 12+ open and a project loaded
- Python 3.12 (on Windows, run `python`, not `python3` — the latter is a stale 3.10 install)
- `pip install pywinauto`
- Working from WSL? Call the Windows Python directly as `python.exe`

## Trying it out

```bash
# See what Ableton's UI currently looks like, saved to a file
python dump_ableton_pywinauto.py --label baseline

# List every automation task available
python automate_ableton_task.py --list-tasks

# Dry run — prints what it *would* do, touches nothing
python automate_ableton_task.py --task arm_track --tracks 1

# For real
python automate_ableton_task.py --task arm_track --tracks 1 --live

# Same thing, but takes a labeled screenshot automatically too
./orchestrate.sh LABS/experiment arm_track --tracks 0
```

## Scope

This project focuses on getting the on-screen (UI) control path rock-solid, since it's the one whose actions can be fully verified. The MCP fills the gaps the UI can't reach.