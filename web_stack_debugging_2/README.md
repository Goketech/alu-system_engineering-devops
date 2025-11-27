# web_stack_debugging_2/

Advanced web stack debugging challenges and exercises. This folder focuses on complex issues like user permissions, service isolation, and minimal fix strategies. Build on concepts from `web_stack_debugging_0` and `web_stack_debugging_1` with hands-on labs for real-world troubleshooting.

## Files in this folder

- `0-iamsomeoneelse` — example demonstrating how to run processes or services as a different user (e.g., for security or isolation), including common pitfalls like permission errors.
- `1-run_nginx_as_nginx` — guide or script to configure and run Nginx as the dedicated `nginx` user, addressing ownership and access issues.
- `100-fix_in_7_lines_or_less` — a debugging challenge: fix a broken web stack configuration or script using 7 lines of code or fewer. Includes the problem description and expected solution.
- `README.md` — this file.

## Purpose

This README provides guidance for advanced debugging scenarios involving user contexts, service permissions, and efficient problem-solving. Use these exercises to practice diagnosing and resolving issues in multi-user, secured environments.

## Debugging checklist (advanced)

- Verify user contexts: effective UID/GID, file ownership, and permissions.
- Check service isolation: running as non-root, SELinux/AppArmor policies.
- Inspect privilege escalation: sudo usage, setuid binaries, and capabilities.
- Audit access controls: file permissions, ACLs, and network restrictions.
- Test minimal fixes: isolate the root cause and apply targeted changes.
- Validate in production-like setups: use containers or VMs with restricted users.

## Common tools & commands (advanced)

User and permissions:

```
# Check current user and groups
id
groups

# View file ownership and permissions
ls -la /path/to/file
stat /path/to/file

# Change ownership (careful!)
sudo chown nginx:nginx /var/log/nginx/

# Check SELinux context (if enabled)
ls -Z /path/to/file
sestatus
```

Service user management:

```
# Switch to user and run command
sudo -u nginx bash -c 'whoami && pwd'

# Check systemd service user
systemctl show nginx | grep User

# View process user
ps -o pid,ppid,user,group,cmd -p $(pgrep nginx)
```

Minimal fix strategies (inspired by `100-fix_in_7_lines_or_less`):

- Use `sed` or `awk` for quick config edits.
- Employ `find` and `xargs` for bulk permission fixes.
- Leverage `curl` or `wget` for testing without full scripts.

Example quick fix script:

```
#!/bin/bash
# Fix Nginx user in config (if wrong)
sed -i 's/user www-data;/user nginx;/' /etc/nginx/nginx.conf
# Reload service
systemctl reload nginx
# Test
curl -I http://localhost/
```

## Example troubleshooting workflows

1) "Permission denied errors when running as non-root"

- Check file ownership: `ls -la /var/www/html/`
- Verify user groups: `id nginx`
- Adjust permissions: `sudo chmod 755 /var/www/html/`
- Test with `sudo -u nginx curl http://localhost/`

2) "Service fails to start due to user context"

- Review systemd unit: `systemctl cat nginx`
- Check logs: `journalctl -u nginx --no-pager`
- Ensure user exists: `getent passwd nginx`
- Fix with `usermod` or config changes.

3) "Challenge: Fix in 7 lines or less"

- Analyze the problem in `100-fix_in_7_lines_or_less`.
- Identify the issue (e.g., wrong port, missing file).
- Apply a minimal fix using shell commands.
- Verify with tests.

## Reproducing the lab locally

For `0-iamsomeoneelse`:

```
# Create a test user
sudo useradd -m testuser

# Switch and run a command
sudo -u testuser bash -c 'echo "Running as $(whoami)"'
```

For `1-run_nginx_as_nginx`:

```
# Ensure nginx user exists
sudo useradd -r -s /bin/false nginx

# Update Nginx config to use nginx user
sudo sed -i 's/user www-data;/user nginx;/' /etc/nginx/nginx.conf

# Change ownership of logs and sites
sudo chown -R nginx:nginx /var/log/nginx /var/www/html

# Restart Nginx
sudo systemctl restart nginx
```

For `100-fix_in_7_lines_or_less`, follow the challenge instructions and implement a fix in ≤7 lines.

## Safety & best practices

- Use VMs or containers for user/permission experiments to avoid system-wide changes.
- Backup before changes: `sudo cp /etc/nginx/nginx.conf /etc/nginx/nginx.conf.bak`
- Test fixes incrementally and have rollback plans.
- Avoid running services as root in production.

## Contributing

Add new challenges as numbered folders (e.g., `2-selinux_troubleshooting`) with problem descriptions, expected fixes, and test cases. Update this README with summaries.

Maintainer: Goketech