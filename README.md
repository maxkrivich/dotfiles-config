# dotfiles

Personal dotfiles managed with [chezmoi](https://www.chezmoi.io).

## Prerequisites

- macOS with [Homebrew](https://brew.sh) installed
- [1Password](https://1password.com) (used as the SSH agent, see `private_dot_ssh/private_config`)

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
| `dot_gitconfig` | `~/.gitconfig` |
| `dot_gitconfig-personal` | `~/.gitconfig-personal` |
| `dot_gitconfig-work` | `~/.gitconfig-work` |
| `dot_config/` | `~/.config/` (ghostty, mise, starship) |
| `private_dot_ssh/private_config` | `~/.ssh/config` (permissions restricted, contents kept out of `chezmoi diff` output by default) |

Git identity switches automatically between `.gitconfig-personal` and `.gitconfig-work` based on whether the repo lives under `Projects/Personal/**` or `Projects/Work/**`.

## Everyday usage

```sh
chezmoi edit <file>   # edit a managed file's source
chezmoi diff           # preview pending changes
chezmoi apply          # apply changes to $HOME
chezmoi cd             # cd into the source directory
```
