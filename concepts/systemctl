# Understanding `systemctl list-units --type=service`

## Overview
The `systemctl` command is used to interact with the `systemd` init system, which manages services on modern Linux distributions. The `list-units` subcommand, when used with `--type=service`, provides a list of active service units.

## Syntax
```bash
systemctl list-units --type=service
```

## Breakdown of the Command
- `systemctl`: Command to control the systemd service manager.
- `list-units`: Lists the units that are currently loaded.
- `--type=service`: Filters the output to show only service units.

## Output Explanation
The output consists of several columns:
1. **UNIT**: Name of the service.
2. **LOAD**: Whether the service is properly loaded.
3. **ACTIVE**: Current active state (e.g., active, inactive, failed).
4. **SUB**: More detailed status information (e.g., running, exited).
5. **DESCRIPTION**: A brief description of the service.

## Additional Options
- **List all units, including inactive ones**:
  ```bash
  systemctl list-units --type=service --all
  ```
- **Show failed services**:
  ```bash
  systemctl list-units --type=service --state=failed
  ```
- **List only running services**:
  ```bash
  systemctl list-units --type=service --state=running
  ```
- **Check if a specific service is active**:
  ```bash
  systemctl is-active <service-name>
  ```
- **Get detailed information about a service**:
  ```bash
  systemctl status <service-name>
  ```

## Example Output
```bash
UNIT                    LOAD   ACTIVE SUB     DESCRIPTION  
avahi-daemon.service   loaded active running Avahi mDNS/DNS-SD Stack  
cron.service           loaded active running Regular background program processing daemon  
dbus.service           loaded active running D-Bus System Message Bus  
```

## Summary
This command is useful for monitoring system services, checking service status, and troubleshooting issues related to systemd services.

