# web_server/

Collection of scripts and configurations for setting up and configuring a web server, primarily using Nginx, as part of the ALU System Engineering / DevOps course. These are educational examples and should be reviewed and adapted before use in production.

## Files in this folder

- `0-transfer_file` — Script or instructions for transferring files to the server (e.g., using scp or rsync).
- `1-install_nginx_web_server` — Script to install and set up Nginx web server on a Linux system.
- `2-setup_a_domain_name` — Configuration or script to set up a domain name for the web server (may involve DNS or Nginx config).
- `3-redirection` — Nginx configuration or script for setting up URL redirections (e.g., HTTP to HTTPS, or path redirects).
- `4-not_found_page_404` — Setup for a custom 404 Not Found page in Nginx.
- `5-design_a_beautiful_404_page.html` — HTML file for a custom, styled 404 error page.
- `README.md` — This file.

## Usage notes

- These scripts are designed for Ubuntu/Debian-based systems. Check system requirements before running.
- Run scripts in order (0 to 5) as they build upon each other.
- Always backup configurations before applying changes.
- Test in a development environment first. Do not run on production servers without review.
- For Nginx, ensure you have sudo privileges or run as root.

## Examples

- To install Nginx (run `1-install_nginx_web_server`):

  ```bash
  sudo ./1-install_nginx_web_server
  ```

- To set up a domain (after installing Nginx):

  ```bash
  sudo ./2-setup_a_domain_name
  ```

- To enable redirections:

  ```bash
  sudo ./3-redirection
  ```

- To customize the 404 page, edit `5-design_a_beautiful_404_page.html` and apply with `4-not_found_page_404`.

## Safety and maintenance

- Review each script before execution. They may modify system files (/etc/nginx/, etc.).
- Use version control for any customizations.
- If adding new scripts, follow the numbering convention and update this README.
- Contributions: Keep scripts minimal, add comments, and include rollback instructions if possible.

## Prerequisites

- Ubuntu or Debian-based Linux distribution.
- Internet connection for package installation.
- Sudo access.