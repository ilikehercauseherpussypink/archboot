# Safety notes

`leanin` is designed for a personal machine, but it keeps the risky parts explicit.

## Modes

| Mode | Behavior |
| --- | --- |
| `--doctor` | Read-only environment checks |
| `--plan` | Read-only package and service overview |
| `--dry-run` | Shows mutable actions without running them |
| `--yes` | Uses safe defaults; it does not force replacements |

`LEANIN_CI=1` only permits safe modes. `ARCHBOOT_CI=1` remains a temporary compatibility alias.

`LEANIN_REPO`, `LEANIN_BRANCH`, and `LEANIN_CI` are the primary override variables. The matching `ARCHBOOT_*` names remain supported as temporary fallbacks; when both are present, the `LEANIN_*` value wins.

## Existing state

Enter keeps the existing Git identity, SSH key, Codex installation, divergent npm prefix, GitHub key state and inactive service state unless a safe action is explicitly accepted. Local SSH keys are never deleted; a confirmed regeneration creates backups and restores them if generation fails.

When started through `curl | bash`, prompts, `ssh-keygen`, and `ssh-add` use `/dev/tty` when available. Without a terminal, the installer keeps the safe default, refuses interactive key generation, and leaves agent loading for a later `ssh-add` command.

`--help` and `--version` finish before the repository bootstrap, so they do not create logs or download the full project.

## GitHub authentication

During an interactive run, leanin can launch a supervised GitHub authentication worker. It requests the `admin:public_key` scope, waits visibly for the browser/device-code flow, rechecks `gh auth status` and `gh ssh-key list`, then continues automatic SSH-key registration only when key management is available.

When a graphical session and supported terminal are available, the worker can open in a separate terminal. Otherwise it uses the current `/dev/tty`. `--yes`, `--dry-run`, `--doctor`, `--plan`, and CI never launch it. If it fails, times out, or cannot start, leanin shows the key, the exact `gh` command, and the manual GitHub-keys URL.

`GH_TOKEN` or `GITHUB_TOKEN` can satisfy GitHub CLI authentication for headless automation, but leanin does not require either for the normal personal flow.

## Packages and services

The installer never removes packages, clears the pacman lock or enables a missing service. It verifies the pacman lock before package work and treats the Mullvad package conflict as a skip.

## Remote bootstrap and logs

The short domain serves `install.sh`; the script then downloads the repository tarball from GitHub into a temporary directory and validates its layout before continuing. The repository/branch override inputs are restricted to safe GitHub HTTPS values.

Real runs log to `~/.local/state/leanin/logs` with restrictive permissions. Existing `~/.local/state/archboot/logs` files are not migrated or removed. Common token patterns and private-key blocks are redacted, but inspect logs before sharing them.
