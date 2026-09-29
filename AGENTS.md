# AGENTS.md — layer-wl-record-pixelflux

Standalone candy repo for the `wl-record-pixelflux` layer — the `pixelflux-record`
desktop video recorder for the selkies streaming desktop. The candy lives in
`charly.yml` at the repo root: the `require:` list, the `copy:` plan steps, the
`check:` assertion, and the embedded `skill:` entity projected into the
marketplace corpus as `/charly-selkies:wl-record-pixelflux`.

Canonical files:

- `charly.yml` — the `wl-record-pixelflux:` candy entity and the
  `wl-record-pixelflux-skill:` skill entity.
- `pixelflux-record` — the Python wrapper copied to `~/.local/bin/pixelflux-record`.
- `CHANGELOG/` — per-CalVer release history.
- `.github/workflows/tag-on-merge.yml` — CalVer tag + `CHANGELOG/` on merge.
- `README.md` — user overview only; never agent guidance.

## Load these skills first (R0)

- `/charly-selkies:wl-record-pixelflux` — the owning skill. The capture-bridge
  socket protocol, the ffmpeg muxing, and the `ScreenCapture` singleton
  invariant. Load before editing or troubleshooting the layer.
- `/charly-image:layer` — the candy authoring reference (`charly.yml` schema,
  `plan:` step verbs incl. `check:`, per-distro `distro:` arms, package/repo
  sections, service declarations). Load before editing any entity field or plan
  step.

## Build / validate / test

- The merge gate is the **org-wide** `charly/pr-validator` (required check
  `validate / validate`, defined in `opencharly/.github`); this repo has **no
  per-repo candy gate** and ships only `.github/workflows/tag-on-merge.yml`.
- The candy's `plan:` `check:` step is the functional evidence — the wrapper
  installed and executable at mode `0755`.
- The `require:` on `plugin-record` is load-bearing: without it the `record:`
  verb is not build-connected and a baked check SKIPs with `unknown verb
  "record"`. Keep the pin in step with the plugin.

## Modify this repo

- Edit the `wl-record-pixelflux:` candy entity AND the
  `wl-record-pixelflux-skill:` skill entity in `charly.yml` together. The skill
  is the projected usage source, so a behaviour change not mirrored in the skill
  leaves the corpus stale.
- A change to the wrapper belongs in `pixelflux-record`; the `copy:`/`check:`
  path and mode move with it. It must never spawn its own capture — the selkies
  `ScreenCapture` is a process-wide singleton.

## Landing

- PR-only. Every change lands through a pull request; the org-required
  `charly/pr-validator` validates the diff and body and arms native auto-merge on
  PASS. Direct pushes to `main` are blocked.
- History lives in `CHANGELOG/` (written by `tag-on-merge` at merge time); the PR
  body IS the changelog.
- The authoritative rulebook is the umbrella `AGENTS.md` in
  `opencharly/opencharly` and `charly/AGENTS.md` in the charly repo. Do not
  restate its rules here.
