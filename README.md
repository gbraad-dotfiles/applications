Gerard Braad's application descriptions
=======================================

Application install/uninstall descriptions for use with [`apps.zsh`](https://github.com/gbraad-dotfiles/upstream/blob/main/zsh/.zshrc.d/apps.zsh) helper

The `apps` helper script is a Zsh utility designed to provide a fuzzy, interactive interface for managing and executing application actions defined in Markdown files. This makes it easy to organize, discover, and run application-specific tasks or installation commands, especially when maintaining portable or modular dotfiles.

## Features

  - **Fuzzy App Selection:** Browse and select from available application definitions using `fzf`.
  - **Section/Action Picker:** Select specific actions (sections) to run for each app, such as `install`, `update`, or custom actions.
  - **Modular App Definitions:** Application actions are defined in Markdown files (one per app), allowing for easy organization and documentation.
  - **Exit Code Propagation:** Correctly returns the exit status of script blocks, enabling use in shell conditionals (`if apps ...; then ...`).
  - **Pipeable Output:** Ensures only script output is sent to `stdout`; logs and info go to `stderr`, so you can safely pipe output (e.g. `apps tailscale status | grep online`).
  - **Symlink-Safe:** Robustly handles app definition directories that are symlinks.
  - **Lightweight & Extensible:** Easily add new apps or actions by creating or editing Markdown files.


## Getting Started

### Directory Structure

By default, app definitions live in `~/.dotapps/`. Each app has a Markdown file, possibly nested in subdirectories, e.g.:

```
~/.dotapps/
  tailscale.md
  bundle/
    essential.md
  obsidian.md
```

Each Markdown file uses sections (`## section-name`) and code blocks for actions


## OS-Specific and Packager-Specific Actions
You can define actions that are specific to an operating system (like linux, darwin, or windows), or to a package manager or method (like flatpak, appimage, brew, etc). The script will automatically select the most appropriate override based on the current environment.

Section naming convention
  - `## action` — Generic action (default for all systems)
  - `## <os>-action` — OS-specific override (e.g. `bluefin-install`, `fedora-install`)
  - `## <packager>-action` — Packager-specific override (e.g. `flatpak-install`, `brew-install`)

When running an action, the script checks in order:

  -  OS-specific (`<os>-action`)
  - Packager-specific (`<packager>-action`)
  - Generic (`action`)

## How to override
Simply add a new section with the appropriate suffix (as above) to your application's markdown file. The helper script will handle the rest, picking the best-matching action for your current environment.


## Usage

### Fuzzy App and Action Selection
```sh
apps
```

Prompts you to select an app and then an action/section to run.

### Fuzzy Action Selection for a Specific App
```sh
apps bundle/essential
```

Prompts you to select an action/section for bundle/essential.

### Run a Specific App Action Directly
```sh
apps tailscale install
```

Runs the install action for tailscale, applying any OS/packager-specific overrides.

### Use in Conditionals
```sh
if apps obsidian check user; then
    echo "Obsidian user install detected!"
fi
```

The exit code from the script block is returned, so you can use apps in if/elif/else statements.

### Pipeable Output
```sh
apps tailscale status | grep online
```

Only the output from the script block is sent to stdout, so piping and grepping works as expected.
