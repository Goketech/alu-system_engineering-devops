# web_stack_debugging_3/

Expert-level web stack debugging with system tracing and automation. This folder introduces advanced tools like `strace` for deep process inspection and Puppet manifests for reproducible debugging setups. Focus on low-level troubleshooting and infrastructure as code for debugging.

## Files in this folder

- `0-strace_is_your_friend.pp` — Puppet manifest that demonstrates using `strace` to trace system calls of a web server process (e.g., Nginx), including setup and example traces.
- `README.md` — this file.

## Purpose

This README guides advanced debugging using system-level tracing and configuration management. Learn to diagnose elusive issues like syscall failures, file access problems, and performance bottlenecks with `strace`, while automating setups with Puppet.

## Debugging checklist (expert)

- Use tracing tools: `strace`, `perf`, `dtrace` for syscall and kernel-level insights.
- Automate environments: Use Puppet/Ansible to reproduce issues consistently.
- Analyze traces: Look for errors, hangs, or unexpected calls in strace output.
- Correlate with logs: Match trace events to application logs.
- Test fixes: Apply changes and re-trace to verify.
- Scale debugging: Use manifests to deploy debug setups across multiple hosts.

## Common tools & commands (expert)

Strace basics:

```
# Attach to running process
sudo strace -p $(pgrep nginx) -f -s 200

# Trace new process
sudo strace -f -o trace.log nginx

# Filter syscalls (e.g., only file operations)
sudo strace -e trace=file -p $(pgrep nginx)

# Count syscalls
sudo strace -c -p $(pgrep nginx)
```

Puppet for debugging setup:

```
# Apply the manifest (dry-run first)
sudo puppet apply --noop 0-strace_is_your_friend.pp

# Apply changes
sudo puppet apply 0-strace_is_your_friend.pp

# Check Puppet logs
sudo journalctl -u puppet -n 20
```

Advanced tracing:

```
# Trace with timestamps
sudo strace -t -p $(pgrep nginx)

# Follow child processes
sudo strace -f -p $(pgrep nginx)

# Decode network calls
sudo strace -e trace=network -p $(pgrep nginx)
```

## Example troubleshooting workflows

1) "Process hangs or slow responses"

- Attach strace: `sudo strace -p $(pgrep nginx) -f`
- Look for blocking syscalls (e.g., `epoll_wait`, `read` hanging).
- Check for file locks or network timeouts in trace.

2) "Permission or access errors"

- Trace file operations: `sudo strace -e trace=file -p $(pgrep nginx)`
- Identify denied accesses (EACCES) and fix permissions.

3) "Reproducing issues with Puppet"

- Use `0-strace_is_your_friend.pp` to set up a traced Nginx.
- Run the manifest to deploy.
- Trigger the issue and collect traces.

## Reproducing the lab locally

Install Puppet and strace:

```
# Install Puppet (Ubuntu/Debian)
wget https://apt.puppet.com/puppet7-release-focal.deb
sudo dpkg -i puppet7-release-focal.deb
sudo apt update && sudo apt install puppet-agent

# Install strace
sudo apt install strace
```

Run the Puppet manifest:

```
# Navigate to the folder
cd web_stack_debugging_3

# Dry-run
sudo puppet apply --noop 0-strace_is_your_friend.pp

# Apply
sudo puppet apply 0-strace_is_your_friend.pp

# Check if Nginx is running with tracing
ps aux | grep nginx
sudo strace -p $(pgrep nginx) -c  # Quick summary
```

## Safety & best practices

- Strace can impact performance; use in staging or with `-c` for summaries.
- Puppet changes system state; always `--noop` first and backup configs.
- Avoid tracing in production without approval; use isolated environments.
- Clean up traces and logs after debugging.

## Contributing

Add new manifests or tracing examples as numbered files (e.g., `1-perf_tracing.pp`) with descriptions and usage. Update this README.

Maintainer: Goketech