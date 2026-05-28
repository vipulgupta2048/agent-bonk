# agent-bonk

A tiny **Stop hook** that plays a Mario-style pipe sound and prints a `DONE` banner the moment your agent finishes a turn. Never miss the end of a long run again — get a satisfying *bonk* instead of staring at the terminal.

```
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
          🪈   D O N E   🪈
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

## Demo

<video src="https://github.com/vipulgupta2048/agent-bonk/raw/main/demo.mp4" controls muted></video>

> If the player above doesn't load, [watch the demo here](demo.mp4).

## What's in here

```
.commandcode/
├── settings.json        # the Stop hook
└── sounds/
    └── pipe-fall.mp3     # the sound it plays
```

The layout mirrors your `$HOME/.commandcode/` directory, so installing is just a copy.

## Install (Command Code)

Copy the sound into your global Command Code config:

```bash
mkdir -p "$HOME/.commandcode/sounds"
cp .commandcode/sounds/pipe-fall.mp3 "$HOME/.commandcode/sounds/"
```

Then add the `hooks` block from [`.commandcode/settings.json`](.commandcode/settings.json) to your own `$HOME/.commandcode/settings.json` (or a project's `.commandcode/settings.local.json`). If you don't have a settings file yet, you can copy this one wholesale:

```bash
cp .commandcode/settings.json "$HOME/.commandcode/settings.json"
```

If you already have a `settings.json`, merge just the `"hooks"` key so you don't clobber your existing `permissions` and other settings.

That's it — finish a turn and you'll hear the pipe drop.

## How it works

The hook runs on the `Stop` event (when the agent finishes responding):

```json
"command": "afplay $HOME/.commandcode/sounds/pipe-fall.mp3 >/dev/null 2>&1 & disown; printf '%s' '{\"systemMessage\":\"...DONE...\"}'"
```

- `afplay ... & disown` plays the sound in the background so the agent never waits on it.
- `printf '{"systemMessage":"..."}'` returns JSON that the agent renders as the `DONE` banner.

## Use it with your own agent

Built for [Command Code](https://github.com/CommandCodeAI/commandcode), but the same hook works in any agent that supports `Stop` command hooks (e.g. Claude Code) — just point the path at `.claude/` instead and drop the JSON `systemMessage` line if that agent doesn't render it.

### Not on macOS?

`afplay` is macOS-only. Swap it for your platform's player:

| OS | Replace `afplay` with |
| --- | --- |
| Linux (PulseAudio) | `paplay` |
| Linux (ALSA) | `aplay` |
| Linux/anywhere with ffmpeg | `ffplay -nodisp -autoexit` |
| Windows (PowerShell) | `powershell -c (New-Object Media.SoundPlayer '...').PlaySync()` |

## Customize

- **Different sound?** Drop any `.mp3`/`.wav` into `sounds/` and update the path in the hook.
- **Different banner?** Edit the `systemMessage` text — change `DONE` to `BONK`, add your own ASCII art, whatever you like.

## License

MIT — do whatever you want with it.
