# BO2 Demo Ripper

Save Black Ops II replays as clips while you watch them, without leaving the game.

BO2 Demo Ripper runs next to **retail Steam Black Ops II**. While a replay plays
in the theater, you mark where a clip starts and ends with the number pad. The
app jumps the replay to your start point, records a preview video of just that
part, and saves the demo's files next to the video. The result is a clip folder
you can load in Redacted later for the proper cinematic render.

**Version 1.1.2**

- [Download](#download)
- [Requirements](#requirements)
- [Quick start](#quick-start)
- [Hotkeys](#hotkeys)
- [Start and end ticks](#start-and-end-ticks)
- [Jumping to the start tick](#jumping-to-the-start-tick)
- [Recording](#recording)
- [What gets saved](#what-gets-saved)
- [Troubleshooting](#troubleshooting)
- [Safety](#safety)
- [Licences](#licences)

---

## Download

Get **Bo2DemoRipper2026.exe** from the [latest release](https://github.com/Koriqzia/bo2-demo-ripper/releases/latest).
It's the whole app in one file, with nothing to install.

Put it in a folder you can write to, such as `Documents\BO2 Demo Ripper`, not
Program Files. The app keeps its settings in `DemoRipper.settings.txt` next to
the exe.

## Requirements

| | |
|---|---|
| Windows | **Windows 11** for recording with game sound. Windows 10 (1903 or later) works, but the built-in recorder records without sound there. Use OBS instead if you need sound on Windows 10. |
| .NET | .NET Framework 4.7.2 or later (already part of Windows 10 1803 and later). |
| Game | Retail **Steam** Black Ops II multiplayer (`t6mp.exe`). The memory offsets were found on the build dated 2026-08-17. |
| Display mode | **Fullscreen windowed** (borderless) is recommended. Fullscreen also recorded correctly in testing, but fullscreen BO2 minimises when you click another window. |
| Graphics | Any GPU with a hardware H.264 encoder (NVIDIA, AMD, Intel). Windows falls back to a slower software encoder without one. |

## Quick start

1. Start BO2, open **Theater**, and load a replay.
2. Start **BO2 Demo Ripper**. The top line should read *Found Retail BO2
   successfully*. The first time, pick a **library folder**. Every clip gets its
   own folder inside it.
3. Fill in the **clip name**: kills, map and details (for example `3k`,
   `express`, `toma dsrknife ballista`). The map fills itself in from the replay.
4. Back in the game, play the replay:
   - press **numpad `*`** where the clip should start,
   - press **numpad `-`** where it should end.
5. Press **numpad `/`**. The app jumps the replay back to just before your start
   point and plays. Recording starts at the start tick and stops at the end tick.
6. The clip saves itself, and you hear a high beep. Press numpad `/` again for
   the next one.

Everything in steps 4–6 works while BO2 has focus. Beeps tell you what happened:
high for a start tick, lower for an end tick, a double beep when armed, a high
beep when a clip saves, and a double low beep when something went wrong. The
status line at the bottom of the app explains what went wrong.

## Hotkeys

These work system-wide, including while BO2 has focus. Only the number-pad keys
are used, so the `/` and `-` on the main keyboard still type normally.

| Key | What it does |
|---|---|
| **Numpad `/`** | Record the clip. If recording is armed or the replay is jumping, cancels it. If recording, stops it. If a take is waiting for a name, saves it. |
| **Numpad `*`** | Set the **start tick** at the current point in the replay. |
| **Numpad `-`** | Set the **end tick** at the current point in the replay. |
| **Numpad `+`** | **Go to start tick**: jump the replay there and pause on it. Press again to stop. |

Each hotkey has a button in the app that does the same thing.

## Start and end ticks

A *tick* is BO2's replay clock in milliseconds. The app shows the live time and
tick under the time sliders, e.g. `replay 2:37 / 4:39 tick 159000 / 280900`.

- Setting a tick also moves the matching **Start** or **End** time, which is used
  in the folder name.
- An end tick before the start tick (or the other way round) clears the other
  one, and the status line says so.
- Typing a different time by hand clears that tick, so the clip is never saved
  with a tick for a moment you moved away from.
- Ticks belong to the replay they were set on. Loading a different replay
  clears them.
- **Auto End Recording at End Tick** (on by default) stops recording when the
  replay plays past the end tick.

## Jumping to the start tick

BO2's theater lets you jump through a replay in 10-second steps: **Left** jumps
back, **Right** jumps forward, and **Space** plays or pauses. BO2 Demo Ripper
presses these keys for you, even while another window has focus.

- **Jump to Start Tick before recording** (on by default): when you press
  Record with a start tick set, the app jumps to the last jump point before the
  start tick, from ahead of it or behind it. It then arms recording and presses
  play. Recording starts the moment the replay reaches the start tick.
- **Go to** (numpad `+`) jumps, plays, and pauses right on the start tick.
- **First watch of a replay:** BO2 only creates jump points as far as the replay
  has been watched. If the start tick is past that point, the app jumps as far
  as it can and plays from there. It still starts recording at the start tick,
  just after a longer wait, and the status line says why.
- The keys have to be BO2's defaults (Left, Right, Space).
- If jumps do nothing on your PC, tick **Wake BO2 before each theater key**
  under *Offsets and maintenance*.

## Recording

Open the **Recording** section to choose the recorder and its settings. Settings
are remembered.

### Built-in recorder (default)

Records the BO2 window straight to an MP4, with **only BO2's sound**. Nothing
else needs to be running, and Discord, music and notification sounds stay out
of the recording.

| Setting | Default | Notes |
|---|---|---|
| Frame rate | 60 | The file is a steady frame rate whatever BO2 is drawing. |
| H.264 profile | **Main** | Main plays everywhere. High looks a little better at the same bitrate. Baseline is the simplest and least efficient. |
| Bitrate | **Auto** | 7470 kbps up to 1080p, 12000 at 1440p, 20000 at 4K. The note under it shows BO2's size and the bitrate it gets. Untick Auto to type your own. |
| Game volume | 0 dB | Makes BO2 louder or quieter **in the recording only**. Loud moments are softened instead of distorting. Changes take effect immediately. |

Always: resolution = BO2's own size, a keyframe every 0.5 s (quick scrubbing in
editors), and AAC audio at 48 kHz, 192 kbps.

The recorder gets ready when you press Record, so recording begins within
milliseconds of the start tick. If BO2 closes while recording, the file is still
finished properly.

### OBS

Pick **OBS** to record through OBS Studio instead. In OBS, open **Tools →
WebSocket Server Settings**, tick **Enable WebSocket server**, then use **Show
Connect Info** and copy the port and password into the app. **Test connection**
checks it. OBS records with its own settings, and the file is moved into the
clip folder when OBS has finished writing it. The password is stored encrypted
for your Windows account.

## What gets saved

Each clip is a folder in your library named
`<kills> <map> <details> - <start> - <end>`, for example
`3k express toma dsrknife ballista - 243 - 304` for 2:43 to 3:04:

```
3k express toma dsrknife ballista - 243 - 304\
    tdm_mp_express_….demo              the replay
    tdm_mp_express_….demo.summary
    tdm_mp_express_….demo.tags
    tdm_mp_express_….demo.thumbnail
    3k express toma dsrknife ballista - 243 - 304.mp4    the preview
    clip.txt
```

The four demo files are what Redacted needs to load the replay. The demo is read
from BO2's memory when you press Record, so it doesn't matter what BO2 does
afterwards.

`clip.txt` lists the clip's details, one `key=value` per line:

| Key | Meaning |
|---|---|
| `clip` | folder name |
| `demo`, `map`, `mode`, `size` | the replay file, map id (e.g. `mp_express`), game mode, demo size in bytes |
| `ripped`, `source` | when it was saved, and `retail t6mp` |
| `start`, `end` | clip times as shown in the game (`2:43`) |
| `start_tick`, `end_tick`, `demo_end_tick` | exact replay ticks, when set |
| `preview` | the preview video's file name, when recorded |

**Takes.** A recording goes to `<library>\_takes\` first. It moves into the clip
folder when it's saved. If the clip has no name, or a folder with that name
already exists, the take waits and the Record button reads **Save clip**.
**Discard take** deletes a waiting take. **Rip Demo** saves the
demo files with no preview.

## Troubleshooting

**"Not connected"** — BO2 isn't running, or it's a different version. Open
*Offsets and maintenance* → **Diagnose** to see what the app finds. After a game
update, **Relocate from known demo…** recovers most offsets using a demo you
ripped before. The Tick and DemoEndTick offsets have to be found again by hand.

**"No replay tick to mark"** — a replay has to be open in the theater.

**BO2 doesn't list 4K (or your monitor's resolution)**
- If Windows is **duplicating** your screen to another monitor, BO2 only sees
  resolutions both screens support. Set the displays to **Extend** (Settings →
  System → Display).
- With display scaling above 100%, Windows reports a smaller screen to BO2. Fix:
  right-click `t6mp.exe` → Properties → Compatibility → Change high DPI settings
  → tick *Override high DPI scaling behavior* → **Application**.

**"BO2 is minimised, so there's no picture to record"** — fullscreen BO2
minimises when you click another window. Press numpad `/` from inside the game,
or play in fullscreen windowed.

**Recording is silent** — check BO2's own volume first: the recording has only
BO2's sound, at the level BO2 plays it. Raise **Game volume** if you play
quietly. On Windows 10 the built-in recorder can't capture one program's sound;
the status line says so when recording starts.

**Jumping to the start tick doesn't work**
- The theater keys have to be BO2's defaults (Left, Right, Space).
- On a replay's first watch, jump points only exist as far as you've watched.
- Try **Wake BO2 before each theater key** under *Offsets and maintenance*.
- If it still fails, the app arms anyway: jump back yourself and press play.

**"Numpad / already taken by another program"** — another app has registered
that key. Close it, or use the buttons.

## Licences

BO2 Demo Ripper is one file with its libraries compiled in, so their licence
notices travel with it. **THIRD-PARTY-NOTICES.txt** lists them in full - SharpDX,
Costura and Fody, all MIT - and is published beside the exe on every release. The
same text is inside the app, under *Offsets and maintenance* -> **Third-party
notices**, with a *Save a copy* button.

The app ships no part of OBS Studio or Black Ops II, and is not affiliated with
Activision, Treyarch or Valve.
## Safety

- **The app never changes BO2's memory.** It only *reads* it: the replay, the
  replay clock and the map. Jumping uses the same key presses you'd make
  yourself, sent to BO2's window.
- Retail BO2 uses **Valve Anti-Cheat**. Reading memory and sending keys isn't
  cheating and doesn't touch online play, but no third-party tool can guarantee
  what VAC will do. **Use it at your own risk.**
- Use it in the theater, not in online matches.

This project isn't affiliated with Activision, Treyarch or Valve.
