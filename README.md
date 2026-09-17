# termbrowse

Browse the web inside a terminal, on a phone.

Same idea as [Carbonyl](https://github.com/fathyb/carbonyl), different plumbing.
Carbonyl is a Chromium *fork* that renders half-blocks from inside the
compositor; building it for Android/bionic is a ~100GB, many-hour proposition,
and its prebuilt binaries are glibc-only. Termux ships a stock bionic Chromium,
so termbrowse drives that over the DevTools Protocol instead:

```
headless Chromium --(Page.startScreencast)--> JPEG frames
                                                  |
                                                chafa
                                                  |
                                     truecolor half-block cells
```

Input goes back the other way as `Input.dispatchKeyEvent` /
`dispatchMouseEvent`, so links are clickable — tap where you see them.

## Requirements

```sh
pkg install python chromium chafa
pip install websocket-client
```

A terminal with truecolor and, ideally, the Symbols-for-Legacy-Computing or
Octant block ranges; termbrowse probes the font and falls back to plain half
blocks when they are missing.

## Usage

```sh
./termbrowse [url]          # defaults to lite.duckduckgo.com
```

| key | action |
| --- | --- |
| `ctrl-o` | open a URL |
| `ctrl-r` | reload |
| `ctrl-h` / `ctrl-l` | back / forward |
| arrows, pgup/pgdn | scroll |
| tap | click |
| `ctrl-q` | quit |

## Environment

- `TERMBROWSE_ZOOM` — above 1 trades visible page area for bigger text (default `1`)
- `TERMBROWSE_KEEP=1` — leave Chromium running on `:9222` after exit

## Notes

Chromium is launched headless with a viewport sized from the terminal grid, at
8x16 device pixels per cell. Half-block output gives one colour per half cell,
so a 50x20 terminal is really 50x40 pixels of page — fine for logos and layout,
useless for body text, which is why the default start page is a lite one.
