
# firewall/

Examples and lab exercises showing basic firewall and port forwarding concepts. These files are intended for learning and testing in isolated environments — do not run them on production hosts without review.

## Files in this folder

- `0-block_all_incoming_traffic_but` — example showing how to restrict incoming traffic, typically leaving only essential ports open (e.g., SSH or specific service ports). Inspect and adapt carefully.
- `100-port_forwarding` — example demonstrating port forwarding / NAT rules to forward traffic from one port or interface to another host/port.
- `README.md` — this file.

## Purpose

This folder demonstrates how firewall rules and port forwarding can be used to:

- Harden access by blocking unwanted incoming connections
- Expose internal services via port forwarding when appropriate for labs
- Combine simple firewall rules with NAT for controlled access

These examples are intentionally minimal to illustrate concepts. They are not production-ready policies.

## Quick start

1. Read the example files and understand the commands or scripts before running them.
2. Test firewall changes in an isolated environment (VM, container, or disposable instance).
3. Back up existing firewall configs before applying changes.

Example: check current iptables/nftables rules (Linux) before applying changes:

```bash
sudo iptables -L -n --line-numbers
# or (if using nftables)
sudo nft list ruleset
```

If you test `100-port_forwarding`, verify connectivity from a client and confirm the forwarding behavior:

```bash
curl http://<forwarding-host>:<forwarded-port>/
```

## Safety notes

- Do not run example scripts blindly on production systems.
- Ensure you have console or out-of-band access (or a rollback mechanism) when changing firewall rules remotely to avoid locking yourself out.
- Prefer ephemeral lab environments for experimentation.

## Contributing

When adding examples, include:

- A short description of the scenario and intended environment
- Commands to reproduce the lab and rollback steps
- Any required privileges and the recommended verification steps

Maintainer: Goketech

