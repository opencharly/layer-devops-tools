# AGENTS.md — layer-devops-tools

Standalone candy repo for the `devops-tools` layer — cloud/infra CLIs (AWS CLI
v2, Scaleway, OpenTofu, kubectx/kubens) plus JSON/sync/DNS utilities. The candy
lives in `charly.yml` at the repo root: the `require:` dep on `layer-nodejs`,
the package + `download:` `plan:` steps, the `check:` assertions, and the
embedded `skill:` entity projected into the marketplace corpus as
`/charly-coder:devops-tools`.

Canonical files:

- `charly.yml` — the `devops-tools:` candy entity and the `devops-tools-skill:`
  skill entity.
- `package.json` — the Node.js dependency manifest.
- `CHANGELOG/` — per-CalVer release history.
- `.github/workflows/tag-on-merge.yml` — CalVer tag + `CHANGELOG/` on merge.
- `README.md` — user overview only; never agent guidance.

## Load these skills first (R0)

- `/charly-coder:devops-tools` — the owning skill. The cloud-CLI set, the
  download-verb binaries, and the per-distro DNS packages. Load before editing or
  troubleshooting the layer.
- `/charly-image:layer` — the candy authoring reference (`charly.yml` schema,
  `plan:` step verbs incl. `download:`/`check:`, per-distro `distro:` arms,
  package sections). Load before editing any entity field or plan step.

## Build / validate / test

- The merge gate is the **org-wide** `charly/pr-validator` (required check
  `validate / validate`, defined in `opencharly/.github`); this repo has **no
  per-repo candy gate** and ships only `.github/workflows/tag-on-merge.yml`.
- The candy's `plan:` `check:` steps are the functional evidence: each cloud CLI
  at its fixed path plus its version banner, and the per-distro `dig` lookup.
  They must stay valid on every distro arm they run on.
- The cloud CLIs are `download:`-verb binaries, so they are distro-agnostic; a
  version bump is a pinned URL change, and the matching `check:` version
  assertion follows it.

## Modify this repo

- Edit the `devops-tools:` candy entity AND the `devops-tools-skill:` skill
  entity in `charly.yml` together. The skill is the projected usage source, so a
  tool or version change not mirrored in the skill leaves the corpus stale.
- Package changes go in the top-level `package:` or a `distro:` arm; new
  behaviour claims belong in the `plan:` as an observable `check:` step, and in
  the skill body.

## Landing

- PR-only. Every change lands through a pull request; the org-required
  `charly/pr-validator` validates the diff and body and arms native auto-merge
  on PASS. Direct pushes to `main` are blocked.
- History lives in `CHANGELOG/` (written by `tag-on-merge` at merge time); the
  PR body IS the changelog.
- The authoritative rulebook is the umbrella `AGENTS.md` in
  `opencharly/opencharly` and `charly/AGENTS.md` in the charly repo. Do not
  restate its rules here.
