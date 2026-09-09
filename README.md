# dotfiles

Personal dotfiles managed with [chezmoi](https://www.chezmoi.io).

## Prerequisites

- macOS with [Homebrew](https://brew.sh) installed
- [1Password](https://1password.com) or [Bitwarden](https://bitwarden.com) (used as the SSH agent, see `private_dot_ssh/private_config.tmpl`)

## Quick setup

```sh
# install chezmoi if not already present
brew install chezmoi

# initialize and apply dotfiles from this repo
chezmoi init --apply git@github.com-work:maxkrivich/dotfiles-config.git
```

On a brand-new machine without SSH keys set up yet, bootstrap over HTTPS instead:

```sh
chezmoi init --apply https://github.com/maxkrivich/dotfiles-config.git
```

On first run, chezmoi will prompt once for your full name, personal/work email, personal/work SSH signing key (public key), and which SSH agent (1Password or Bitwarden) to use. Answers are cached in chezmoi's own config, so later `chezmoi apply` runs won't re-prompt.

Commit signing uses `gpg.format = ssh`. With 1Password, `gpg.ssh.program` points at `op-ssh-sign` so signing happens via the agent without the private key ever touching disk. Bitwarden has no equivalent helper yet, so with `sshAgent: bitwarden` signing falls back to the default `ssh-keygen` — verify this actually works before relying on it.

## Install packages

The `Brewfile` isn't run automatically by chezmoi, install it separately:

```sh
brew bundle --file=~/.local/share/chezmoi/Brewfile
```

## What's managed

| Source | Target |
|---|---|
| `dot_zshrc` | `~/.zshrc` |
| `dot_tmux.conf` | `~/.tmux.conf` |
| `dot_gitconfig.tmpl` | `~/.gitconfig` |
| `dot_gitconfig-work.tmpl` | `~/.gitconfig-work` |
| `dot_config/` | `~/.config/` (ghostty, mise, starship) |
| `private_dot_ssh/private_config.tmpl` | `~/.ssh/config` (permissions restricted, contents kept out of `chezmoi diff` output by default) |

Git identity defaults to personal everywhere. Only repos under `Projects/Work/**` switch to the work identity via `.gitconfig-work`.

## Everyday usage

```sh
chezmoi edit <file>   # edit a managed file's source
chezmoi diff           # preview pending changes
chezmoi apply          # apply changes to $HOME
chezmoi cd             # cd into the source directory
```
