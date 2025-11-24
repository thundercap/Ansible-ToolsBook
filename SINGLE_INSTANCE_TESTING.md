# Testing on Single Instance - Quick Start

## Your Setup
- **Single Server**: 18.206.81.33
- **All Services**: Docker, AWS CLI, Prometheus, Grafana, Jenkins, SonarQube
- **User**: ubuntu
- **SSH Key**: /home/paul/ansible.pem

## Pre-Flight Checks

1. **Verify SSH access**:
   ```bash
   ssh -i /home/paul/ansible.pem ubuntu@18.206.81.33
   ```

2. **Test Ansible connectivity**:
   ```bash
   ansible -i inventories.txt all -m ping
   ```

## Run the Playbook

```bash
ansible-playbook -i inventories.txt runbook.yml -v
```

## What Will Be Installed

On your single instance (18.206.81.33), you'll get:

1. **Docker** + Docker Compose
2. **AWS CLI v2**
3. **Prometheus** (port 9090)
4. **Grafana** (port 3000)
5. **Jenkins** (port 8080)
6. **SonarQube** (port 9000)

## After Installation - Access Your Services

```bash
# Grafana
http://18.206.81.33:3000
Username: admin
Password: your_admin_password (set in grafana/defaults/main.yml)

# Prometheus
http://18.206.81.33:9090

# Jenkins
http://18.206.81.33:8080
# Get initial admin password:
ssh -i /home/paul/ansible.pem ubuntu@18.206.81.33 "sudo cat /var/lib/jenkins/secrets/initialAdminPassword"

# SonarQube
http://18.206.81.33:9000
Username: admin
Password: admin (change on first login)
```

## Estimated Time
- First run: ~15-20 minutes
- Subsequent runs: ~5-10 minutes (idempotent)

## Troubleshooting

If the playbook fails:

1. **Check SSH key permissions**:
   ```bash
   chmod 600 /home/paul/ansible.pem
   ```

2. **Ensure security group allows ports**:
   - 22 (SSH)
   - 3000 (Grafana)
   - 8080 (Jenkins)
   - 9000 (SonarQube)
   - 9090 (Prometheus)

3. **Run with more verbosity**:
   ```bash
   ansible-playbook -i inventories.txt runbook.yml -vvv
   ```

## System Requirements

Your instance should have at least:
- **2 vCPUs**
- **4 GB RAM** (8 GB recommended for all services)
- **20 GB disk space**

## Clean Up (if needed)

To remove everything:
```bash
# Stop all containers
ssh -i /home/paul/ansible.pem ubuntu@18.206.81.33 "docker stop \$(docker ps -aq)"

# Remove all containers
ssh -i /home/paul/ansible.pem ubuntu@18.206.81.33 "docker rm \$(docker ps -aq)"
```
