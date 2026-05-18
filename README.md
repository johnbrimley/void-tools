# void-tools

A collection of quality-of-life shell scripts for Void Linux.

## Install

Download and run the installer using `xbps-fetch`:

```sh
xbps-fetch https://raw.githubusercontent.com/johnbrimley/void-tools/main/svc-installer
bash ./svc-installer
```

The installer fetches the latest versions of all scripts from this repo and places them in `~/.local/bin/`. If that directory isn't on your `PATH` yet, it will add it to `~/.bash_profile`.

## Scripts

### `pkg-install <package> [package...]`

Install one or more packages. Suggests `pkg-search` if a package name isn't found.

```sh
sudo pkg-install firefox
```

### `pkg-remove <package> [package...]`

Remove a package and its unneeded dependencies. Checks the package is actually installed first.

```sh
sudo pkg-remove firefox
```

### `pkg-search <query>`

Search available packages by name or description.

```sh
pkg-search elogind
```

### `pkg-list [filter]`

List installed packages. Accepts an optional filter to narrow results.

```sh
pkg-list            # all installed packages
pkg-list python     # installed packages matching "python"
```

### `pkg-update`

Sync repositories and upgrade all installed packages.

```sh
sudo pkg-update
```

---

### `svc-create <name> <command...>`

Create a new runit service and enable it immediately. Writes `/etc/sv/<name>/run` with the given command and marks it as void-tools-managed.

```sh
sudo svc-create mysite node /path/to/app.js
sudo svc-create myapi /usr/local/bin/myapi --port 8080
```

### `svc-delete <name>`

Delete a service and its `/etc/sv/<name>` directory. Only works on services created with `svc-create` — refuses to touch anything installed by xbps. Disables the service first if it is currently enabled.

```sh
sudo svc-delete mysite
```

### `svc-list`

List enabled services and their current status.

```sh
svc-list            # show enabled services
svc-list --all      # show all available services in /etc/sv/
```

### `svc-add <service>`

Enable a service. If the service name isn't found, suggests similarly named services.

```sh
sudo svc-add dbus
```

### `svc-log [--all] [service]`

Show service logs from `/var/log/`. With no arguments, tails the last 20 lines from every enabled service that has logging configured. Pass a service name to focus on one. `--all` dumps the full log instead of just the tail.

```sh
svc-log                  # last 20 lines from all enabled services
svc-log dbus             # last 20 lines from dbus
svc-log --all dbus       # full dbus log
svc-log --all            # full log from all enabled services
```

### `svc-remove <service>`

Disable a service. Brings it down cleanly before removing it.

```sh
sudo svc-remove dbus
```

### `svc-installer`

Re-run at any time to update all scripts to the latest version from this repo.

```sh
svc-installer
```
