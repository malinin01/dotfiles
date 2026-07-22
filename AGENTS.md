# Repository Guide

## Layout and Sources of Truth

- This is a GNU Stow repository. Each top-level package mirrors its destination below `$HOME`; `.stowrc` supplies `--target=~`.
- `bootstrap.nu` stows common packages plus `hammerspoon startup` on macOS or `cosmic flatpak scripts` elsewhere. `agentgateway` is never bootstrapped; stow it explicitly when intended.
- Use `Brewfile` for macOS packages and `sysconfig.yaml` for Arch and Flatpak package lists. There is no maintained Debian system-package manifest.

## Safety

- Do not run `nu ./bootstrap.nu` as validation: it installs or updates Rust, clones or pulls `~/.nu_scripts`, creates Stow links, may edit `/etc/shells`, changes the login shell, and launches Nushell.
- Keep `nushell/.config/nushell/history.txt` and `nvim/.config/nvim/lazy-lock.json` ignored. Nushell also regenerates shell initialization files under `~/.cache`; those are not package content.
- Do not bulk-normalize files under `cosmic/`; the pre-commit EOF fixer deliberately excludes that application-generated tree.
- Keep password-manager references such as `pass://` unresolved in tracked files; never replace them with plaintext credentials.
- Avoid starting Neovim merely to validate configuration: first startup clones `lazy.nvim`. `startup/.startup/boot.sh` is Nushell despite its `.sh` suffix and starts services and containers.

## Verification

- Focused repository checks: `pre-commit run --files <changed-files>`.
- Full configured checks: `pre-commit run --all-files`. There is no separate repository-wide test suite.
- Check Stow conflicts without changing links: `stow --simulate --verbose <package>`.
- For scripts under `scripts/.local/bin/`, use `bash -n <script>`; do not apply this to the Nushell startup script.
- Validate agentgateway Compose changes without starting services: `docker compose --env-file agentgateway/.config/agentgateway/.env -f agentgateway/.config/agentgateway/compose.yaml config --quiet`.
