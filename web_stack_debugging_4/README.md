# web_stack_debugging_4/

Debugging resource limits and constraints in web stacks. This folder explores how user limits (ulimits), system constraints, and Puppet-managed configurations affect service performance and reliability. Learn to diagnose and fix issues caused by hitting resource ceilings.

## Files in this folder

- `0-the_sky_is_the_limit_not.pp` — Puppet manifest demonstrating scenarios where services hit resource limits (e.g., file descriptors, memory), leading to failures or degraded performance.
- `1-user_limit.pp` — Puppet manifest for setting and managing user limits (ulimits) for services, ensuring proper resource allocation.
- `README.md` — this file.

## Purpose

This README provides guidance for debugging issues related to resource exhaustion, limits, and configuration management. Use these manifests to simulate and resolve problems like "too many open files" or memory limits in web stacks.

## Debugging checklist (resource-focused)

- Check current limits: ulimits, systemd limits, and kernel parameters.
- Monitor resource usage: open files, memory, CPU.
- Identify limit violations: logs showing "too many files" or OOM kills.
- Adjust limits: via Puppet, systemd, or sysctl.
- Test under load: Simulate traffic to trigger limits.
- Automate limit management: Use manifests for consistency.

## Common tools & commands (resource limits)

Check limits:

```
# View current ulimits
ulimit -a

# Check systemd service limits
systemctl show nginx | grep Limit

# Kernel parameters
sysctl -a | grep fs.file-max
```

Monitor usage:

```
# Open file descriptors
lsof | wc -l
lsof -p $(pgrep nginx) | wc -l

# Memory usage
ps -o pid,ppid,user,%mem,rss -p $(pgrep nginx)
free -h

# Check for OOM kills
dmesg | grep -i oom
```

Puppet for limits:

```
# Apply manifests
sudo puppet apply --noop 0-the_sky_is_the_limit_not.pp
sudo puppet apply 0-the_sky_is_the_limit_not.pp

# Check Puppet resources
sudo puppet resource limits
```

## Example troubleshooting workflows

1) "Too many open files error"

- Check ulimits: `ulimit -n`
- Count open files: `lsof -p $(pgrep nginx) | wc -l`
- Increase limit: `ulimit -n 65536` (temporary) or via Puppet.

2) "Out of memory errors"

- Monitor memory: `free -h` and `ps aux --sort=-%mem`
- Check limits: `ulimit -m`
- Adjust via systemd or Puppet.

3) "Service fails under load"

- Use `1-user_limit.pp` to set appropriate limits.
- Test with load tools like `ab` or `siege`.

## Reproducing the lab locally

Install Puppet:

```
sudo apt update && sudo apt install puppet-agent
```

Run manifests:

```
cd web_stack_debugging_4

# Simulate limits issue
sudo puppet apply 0-the_sky_is_the_limit_not.pp

# Check limits
ulimit -a

# Set user limits
sudo puppet apply 1-user_limit.pp

# Test with a script that opens many files
python3 -c "import os; [open(f'/tmp/test{i}', 'w') for i in range(1000)]"
# Check if it fails due to limits
```

## Safety & best practices

- Test limits in isolated environments to avoid system instability.
- Backup before applying manifests: `sudo cp /etc/security/limits.conf /etc/security/limits.conf.bak`
- Monitor after changes; high limits can lead to resource exhaustion.
- Use `--noop` with Puppet for dry runs.

## Contributing

Add new limit-related manifests (e.g., `2-memory_limits.pp`) with problem setups and fixes. Update this README.

Maintainer: Goketech