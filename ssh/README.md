
# ssh/

Collection of SSH-related examples and a Puppet manifest used in the course. The files here are intended as learning material and convenience helpers — they are not production-ready and should be reviewed before use.

## Files in this folder

- `0-use_a_private_key` — notes / examples showing how to use an existing private key when connecting (do not store private keys in repo).
- `1-create_ssh_key_pair` — instructions or helper script to create an SSH key pair for use in labs.
- `2-ssh_config` — example `ssh_config` snippets students can adapt to their environment.
- `100-puppet_ssh_config.pp` — Puppet manifest that demonstrates how SSH client config and settings can be managed with Puppet. Read the manifest before applying it.
- `README.md` — this file.

## Usage notes

- Safety first: never commit private keys or secrets. If you generate keys locally for the lab, add them to your machine's `~/.ssh` and to `.gitignore` if they touch the repository.
- Review `100-puppet_ssh_config.pp` before running. To test the manifest locally (requires Puppet installed):

	- Run a no-op (dry run) to see changes Puppet would make:

		sudo puppet apply ssh/100-puppet_ssh_config.pp --noop

	- If the dry run looks correct, run without `--noop` to apply changes:

		sudo puppet apply ssh/100-puppet_ssh_config.pp

	Note: Running manifests with `sudo` or as root may modify system SSH configs. Prefer running in a disposable VM or container for labs.

## Examples

- To generate a key pair (see `1-create_ssh_key_pair`):

	ssh-keygen -t ed25519 -C "your.email@example.com"

- To use an SSH key when connecting to a host (do not store keys here):

	ssh -i ~/.ssh/id_ed25519 user@host.example.com

## Maintenance / contributions

If you update or add examples, include a brief usage section and any safety caveats. Keep examples minimal and avoid including real credentials.

