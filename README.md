## Ansible Logrotate Config Management for `custom-monitor.service`

### What this playbook does

- **Ensures log directory exists**: Creates `/var/log/custom-monitor/` with
  permissions `0755`.
- **Installs a logrotate config**: Deploys `/etc/logrotate.d/custom-monitor`
  using a template with the following behavior:
  - **daily** rotation
  - **rotate 7** (keep the most recent 7 days of logs)
  - **compress** rotated logs
  - **notifempty** so empty (0-byte) logs are not rotated
  - **missingok** so rotation does not fail if logs do not yet exist
  - **postrotate hook** that reloads `custom-monitor.service`

### Prerequisites

- **Ansible** installed on your local machine (control node), e.g.:

  ```bash
  pip install ansible
  ```

- Network access from your control node to the host:
  - Hostname/IP: `35.182.198.10`
  - SSH user: `ubuntu`

- Specify the path to the private key in the `inventory.yml` file under `ansible_ssh_private_key_file`.
  - Example: If your key file is named `sre-logrotate-0.pem` and is in the same directory as `inventory.yml`, set:
    ```yaml
    ansible_ssh_private_key_file: ./sre-logrotate-0.pem
    ```
  - Ensure the private key has restricted permission:
    ```bash
    chmod 600 sre-logrotate-0.pem
    ```

### How to run the playbook

From the repository root (where `inventory.yml` and `logrotate.yml` live), run:

```bash
ansible-playbook -i inventory.yml logrotate.yml
```
