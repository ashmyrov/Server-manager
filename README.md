# server-manager

Small CLI to manage multiple Git SSH identities by workspace folders.

## Idea

- You keep projects under a root folder (default: `~/Server`)
- Each top-level folder is a "namespace" (example: `~/Server/Dinner/`, `~/Server/Personal/`)
- For each namespace we use one SSH key: `~/.ssh/<namespace_lowercase>`
- Git auto-selects the correct key based on repo path using `gitconfig includeIf gitdir:*`

Result: you can clone repos with normal URLs like `git@github.com:org/repo.git`,
and the correct SSH key is chosen automatically depending on which folder the repo lives in.

## Files used on your machine

- `~/.server-manager.conf` — config (paths)
- `~/.gitconfig` — only gets one include line:
  - `[include] path = ~/.gitconfig-server`
- `~/.gitconfig-server` — managed by this tool (contains includeIf blocks)
- `~/.gitconfig-<namespace>` — generated per namespace
- `~/.ssh/<namespace>` and `~/.ssh/<namespace>.pub` — generated SSH keys

## Install (Linux / macOS)

1) Clone repo and put `server` into your PATH:

```bash
git clone <this-repo> ~/server-manager
mkdir -p ~/bin
ln -sf ~/server-manager/bin/server ~/bin/server
echo 'export PATH="$HOME/bin:$PATH"' >> ~/.bashrc   # use ~/.zshrc on macOS
source ~/.bashrc
