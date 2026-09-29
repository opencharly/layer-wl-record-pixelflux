# layer-wl-record-pixelflux

Desktop video recorder for the OpenCharly selkies streaming desktop, via the
pixelflux H.264 pipeline.

The `wl-record-pixelflux` candy installs the `pixelflux-record` wrapper script
under `~/.local/bin`, which drives the selkies pixelflux capture pipeline through
ffmpeg to record the desktop as H.264. It attaches to the existing capture bridge
at `/tmp/charly-capture.sock` — the same H.264 frames the browser sees over the
selkies WebSocket stream — rather than spawning a second capture instance.

## What it provides

| Property | Value |
|---|---|
| Layer / candy | `wl-record-pixelflux` |
| Script | `~/.local/bin/pixelflux-record` (mode `0755`) |
| Capture | `/tmp/charly-capture.sock` (selkies WebSocket bridge) |
| Requires | `pod-selkies`, `layer-ffmpeg`, `plugin-record` |
| Output | MP4 (H.264 video + optional AAC audio) |
| Service / port | none |

## How to use it

Compose the layer by pinning this repo in a box's `candy:` list — typically
transitively through the `selkies-desktop` metalayer:

```yaml
my-desktop-box:
  candy:
    base: fedora
    candy:
      - '@github.com/opencharly/layer-wl-record-pixelflux:v2026.247.1546'
```

Then, inside the desktop session:

```bash
pixelflux-record output.mp4                  # 30fps, video only
pixelflux-record output.mp4 --fps 60 --audio # 60fps + audio
# Stop with Ctrl-C
```

The `record:` check verb drives the same script declaratively — a `record: start`
step with `record_mode: desktop` + `record_audio: true` auto-detects
pixelflux-record.

The candy's `plan:` asserts the wrapper is installed and executable.

## Layout

- `charly.yml` — the `wl-record-pixelflux:` candy entity (the `require:` list,
  the `copy:` plan steps, the `check:` assertion) and the embedded
  `wl-record-pixelflux-skill:` skill entity.
- `pixelflux-record` — the Python wrapper copied to `~/.local/bin/`.
- `CHANGELOG/` — per-CalVer release history.
- `.github/workflows/tag-on-merge.yml` — CalVer tag + `CHANGELOG/` on merge.
- `README.md` — this user overview.

## Related

- Owning skill: `/charly-selkies:wl-record-pixelflux`
- `/charly-check:record` — the `record:` check verb (`record_mode: desktop` auto-detects pixelflux-record)
- `/charly-selkies:wl-screenshot-pixelflux` — screenshot companion (same capture bridge + singleton)
- `/charly-selkies:wf-recorder` — alternative for sway-desktop (`wlr-screencopy`)
- `/charly-selkies:selkies` — parent candy (capture bridge, WebSocket stream, `ScreenCapture` singleton)
- [`opencharly/charly`](https://github.com/opencharly/charly) — the charly CLI and image builder
- [`opencharly/opencharly`](https://github.com/opencharly/opencharly) — the umbrella
