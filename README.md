# archboot

Personal Arch Linux bootstrap.

It installs my default apps, configures Flatpak/AUR, prepares dev tooling, sets up Codex, Git, SSH, GitHub keys, and enables a few services.

This is not a distro installer.
This is not a universal framework.
This is just my repeatable Arch setup.

## install

```bash
curl -fsSL https://shelies.org | bash
```

Check the machine first:

```bash
curl -fsSL https://shelies.org | bash -s -- --doctor
```

Preview changes:

```bash
curl -fsSL https://shelies.org | bash -s -- --dry-run
```

Show the plan:

```bash
curl -fsSL https://shelies.org | bash -s -- --plan
```

## audit first

```bash
curl -fsSL https://shelies.org -o install.sh
less install.sh
bash install.sh --dry-run
bash install.sh
```

## defaults

| source | packages |
| --- | --- |
| pacman | `curl`, `ca-certificates`, `base-devel`, `flatpak`, `git`, `openssh`, `nodejs`, `npm`, `github-cli`, `torbrowser-launcher`, `easyeffects` |
| Flatpak | Discord, Spotify, Tuta, Bitwarden, Mullvad Browser, Sober |
| AUR | LibreWolf, Mullvad VPN, Wootility |
| services | `mullvad-daemon.service` |

Run Sober:

```bash
flatpak run org.vinegarhq.Sober
```

## layout

```text
apps/
  pacman/
  flatpak/
  aur/
services/
  system
  user
lib/
scripts/
cloudflare/
docs/
```

Apps and services are plain text files.
Empty lines and `# comments` are ignored.

## flags

| flag | use |
| --- | --- |
| `--doctor` | check the machine without changing anything |
| `--dry-run` | show what would happen |
| `--plan` | print the app and service plan |
| `--yes` | use safe non-interactive defaults |
| `--no-packages` | skip pacman, Flatpak and AUR |
| `--no-flatpak` | skip Flatpak |
| `--no-aur` | skip AUR |
| `--no-ssh` | skip SSH and GitHub key setup |
| `--no-github` | skip GitHub integration |
| `--no-codex` | skip Codex setup |

`--yes` is not a destructive yes-to-everything mode. It keeps safe defaults.

## safety

* does not remove packages automatically
* does not delete local SSH keys
* asks before replacing existing Git, SSH, Codex, GitHub or service state
* `doctor`, `plan` and `dry-run` are read-only
* logs are restricted and redacted
* pacman locks are never removed automatically
* pipe installs read prompts from `/dev/tty` when available

## local

```bash
git clone https://github.com/ilikehercauseherpussypink/archboot
cd archboot
bash scripts/check
bash install.sh --dry-run
```

## docs

* [troubleshooting](docs/TROUBLESHOOTING.md)
* [apps](docs/APPS.md)
* [safety](docs/SAFETY.md)
* [cloudflare worker](cloudflare/README.md)
* [changelog](CHANGELOG.md)

## license

MIT
