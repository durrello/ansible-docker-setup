> **Note:** This repo overlaps with [ansible-masterclass](https://github.com/durrello/ansible-masterclass), which is the cleaner, maintained version (roles, dynamic inventory, docs). This one is kept for history.

# ansible-docker-setup

An Ansible learning environment that uses Docker containers as managed nodes. Spin up SSH-enabled
Ubuntu containers, then run a set of playbooks against them — from a simple connectivity check to
installing packages, patching, and deploying a static website to Apache.

## Layout

```
.
├── ansible.cfg                 # Inventory path, host key checking off, no retry files
├── inventory.ini               # Two nodes (web, db groups) over local SSH ports 2222/2223
├── docker-compose.yml          # Brings up the managed-node containers
├── Dockerfile.node             # SSH-enabled Ubuntu image used as a node
├── ansible-ping.yaml           # Connectivity check (ping module)
├── packages.yml                # Install a list of packages
├── packages-variables.yml      # Package install driven by variables
├── patching.yaml               # Apply OS updates/patches
├── update-servers.yaml         # Update all servers
├── playbook1.yaml              # Intro playbook
├── deploy-website.yaml         # Install Apache and deploy the static site
├── website-deployment.yaml     # Website deployment variant
└── website/                    # Static site deployed by the website playbooks
```

## What this demonstrates

- Using Docker containers as Ansible managed nodes (no cloud needed)
- Inventory groups (`web`, `db`) and group variables
- Progressively complex playbooks: ping → packages → patching → app deployment
- Deploying and verifying a real service (Apache serving the bundled site)

## Prerequisites

- [Docker](https://docs.docker.com/get-docker/) and Docker Compose
- [Ansible](https://docs.ansible.com/ansible/latest/installation_guide/index.html)
- An SSH key at `~/.ssh/id_rsa` (referenced by the inventory)

## Usage

```bash
# 1. Bring up the managed nodes
docker compose up -d --build

# 2. Verify connectivity
ansible-playbook ansible-ping.yaml

# 3. Run a playbook (examples)
ansible-playbook packages.yml
ansible-playbook patching.yaml
ansible-playbook deploy-website.yaml

# 4. Tear down
docker compose down
```

## Notes

- `host_key_checking` is disabled in `ansible.cfg` for convenience in this throwaway lab — do not
  carry that setting into production.
- The inventory uses local ports 2222/2223 mapped to the two containers.

## License

MIT
