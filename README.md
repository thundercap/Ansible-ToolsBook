# How to Run This Ansible Playbook

## Prerequisites

1. **Ansible Installed**: Ensure Ansible is installed on your control node (the machine you're running commands from).
   ```bash
   ansible --version
   ```

2. **SSH Access**: Ensure you have SSH access to all target hosts defined in `inventories.txt`.

3. **Install Required Collections**:
   ```bash
   ansible-galaxy collection install -r requirements.yml
   ```

## Running the Playbook

### Basic Execution

Run the entire playbook against all hosts:

```bash
ansible-playbook -i inventories.txt runbook.yml
```

### With Verbose Output

For debugging, use verbose mode:

```bash
ansible-playbook -i inventories.txt runbook.yml -v
# or -vv, -vvv, -vvvv for more verbosity
```

### Run Specific Plays

To run only specific parts:

**Common Setup only (Docker, AWS CLI on all hosts):**
```bash
ansible-playbook -i inventories.txt runbook.yml --tags common
```

**Monitoring Stack only (Prometheus, Grafana on monitor group):**
```bash
ansible-playbook -i inventories.txt runbook.yml --limit monitor
```

**CI/CD Stack only (Jenkins, SonarQube on ControlNode):**
```bash
ansible-playbook -i inventories.txt runbook.yml --limit ControlNode
```

### Dry Run (Check Mode)

Test without making changes:

```bash
ansible-playbook -i inventories.txt runbook.yml --check
```

### Step-by-Step Execution

Run interactively, confirming each task:

```bash
ansible-playbook -i inventories.txt runbook.yml --step
```

## Troubleshooting

### Test Connectivity

Before running the playbook, verify connectivity:

```bash
ansible -i inventories.txt all -m ping
```

### Check Syntax

Validate the playbook syntax:

```bash
ansible-playbook runbook.yml --syntax-check
```

### Run with Sudo Password

If your user needs a password for sudo:

```bash
ansible-playbook -i inventories.txt runbook.yml --ask-become-pass
```

## Expected Execution Flow

1. **Common Setup** runs first on all hosts (monitor + ControlNode)
   - Installs Docker
   - Installs AWS CLI

2. **Monitoring Stack** runs on `monitor` group hosts
   - Installs Prometheus
   - Installs Grafana

3. **CI/CD Stack** runs on `ControlNode` group hosts
   - Installs Jenkins
   - Installs SonarQube

## Post-Installation Access

After successful execution:

- **Grafana**: `http://<monitor-host>:3000` (admin/your_admin_password)
- **Prometheus**: `http://<monitor-host>:9090`
- **Jenkins**: `http://<controlnode-host>:8080`
- **SonarQube**: `http://<controlnode-host>:9000`

## Notes

- The playbook uses `become: true`, so your SSH user must have sudo privileges
- Ensure your inventory file (`inventories.txt`) has the correct IP addresses and credentials
- The first run may take 10-20 minutes depending on network speed and system resources
