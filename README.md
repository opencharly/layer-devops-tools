# devops-tools

Cloud and infrastructure CLI tooling for OpenCharly images — AWS CLI v2,
Scaleway CLI, OpenTofu, kubectx/kubens, and JSON/sync/DNS utilities.

The `devops-tools` candy installs the AWS CLI v2, the Scaleway CLI, OpenTofu,
and the kubectx/kubens kubectl context switchers as standalone binaries under
`/usr/local/bin`, alongside `jq`, `rsync`, `unzip`, and the per-distro DNS lookup
tools (`dig`). The cloud CLIs are declarative `download:`-verb binaries — distro
agnostic by construction — while `jq`/`rsync`/`unzip` and `dig` come from the
distro package manager. Every tool lands at a fixed path and reports its version,
so the composition is verifiable in a disposable container without any cloud
account or live cluster.

## What it provides

| Property | Value |
|---|---|
| Layer / candy | `devops-tools` |
| Requires | `layer-nodejs` (`@github.com/opencharly/layer-nodejs`) |
| Binaries | `/usr/local/bin/aws`, `/usr/local/bin/scw`, `/usr/local/bin/tofu`, `/usr/local/bin/kubectx`, `/usr/local/bin/kubens`, `/usr/bin/jq` |
| Packages | `jq`, `rsync`, `unzip`; `bind-tools`/`dnsutils`/`bind-utils` per distro (`dig`) |
| Service / port | none |

## How to use it

Compose the layer by pinning this repo in a box's `candy:` list:

```yaml
my-devops-box:
  candy:
    base: fedora
    candy:
      - '@github.com/opencharly/layer-devops-tools:v2026.243.0408'
```

Then, inside the built image:

```bash
aws --version        # aws-cli/2.x
scw version          # scaleway-cli v2.x
tofu version         # OpenTofu vX.Y.Z
kubectx --help       # context switcher
jq --version         # jq-1.x
command -v dig       # per-distro DNS lookup
```

The candy's `plan:` asserts each binary at its fixed path and runs its
version banner — a missing or non-functional tool fails the check.

## Layout

- `charly.yml` — the `devops-tools:` candy entity (the `require:` dep, the
  package + `download:` `plan:` steps, the `check:` assertions) and the embedded
  `devops-tools-skill:` skill entity.
- `package.json` — the Node.js dependency manifest the `nodejs` base consumes.
- `CHANGELOG/` — per-CalVer release notes.
- `.github/workflows/tag-on-merge.yml` — CalVer tag + `CHANGELOG/` on merge.
- `README.md` — this user overview.

## Related

- Owning skill: `/charly-coder:devops-tools`
- Node.js dependency: `/charly-coder:nodejs`
- Siblings: `/charly-coder:dev-tools`, `/charly-coder:docker-ce`
- [`opencharly/charly`](https://github.com/opencharly/charly) — the charly CLI and image builder
- [`opencharly/opencharly`](https://github.com/opencharly/opencharly) — the umbrella
