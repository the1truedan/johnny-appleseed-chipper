# Package manager & install-channel catalog (alpha earmark)

**Status:** design catalog for future workstation / server inventory  
**Rule:** probe and receipt — **never** auto-install system updates from Johnny/CHIPPER  
**Host labels:** roles only (`desk-host`, `gpu-host`, `nas-host`)

This document lists **channels** we expect to *detect and version*, not a promise that every channel is wired today.

---

## 1. OS package managers

| Platform | Manager | Typical probe | Version field idea |
|----------|---------|---------------|--------------------|
| macOS | **Homebrew** | `brew --version` · `brew list --versions` | formula/cask + version |
| macOS | Mac App Store | report-only / manual | app id + CFBundleShortVersionString |
| Debian/Ubuntu | **apt** / dpkg | `apt-cache policy` · `dpkg -l` | package + version + arch |
| RHEL/Fedora | **dnf** / yum | `dnf list installed` | NEVRA |
| Arch | **pacman** | `pacman -Q` | name version |
| multi | **Snap** | `snap list` | name · revision · channel |
| multi | **Flatpak** | `flatpak list --app` | app id · branch · version |
| multi | AppImage | path + embedded update info if any | path + mtime + hash |
| Windows | **winget** | `winget list` | package id · version |
| Windows | **Chocolatey** | `choco list -l` | package · version |
| Windows | **Scoop** | `scoop list` | app · version |
| Windows | MSIX / Store | report-only | package family name |

## 2. Version managers (user-space)

| Manager | Notes |
|---------|--------|
| **asdf** | Plugin + current version per tool |
| **mise** | Same idea; modern asdf-class |
| **nvm** / **fnm** | Node line |
| **pyenv** / **uv** python pins | Prefer project lockfiles as SoR |
| **rustup** | toolchain channel + rustc version |
| **sdkman** | JVM ecosystem |

## 3. Language ecosystems

| Ecosystem | Lock / manifest evidence |
|-----------|---------------------------|
| Python | `pyproject.toml`, `requirements*.txt`, `uv.lock`, `poetry.lock` |
| Node | `package.json`, `package-lock.json`, `pnpm-lock.yaml`, `yarn.lock` |
| Rust | `Cargo.toml`, `Cargo.lock` |
| Go | `go.mod`, `go.sum` |
| Ruby | `Gemfile`, `Gemfile.lock` |
| PHP | `composer.json`, `composer.lock` |
| .NET | `*.csproj`, `packages.lock.json` |

CHIPPER-later: treat lockfiles as **artifacts** with content hashes; SBOM export is an adapter, not core.

## 4. Containers & orchestration

| Channel | Version evidence |
|---------|------------------|
| Docker Engine / Compose | engine version · image digests (`repo@sha256:…`) |
| Podman | same shape |
| Kubernetes (if ever) | chart version · image digests — out of alpha scope |

## 5. AI / creative weight channels

| Channel | Custody note |
|---------|----------------|
| Hugging Face hub | `repo_id` + revision + file SHA |
| CivitAI / sidecars | prefer existing `*.cm-info.json` hashes before rehash |
| Ollama | tag + digest when available |
| Comfy / SM pools | path role + size + SHA after promote |
| **FluxDown** (download plane) | multi-protocol pull into **nas-host staging** → promote + hash; UI/server open source ([zerx-lab/FluxDown](https://github.com/zerx-lab/FluxDown), AGPL server image) |
| aria2 / wget / curl | legacy; fine for small files; prefer FluxDown for bulk multi-conn on nas-host |

### Staging law (public wording)

1. Download **on the storage host** (or the GPU host that will consume next) — not “laptop through NFS.”  
2. Land under a dedicated **`_dl` / stage** prefix.  
3. Verify size (+ hash when known).  
4. Promote into the model/doc pool.  
5. Emit a C.H.A.I.N.S.-shaped receipt when the chain implementation exists.

## 6. Staged `manager` enum

```text
homebrew | asdf | mise | apt | dnf | pacman | snap | flatpak |
winget | choco | scoop | npm | pnpm | pip | uv | cargo | go |
docker | url | local | ollama | huggingface | fluxdown | other
```

## 7. Explicit non-goals

- No unattended `apt upgrade` / `brew upgrade` from this design.  
- No credentialed store accounts in public docs.  
- No private LAN URLs in this catalog (use roles).  
- No claim that listing a manager means support is implemented.

## See also

- [MANAGER_MODULE_MAP.md](./MANAGER_MODULE_MAP.md)  
- [PROCESS_TEMPLATES.md](./PROCESS_TEMPLATES.md) Template F (public sanitize)  
- [DEDUP_LAYERS.md](./DEDUP_LAYERS.md)  
