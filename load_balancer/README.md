
# load_balancer/

Examples and lab materials demonstrating simple load balancing concepts and configuration. The contents are intended for teaching and experimentation — test changes in an isolated environment (VM or container) before applying to production systems.

## What's in this folder

- `0-custom_http_response_header` — example showing how to add or modify HTTP response headers at the load balancer layer (useful for custom headers, security policies, or tracing).
- `1-install_load_balancer` — installation or provisioning notes and scripts to set up a load balancer (may include package installation and basic configuration steps).
- `README.md` — this file.

## Purpose

This folder contains minimal, hands-on examples to teach how a load balancer sits in front of backend servers and how it can be configured to:

- Route traffic to multiple backend servers
- Inject or modify HTTP response headers
- Perform simple health checks and basic failover

Use these examples to learn the concepts; they are not meant as production-ready configurations.

## Quick start

1. Inspect the example files to understand the intended topology and configuration.
2. Run any provisioning scripts in `1-install_load_balancer` in a disposable environment (VM or container).
3. Apply header modifications in `0-custom_http_response_header` to see how headers appear to clients.

Example: test a running load balancer with `curl` to see response headers:

	curl -I http://<load-balancer-ip>/

Look for the custom headers or changes introduced by the example configuration.

## Safety notes

- Do not run example scripts directly on production servers. Prefer ephemeral VMs, containers, or lab machines.
- Back up existing configuration files before applying changes.
- If an example script requires elevated privileges, review it line-by-line and consider running it under a controlled user or with `--noop`/dry-run options where available.

## Contributing

If you add examples, include:

- A short description of the topology (diagram or ASCII art)
- Clear, copy-pasteable commands to reproduce the lab
- Safety caveats and rollback steps

Maintainer: Goketech

