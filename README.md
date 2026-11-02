<div align="center">

<img src="snap/gui/cline.png" alt="Cline logo" width="128" />

# Cline

**Autonomous coding agent for your terminal**

[![cline](https://snapcraft.io/cline/badge.svg)](https://snapcraft.io/cline)
[![Base: core26](https://img.shields.io/badge/Base-core26-2C8C3C?logo=canonical&logoColor=white)](https://snapcraft.io/core26)
![Confinement: strict](https://img.shields.io/badge/Confinement-strict-2C8C3C)
[![Maintainer: sidaliteir](https://img.shields.io/badge/Maintainer-sidaliteir-2C8C3C?logo=snapcraft&logoColor=white)](https://snapcraft.io/publisher/sidaliteir)
![Architectures: amd64 and arm64](https://img.shields.io/badge/Architectures-amd64%20%7C%20arm64-2C8C3C?logo=linux&logoColor=white)

[Getting Started](#getting-started) · [Configure](#configure) · [Limitations](#limitations) · [Build](#build) · [Support](#support) · [License](#license)

Unofficial snap packaging for the [Cline CLI](https://github.com/cline/cline), an AI coding agent that edits files and runs commands in your projects.

</div>

---

## Getting Started

<a href="https://snapcraft.io/cline"><img alt="Get it from the Snap Store" src="https://snapcraft.io/en/dark/install.svg" width="180"></a>

```bash
sudo snap install cline
cline auth
cd ~/my-project
cline
```

`cline auth` signs in to Cline, a ChatGPT subscription or OCA, or saves an API key for another provider. For a one-off task, pass the prompt directly:

```bash
cline "Explain what this repository does"
cline --auto-approve false "Fix the failing tests"
```

Tools are approved automatically by default; `--auto-approve false` asks before each one.

## Configure

This snap has no `snap set` options. Manage providers, MCP servers and other settings with Cline itself:

```bash
cline config
cline auth --provider anthropic --apikey sk-... --modelid claude-sonnet-4-6
cline mcp
```

Application data is stored separately from a host installation:

| Data | Location |
| --- | --- |
| Settings, credentials, sessions and logs | `~/snap/cline/common/cline` |
| Global rules, workflows and hooks (`~/Documents/Cline`) | `~/snap/cline/current/Documents/Cline` |

Optional access to projects under `/media` or `/mnt`:

```bash
sudo snap connect cline:removable-media
```

## Limitations

- Commands the agent runs stay inside the snap. They can use bash, the core utilities, `git`, `ripgrep`, `curl`, `jq`, `patch`, `file`, `tree`, `zip`, `unzip` and `xz`. Toolchains installed on your system, such as `node`, `npm`, `python`, `go`, `cargo`, `make` or `docker`, are not available, so the agent cannot build, test or install dependencies for most projects. MCP servers that need those runtimes cannot start either.
- Inside agent commands, `~` is the snap's home (`~/snap/cline/current`), not your home folder. Your Git identity and `~/.ssh` keys are not visible there; set an identity with `git config user.name` / `user.email` in each project. Git over HTTPS works, Git over SSH does not find your keys.
- Projects in hidden folders directly under your home directory are inaccessible. Hidden files inside normal project folders work.
- `cline kanban`, `cline skill` and `cline update` rely on `npm`/`npx` and do not work. Update with `sudo snap refresh cline`; automatic update checks are disabled.

## Build

```bash
git clone https://github.com/sidalit/cline-snap.git
cd cline-snap
sudo snap install snapcraft --classic
snapcraft pack
```

## Support

- **Installation or snap issues:** [open a packaging issue](https://github.com/sidalit/cline-snap/issues).
- **Application bugs:** [report them upstream](https://github.com/cline/cline/issues).

## License

Snap packaging: [MIT](https://github.com/sidalit/cline-snap/blob/main/LICENSE).
