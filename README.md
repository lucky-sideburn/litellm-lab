# LiteLLM Lab Ansible Playbook

Ansible playbook scaffold to install and run with Podman:
- PostgreSQL
- Ollama
- LiteLLM

## Structure

- `playbooks/site.yml`: entrypoint playbook
- `playbooks/transports_shell.yml`: installs a curl-based chat transport script
- `inventories/dev/hosts.yml`: sample inventory (localhost)
- `group_vars/all.yml`: editable variables (ports, images, passwords)
- `roles/podman`: installs Podman and creates shared network
- `roles/postgresql`: runs PostgreSQL container
- `roles/ollama`: runs Ollama container
- `roles/litellm`: runs LiteLLM container with config mounted

## Prerequisites

- Ansible installed on control machine
- Linux target host (or VM) where Podman will run
- Sudo privileges on target host

## Usage

1. Install required collection:

```bash
ansible-galaxy collection install -r requirements.yml
```

2. Update variables in `group_vars/all.yml`:
   - `postgres_password`
   - `litellm_master_key`
   - image tags / ports as needed

3. Run the playbook:

```bash
ansible-playbook -i inventories/dev/hosts.yml playbooks/site.yml
```

4. Install transport shell script on the host:

```bash
ansible-playbook -i inventories/dev/hosts.yml playbooks/transports_shell.yml
```

5. Execute the script on target host:

```bash
litellm-chat.sh
```

## Default Endpoints

- Ollama: `http://<host>:11434`
- LiteLLM: `http://<host>:4000`

## Notes

- LiteLLM default config points to Ollama model `ollama/llama3.2`.
- You can edit `roles/litellm/files/config.yaml` to add more models/providers.
