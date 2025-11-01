# Ansible Data Science Development Environment Setup

A comprehensive Ansible playbook that automates the setup of a complete data science development environment on fresh Linux VMs. Supports **Ubuntu/Debian** and **RHEL/CentOS** distributions with advanced features including GPU support, JupyterHub, R/RStudio, Kubernetes tools, monitoring, and automated backups.

## Features

### Core Components
- **Python Environment**: Python 3.11 with pip and virtualenv
- **Conda**: Miniconda3 with customizable environments
- **Data Science Libraries** (40+ packages):
  - NumPy, Pandas, Scikit-learn
  - TensorFlow, PyTorch, Keras
  - Matplotlib, Seaborn, Plotly
  - XGBoost, LightGBM, CatBoost
  - MLflow, Weights & Biases, Optuna
  - And many more...

### Development Tools
- **Jupyter Lab**: Pre-configured with extensions and systemd service
- **JupyterHub**: Multi-user Jupyter environment (optional)
- **Docker**: Docker CE with Docker Compose
- **Version Control**: Git with sensible defaults
- **Database Clients**: PostgreSQL and MySQL clients
- **Cloud CLIs**: AWS CLI, Google Cloud SDK, Azure CLI
- **Code Editor**: VSCode Server (code-server)

### Advanced Features ⭐
- **R & RStudio Server**: Complete R environment for statistical computing
- **GPU Support**: NVIDIA drivers, CUDA, cuDNN for deep learning
- **Kubernetes Tools**: kubectl, helm, k9s, kubectx for container orchestration
- **Monitoring Stack**: Prometheus, Grafana, Node Exporter
- **Automated Backups**: Local, Borg, or cloud backups with rclone

### System Support
- ✅ Ubuntu 20.04, 22.04, 24.04
- ✅ Debian 10, 11, 12
- ✅ RHEL 8, 9
- ✅ CentOS 8, 9 (Stream)
- ✅ Rocky Linux 8, 9
- ✅ AlmaLinux 8, 9

## Quick Start

### 1. Install Ansible

```bash
# Ubuntu/Debian
sudo apt update && sudo apt install -y ansible

# RHEL/CentOS
sudo yum install -y epel-release
sudo yum install -y ansible

# Or via pip
pip3 install ansible
```

### 2. Clone and Configure

```bash
git clone https://github.com/yourusername/Ansible-for-Dev-Environment-Setup.git
cd Ansible-for-Dev-Environment-Setup

# Install Ansible Galaxy dependencies
ansible-galaxy install -r requirements.yml
```

### 3. Edit Inventory

```ini
# inventory/hosts
[datasience_vms]
vm1 ansible_host=192.168.1.10 ansible_user=ubuntu ansible_ssh_private_key_file=~/.ssh/id_rsa
```

### 4. Customize Variables

Edit `inventory/group_vars/all.yml`:

```yaml
# Basic setup
python_version: "3.11"
install_conda: true
install_docker: true

# Enable advanced features
install_r: true              # R and RStudio Server
install_gpu_support: true    # NVIDIA GPU support
install_jupyterhub: false    # Multi-user Jupyter
install_k8s_tools: true      # Kubernetes tools
install_monitoring: true     # Prometheus & Grafana
install_backup: true         # Automated backups
```

### 5. Run the Playbook

```bash
# Full installation
ansible-playbook -i inventory/hosts playbooks/setup-datasience-env.yml

# For local machine
ansible-playbook -i inventory/hosts playbooks/setup-datasience-env.yml --limit localhost -K

# With specific components
ansible-playbook -i inventory/hosts playbooks/setup-datasience-env.yml --tags python,docker,gpu

# Dry run first
ansible-playbook -i inventory/hosts playbooks/setup-datasience-env.yml --check
```

## Configuration Options

### Core Settings

| Variable | Default | Description |
|----------|---------|-------------|
| `python_version` | `3.11` | Python version to install |
| `install_conda` | `true` | Install Miniconda |
| `conda_env_name` | `datasience` | Conda environment name |
| `install_docker` | `true` | Install Docker CE |
| `jupyter_port` | `8888` | Jupyter Lab port |

### Advanced Features

#### JupyterHub (Multi-User)
```yaml
install_jupyterhub: true
jupyterhub_port: 8000
jupyterhub_admin_users:
  - admin
install_nginx_proxy: true
```

#### R and RStudio Server
```yaml
install_r: true
install_rstudio: true
rstudio_port: 8787
r_packages:
  - tidyverse
  - ggplot2
  - shiny
  # ... more packages
```

#### GPU Support
```yaml
install_gpu_support: true
nvidia_driver_version: "535"
cuda_version: "12.3"
install_cudnn: false  # Requires manual download
install_tensorflow_gpu: true
install_pytorch_gpu: true
```

#### Kubernetes Tools
```yaml
install_k8s_tools: true
k8s_version: "1.28"
install_k9s: true
install_kubectx: true
install_krew: true
```

#### Monitoring
```yaml
install_monitoring: true
prometheus_port: 9090
grafana_port: 3000
install_node_exporter: true
install_grafana: true
```

#### Automated Backups
```yaml
install_backup: true
backup_method: "local"  # or "borg" or "rclone"
backup_base_dir: "/backups"
enable_backup_cron: true
backup_cron_hour: "2"
```

## Usage Examples

### Tag-Based Installation

```bash
# Install only base system
ansible-playbook -i inventory/hosts playbooks/setup-datasience-env.yml --tags base,common

# Install Python and data science tools
ansible-playbook -i inventory/hosts playbooks/setup-datasience-env.yml --tags python,datascience

# Install monitoring stack
ansible-playbook -i inventory/hosts playbooks/setup-datasience-env.yml --tags monitoring

# Install everything except GPU support
ansible-playbook -i inventory/hosts playbooks/setup-datasience-env.yml --skip-tags gpu
```

### Available Tags

- `base`, `common` - Base system setup
- `python`, `datascience` - Python and DS packages
- `docker`, `containers` - Docker installation
- `jupyter`, `notebook` - Jupyter Lab
- `jupyterhub`, `multi-user` - JupyterHub
- `r`, `rstudio` - R and RStudio Server
- `gpu`, `cuda`, `nvidia` - GPU support
- `kubernetes`, `k8s`, `kubectl` - K8s tools
- `monitoring`, `prometheus`, `grafana` - Monitoring
- `backup` - Backup configuration
- `dev-tools`, `tools` - Development utilities

## Post-Installation

### Access Services

```bash
# Jupyter Lab
http://your-server:8888

# JupyterHub (if installed)
http://your-server:8000

# RStudio Server (if installed)
http://your-server:8787

# Grafana (if installed)
http://your-server:3000  # admin/admin

# Prometheus (if installed)
http://your-server:9090
```

### Verify Installation

```bash
# Python and packages
conda activate datasience
python --version
pip list

# Docker
docker --version
docker run hello-world

# GPU (if installed)
~/verify_gpu.py

# Kubernetes tools
kubectl version --client
helm version
k9s version

# Backup system
sudo datasience-backup
```

### Common Commands

```bash
# Start Jupyter Lab
jupyter lab --ip=0.0.0.0 --port=8888 --no-browser

# Activate conda environment
conda activate datasience

# Create JupyterHub user
sudo useradd -m -s /bin/bash -G jupyterhub-users username
sudo passwd username

# Run manual backup
sudo datasience-backup

# Restore from backup
sudo datasience-restore /backups/backup_TIMESTAMP.tar.gz

# Check Prometheus status
sudo systemctl status prometheus

# View Grafana logs
sudo journalctl -u grafana-server -f
```

## Project Structure

```
.
├── ansible.cfg                     # Ansible configuration
├── inventory/
│   ├── hosts                       # Host definitions
│   └── group_vars/
│       └── all.yml                 # Global variables
├── playbooks/
│   └── setup-datasience-env.yml    # Main playbook
├── roles/
│   ├── common/                     # Base system setup
│   ├── python-datascience/         # Python and DS packages
│   ├── docker/                     # Docker installation
│   ├── jupyter/                    # Jupyter Lab
│   ├── jupyterhub/                 # JupyterHub multi-user
│   ├── r-rstudio/                  # R and RStudio Server
│   ├── gpu-support/                # NVIDIA GPU support
│   ├── kubernetes-tools/           # K8s tools
│   ├── monitoring/                 # Prometheus & Grafana
│   ├── backup/                     # Backup configuration
│   └── dev-tools/                  # Development utilities
├── requirements.yml                # Ansible Galaxy dependencies
├── README.md                       # This file
└── QUICKSTART.md                   # Quick start guide
```

## Advanced Topics

### GPU Support Setup

1. **Enable GPU support:**
   ```yaml
   install_gpu_support: true
   ```

2. **Run the playbook:**
   ```bash
   ansible-playbook -i inventory/hosts playbooks/setup-datasience-env.yml --tags gpu
   ```

3. **Reboot the system** for driver changes to take effect

4. **Verify GPU detection:**
   ```bash
   nvidia-smi
   ~/verify_gpu.py
   ```

### JupyterHub Multi-User Setup

1. **Enable JupyterHub:**
   ```yaml
   install_jupyterhub: true
   jupyterhub_admin_users:
     - admin
   ```

2. **Create users:**
   ```bash
   sudo useradd -m -s /bin/bash -G jupyterhub-users alice
   sudo passwd alice
   ```

3. **Access JupyterHub** at `http://your-server:8000`

### Backup Configuration

**Local Backups:**
```yaml
backup_method: "local"
backup_base_dir: "/backups"
enable_backup_cron: true
```

**Borg Backups:**
```yaml
backup_method: "borg"
borg_repo: "/backups/borg-repo"
borg_passphrase: "your-secure-passphrase"
```

**Cloud Backups with Rclone:**
```yaml
backup_method: "rclone"
rclone_remote: "s3"
rclone_remote_path: "my-bucket/datasience-backups"
```

Configure rclone: `~/.config/rclone/rclone.conf`

### Monitoring Setup

Access Grafana at `http://your-server:3000` (admin/admin)

1. Add Prometheus data source: `http://localhost:9090`
2. Import dashboards for:
   - Node Exporter (Dashboard ID: 1860)
   - Docker (Dashboard ID: 893)
   - NVIDIA GPU (Dashboard ID: 12239)

## Troubleshooting

### Common Issues

**SSH Connection Failed:**
```bash
ansible -i inventory/hosts all -m ping
ssh ubuntu@your-server
```

**Permission Denied:**
```bash
# Use -K flag for sudo password
ansible-playbook -i inventory/hosts playbooks/setup-datasience-env.yml -K
```

**GPU Not Detected:**
```bash
lspci | grep -i nvidia
nvidia-smi
~/verify_gpu.py
```

**Package Installation Failed:**
```bash
# Check internet connectivity
ping -c 4 google.com

# Update package cache
sudo apt update  # Ubuntu/Debian
sudo yum update  # RHEL/CentOS
```

**Jupyter Not Starting:**
```bash
jupyter lab --ip=0.0.0.0 --port=8888 --no-browser
journalctl -u jupyter -f
```

**Docker Permission Issues:**
```bash
# Add user to docker group
sudo usermod -aG docker $USER
# Log out and back in
```

### Logs and Debugging

```bash
# Ansible verbose output
ansible-playbook -i inventory/hosts playbooks/setup-datasience-env.yml -vvv

# Check service status
sudo systemctl status jupyter
sudo systemctl status jupyterhub
sudo systemctl status prometheus
sudo systemctl status grafana-server
sudo systemctl status docker

# View logs
journalctl -u jupyter -f
journalctl -u jupyterhub -f
tail -f /var/log/jupyterhub/jupyterhub.log
```

## Security Best Practices

1. **SSH Keys:** Use SSH key authentication, disable password auth
2. **Firewall:** Configure UFW/firewalld for exposed ports
3. **SSL/TLS:** Use Let's Encrypt for HTTPS (Nginx reverse proxy)
4. **Jupyter Security:** Set up password/token authentication
5. **Regular Updates:** Keep system and packages updated
6. **Backup Encryption:** Use encrypted backups (Borg with encryption)
7. **User Permissions:** Follow principle of least privilege
8. **Secrets Management:** Use Ansible Vault for sensitive data

```bash
# Example: Encrypt sensitive variables
ansible-vault create inventory/group_vars/vault.yml
ansible-playbook -i inventory/hosts playbooks/setup-datasience-env.yml --ask-vault-pass
```

## Performance Tuning

1. **Parallel Execution:** Use `-f` flag for multiple hosts
   ```bash
   ansible-playbook -i inventory/hosts playbooks/setup-datasience-env.yml -f 10
   ```

2. **Fact Caching:** Already configured in `ansible.cfg`

3. **Pipelining:** Enabled by default for faster execution

4. **Package Caching:** Consider apt-cacher-ng for multiple VMs

5. **GPU Memory:** Configure TensorFlow/PyTorch memory growth

## Contributing

Contributions are welcome! Please:

1. Fork the repository
2. Create a feature branch (`git checkout -b feature/amazing-feature`)
3. Test your changes on both Ubuntu and RHEL
4. Commit with clear messages
5. Push and open a Pull Request

### Development

```bash
# Test on local VM
vagrant up
ansible-playbook -i inventory/hosts playbooks/setup-datasience-env.yml --limit localhost

# Lint playbook
ansible-lint playbooks/setup-datasience-env.yml

# Test specific role
ansible-playbook -i inventory/hosts playbooks/setup-datasience-env.yml --tags gpu --check
```

## Roadmap

- [x] RHEL/CentOS support
- [x] R and RStudio Server
- [x] GPU support (CUDA, cuDNN)
- [x] Kubernetes tools
- [x] Monitoring (Prometheus, Grafana)
- [x] JupyterHub multi-user
- [x] Automated backups
- [ ] ARM64 architecture support
- [ ] Airflow for workflow orchestration
- [ ] Spark cluster setup
- [ ] MLOps tools (Kubeflow, MLflow server)
- [ ] SSL/TLS automation with Let's Encrypt

## License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

## Acknowledgments

- Built for data scientists, ML engineers, and researchers
- Inspired by best practices from the data science community
- Uses Ansible role-based organization for modularity

## Support

- **Issues:** [GitHub Issues](https://github.com/olaflaitinen/Ansible-for-Dev-Environment-Setup/issues)
- **Discussions:** [GitHub Discussions](https://github.com/olaflaitinen/Ansible-for-Dev-Environment-Setup/discussions)

## Related Projects

- [JupyterHub](https://jupyter.org/hub)
- [Ansible](https://www.ansible.com/)
- [NVIDIA CUDA](https://developer.nvidia.com/cuda-toolkit)
- [Prometheus](https://prometheus.io/)
- [Grafana](https://grafana.com/)

---

**Built with ❤️ for the Data Science Community**

*Last Updated: {{ ansible_date_time.date | default('2025-01-01') }}*
