# Dotfiles

My modular dotfiles configuration for macOS (Homebrew) and Linux (Arch and Debian downstreams like Ubuntu and Pop!_OS) using GNU Stow for symlink management.

Each tool's configuration is organized in its own directory, making it easy to install or remove specific components as needed.

## Features

Some of the configurations included:

- **Nushell**: Modern shell configuration with custom scripts and completions
- **Neovim**: Text editor configuration with plugins and custom settings
- **Git**: Version control system configuration and aliases
- **Tmux**: Terminal multiplexer configuration
- **Starship**: Cross-shell prompt with custom theming
- **Zellij**: Terminal workspace manager configuration
- **Atuin**: Shell history sync and search configuration
- **Fastfetch**: System information tool configuration
- **Hammerspoon**: macOS automation and window management (macOS only)
- **Startup**: macOS launch agent configurations (macOS only)

## Installation

### Prerequisites

> [!IMPORTANT]
> **macOS**: Ensure you have [Homebrew](https://brew.sh/) installed on your system.
> **Linux (Arch-based)**: Install at least `cli` and `system` packages from `sysconfig.yaml`.

### Setup

1. Clone the repository:

```bash
git clone https://github.com/vimkin/dotfiles.git ~/dotfiles
cd ~/dotfiles
```

2. Install dependencies and configure:

**macOS:**

```bash
# Install everything (CLI, GUI app, App Store apps, fonts) at once with Homebrew
brew bundle install
```

**Linux (Arch-based):**

```bash
sudo pacman -S --needed $(yq '.packages.pacman.cli[]' sysconfig.yaml)
sudo pacman -S --needed $(yq '.packages.pacman.gui[]' sysconfig.yaml)
sudo pacman -S --needed $(yq '.packages.pacman.system[]' sysconfig.yaml)
sudo pacman -S --needed $(yq '.packages.pacman.fonts[]' sysconfig.yaml)
paru -S --needed $(yq '.packages.aur.cli[]' sysconfig.yaml)
paru -S --needed $(yq '.packages.aur.gui[]' sysconfig.yaml)
flatpak install flathub $(yq '.packages.flatpak[]' sysconfig.yaml)
```

3. Run the bootstrap script:

```bash
nu ./bootstrap.nu
```

The above command will:

- ‼️ Set Nushell as the default shell
- Set up rustup and install Rust toolchains
- Use GNU Stow to create symlinks for each configuration module (nushell, nvim, etc.)
- Clone and link additional Nushell scripts from the community repository

### Manual Configuration

For selective installation, use Stow directly:

```bash
stow nushell  # Install only Nushell config
stow nvim     # Install only Neovim config
stow starship # Install only Starship config
```
