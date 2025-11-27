
# web_stack_debugging_0/

Hands-on debugging notes and exercises for troubleshooting a simple web stack. This folder contains minimal examples used in labs to demonstrate how to diagnose common issues across the web stack: browser, DNS, network, load balancer, web server, application, and database layers.

## Files in this folder

- `0-give_me_a_page` — a minimal example that should serve a simple web page. Use this to reproduce basic "page not found" or HTTP error troubleshooting exercises.
- `README.md` — this file.

## Purpose

This README provides a practical checklist, common commands, and example workflows to guide debugging sessions for small web stacks. It is intended for learning and experimentation; always test changes in an isolated environment before applying them to production.

## Debugging checklist (high-level)

- Reproduce the problem: gather exact error messages, timestamps, and steps to reproduce.
- Confirm scope: is the problem single-user, network-wide, or service-specific?
- Check client-side issues: browser cache, developer console, and client networking.
- Verify DNS and routing: DNS records, TTLs, and traceroute.
- Inspect load balancer / reverse proxy: health checks, upstream status, and configuration.
- Inspect web server: access/error logs, service status, and resource usage.
- Inspect application: logs, stack traces, and dependency health (databases, caches).
- Verify backend services: database connectivity, query performance, and locks.
- Monitor resource constraints: CPU, memory, disk, and network interfaces.

## Common tools & commands

Client / DNS:

```
# Check DNS resolution
dig +short example.com
host example.com

# Test connectivity and HTTP response
curl -I http://<host>/
curl -v http://<host>/path

# Trace route (network path)
traceroute example.com
ping -c 4 example.com
```

Server / Networking:

```
# Check listening sockets and connections
ss -ltnp
netstat -tunlp

# View active TCP connections briefly
sudo tcpdump -n -i any port 80 or port 443

# Inspect firewall rules (iptables/nftables)
sudo iptables -L -n --line-numbers
sudo nft list ruleset
```

Web server / Proxy (example for Nginx):

```
# Test configuration
sudo nginx -t

# Reload configuration gracefully
sudo systemctl reload nginx

# View access and error logs (follow)
sudo tail -F /var/log/nginx/access.log /var/log/nginx/error.log

# Check service status
sudo systemctl status nginx --no-pager
```

Application / Backend:

```
# View application logs (path varies by app)
journalctl -u myapp.service -f

# Check app process and open files
ps aux | grep myapp
sudo lsof -p <pid>

# Reproduce with strace (careful on production)
sudo strace -f -p <pid> -s 200
```

Database:

```
# Basic connectivity (Postgres example)
PGPASSWORD=secret psql -h db.example.com -U user -d dbname -c 'SELECT 1'

# Check running queries
sudo -u postgres psql -c "SELECT pid, state, query FROM pg_stat_activity;"
```

Observability & performance:

```
# Top-like view
htop

# Disk usage
df -h
du -sh /var/log/* | sort -h

# Check open file descriptors
sudo lsof | wc -l
```

## Example troubleshooting workflows

1) "Page doesn't load / timeout"

- Reproduce from your client and capture `curl -v` output.
- Check DNS resolution with `dig`.
- Ping or traceroute the host to find network issues.
- From a host in the same network as the server, `curl` the backend directly to narrow whether the load balancer/proxy is involved.
- On the server, inspect web server logs and `tcpdump` for incoming requests.

2) "500 Internal Server Error"

- Check the web server error log (e.g., `/var/log/nginx/error.log`) for stack traces or upstream errors.
- Check application logs for exceptions and recent deployments.
- Verify backend dependencies (database reachable, caches up).

3) "Slow responses"

- Check server CPU / memory / I/O with `htop` and `iostat`.
- Look for slow queries in database logs or `pg_stat_statements`.
- Inspect application traces (if tracing is enabled) for bottlenecks.
- Check network latency with `ping` and `mtr`.

4) "Intermittent failures / 502 from proxy"

- Verify upstream health checks on the load balancer.
- Check for worker process crashes or restarts in application logs.
- Confirm connection limits (max_fds, keepalive settings) on web server and backend.

## Reproducing the lab locally

The `0-give_me_a_page` folder provides a minimal example that should serve a simple static page. To test locally:

```
# move into the example folder
cd web_stack_debugging_0/0-give_me_a_page

# start a simple Python HTTP server for quick testing (Python 3)
python3 -m http.server 8000

# then in another terminal
curl http://localhost:8000/
```

Or run the example behind a small reverse proxy (Nginx) to practice proxy-related debugging.

## Safety & best practices

- Always test changes in a disposable environment first (VM, container, or staging).
- When modifying firewall or network settings, ensure you have out-of-band access or a rollback plan to avoid locking yourself out.
- Record steps you take and timestamps to correlate with logs during post-mortem.

## Contributing

Add new debugging scenarios or helper scripts as small folders using the leading number convention (e.g., `1-dns_troubleshooting`) and update this README with a one-line summary and reproduction steps.

Maintainer: Goketech

