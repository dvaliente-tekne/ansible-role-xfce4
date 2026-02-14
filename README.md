# XFCE4 Role

Installs XFCE4 desktop environment and related packages for Arch Linux. Optionally installs and configures LightDM on specific hosts.

## What It Does

1. **Installs XFCE4 packages** on all hosts (desktop, themes, portals, Xorg)
2. **Installs LightDM** on ASTER hostname only
3. **Enables LightDM service** (without starting) on ASTER
4. **Verifies installation** of all packages and service state

## Requirements

- `community.general` collection (for `pacman` module)
- LightDM host check uses the hostname read from `/etc/hostname`; no extra role required

```bash
ansible-galaxy collection install community.general
```

## Role Variables

| Variable | Default | Description |
|----------|---------|-------------|
| `xfce4_packages` | See defaults | XFCE4 packages to install on all hosts |
| `xfce4_lightdm_packages` | See defaults | LightDM packages for specific hosts |
| `xfce4_lightdm_hostname` | `ASTER` | Hostname that requires LightDM |
| `xfce4_lightdm_service` | `lightdm` | LightDM service name |

### XFCE4 Packages (all hosts)

- xfce4, xfce4-goodies, xfce4-panel-profiles
- gnome-keyring, seahorse, libsecret
- xdg-desktop-portal, xdg-desktop-portal-gtk, xdg-desktop-portal-xapp, xdg-desktop-portal-cosmic
- libportal, libportal-gtk4, libportal-qt6
- bibata-cursor-theme-bin, flat-remix, kora-icon-theme
- xorg-server, xorg-apps, xdotool

### LightDM Packages (ASTER only)

- lightdm, lightdm-gtk-greeter, lightdm-gtk-greeter-settings

## Dependencies

None. The role reads the hostname from `/etc/hostname` and uses it to decide whether to install LightDM (when it matches `xfce4_lightdm_hostname`, case-insensitive).

## Example Playbook

```yaml
- hosts: workstations
  roles:
    - ansible-role-xfce4   # Installs XFCE4, LightDM on host matching xfce4_lightdm_hostname (default: ASTER)
```

## Tags

| Tag | Description |
|-----|-------------|
| `xfce4` | All XFCE4 tasks |
| `packages` | Package installation only |
| `lightdm` | LightDM specific tasks |
| `service` | Service management |
| `verify` | Verification tasks |

## Host-Specific Behavior

| Hostname | XFCE4 Packages | LightDM Packages | LightDM Service |
|----------|----------------|------------------|-----------------|
| ASTER | Installed | Installed | Enabled (stopped) |
| Others | Installed | Skipped | Skipped |

## License

MIT-0

## Author

dvaliente
