# huen-sessionizer

A tmux project and session manager written in Zig. The executable is `tms`.

- Find Git and jj repositories, worktrees, and bookmarked directories.
- Open projects as sessions with a built-in fuzzy picker.
- Switch sessions and windows with pane previews.
- Configure startup sessions, worktree windows, shortcuts, and indexed marks.

Runs on macOS and Linux. Requires tmux and Git; jj is optional.

## Install

Install [mise](https://mise.jdx.dev/getting-started.html) 2026.10.17 or newer,
then build from source:

```sh
git clone https://github.com/rickyhugo/huen-sessionizer.git
cd huen-sessionizer
mise trust
mise install --locked zig python
MISE_AUTO_INSTALL=false mise run install
```

This uses pinned Zig 0.17.1 and Python. The installer places `tms` in
`$HOME/.local/bin` and notices in `$HOME/.local/share/licenses/tms`.
Add `$HOME/.local/bin` to `PATH`.

To use another prefix:

```sh
MISE_AUTO_INSTALL=false mise run install -- /your/prefix
```

Release archives, when published, are available on the
[releases page](https://github.com/rickyhugo/huen-sessionizer/releases).
See [packaging](docs/packaging.md) for targets and checksum verification.

## Quick start

Set the directories to search, then open the picker:

```sh
tms config --paths ~/code ~/work
tms
```

Add popup bindings to `~/.tmux.conf`:

```tmux
bind C-o display-popup -E "tms"
bind C-j display-popup -E "tms switch"
bind C-w display-popup -E "tms windows"
```

## Commands

| Command | Purpose |
| --- | --- |
| `tms` | Find a project and open its session |
| `tms switch` | Pick another session |
| `tms windows` | Pick a window in the current session |
| `tms start` | Open configured startup sessions |
| `tms sessions` | List sessions, marking the current one with `*` |
| `tms kill` | Kill the current session and switch to another |
| `tms rename NAME` | Rename the active session and its directory |
| `tms refresh` | Create missing worktree windows |
| `tms clone-repo` | Clone a repository and open its session |
| `tms add URL` | Clone a repository to `~/<host>/<path>` and open its session |
| `tms init-repo` | Create a repository |
| `tms bookmark` | Add a directory to the project picker |
| `tms open-session` | Open a session directly |
| `tms marks` | Manage sessions accessible by index |
| `tms config` | Change search paths and other settings |

Run `tms --help` or `tms COMMAND --help` for arguments and options.

To show sessions in the tmux status bar:

```tmux
set -g status-right "#(tms sessions)"
```

## Configuration

Use `tms config` for common settings:

```sh
tms config --session main
tms config --excluded node_modules target
tms config --full-path true
```

Default configuration paths:

- Linux: `~/.config/tms/config.toml`
- macOS: `~/Library/Application Support/tms/config.toml`

Set `TMS_CONFIG_FILE` to use an explicit path. The configuration supports
search depth, repository providers, bookmarks, startup sessions, picker colors,
and custom key bindings.

For example, add this to the TOML file:

```toml
[shortcuts]
"ctrl-k" = "delete_to_line_end"
```

## Shell completions

Add the matching line to your shell configuration:

```bash
# Bash (~/.bashrc)
source <(COMPLETE=bash tms)
```

```zsh
# Zsh (~/.zshrc)
source <(COMPLETE=zsh tms)
```

```fish
# Fish (~/.config/fish/config.fish)
COMPLETE=fish tms | source
```

## Development

```sh
mise install --locked
mise run hooks:setup
mise run build
mise run test
mise run ci
```

Mise pins the development tools. hk runs shared local and CI checks for
formatting, Zig tests, command behavior, terminal handling, and packaging.
The commit hook enforces [Conventional Commits](https://www.conventionalcommits.org/).

Full hk checks run on Linux and Apple Silicon. Intel macOS supports source
and archive builds; the pinned hk version has no Intel macOS binary.

See [development](docs/development.md) and [packaging](docs/packaging.md).

## Credits and license

Derived from [jrmoulton/tmux-sessionizer](https://github.com/jrmoulton/tmux-sessionizer),
with the application rewritten in Zig.
Licensed under [MIT](LICENSE). Third-party notices accompany installed binaries
and release archives.
