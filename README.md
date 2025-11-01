# Ansible Data Science Development Environment Setup

An Ansible playbook that automates the setup of a complete data science development environment on fresh Linux VMs (Ubuntu/Debian). This playbook configures Python, data science libraries, Jupyter Lab, Docker, and various development tools in minutes.

## Features

### Core Components
- **Python Environment**: Python 3.11 with pip and virtualenv
- **Conda**: Miniconda3 with customizable environments
- **Data Science Libraries**:
  - NumPy, Pandas, Scikit-learn
  - TensorFlow, PyTorch, Keras
  - Matplotlib, Seaborn, Plotly
  - XGBoost, LightGBM, CatBoost
  - And 30+ more essential packages

### Development Tools
- **Jupyter Lab**: Pre-configured with extensions and systemd service
- **Docker**: Docker CE with Docker Compose
- **Version Control**: Git with sensible defaults
- **Database Clients**: PostgreSQL and MySQL clients
- **Cloud CLIs**: AWS CLI (optional: GCloud, Azure)
- **Code Editor**: Optional VSCode Server (code-server)

### Additional Features
- Pre-configured directory structure for projects, data, models, and notebooks
- Systemd service for Jupyter Lab
- User-friendly environment variables
- Comprehensive error handling and idempotency

## Prerequisites

### Control Machine (where you run Ansible)
- Ansible 2.9+
- Python 3.6+
- SSH access to target machines

```bash
# Install Ansible on Ubuntu/Debian
sudo apt update
sudo apt install ansible

# Or using pip
pip3 install ansible
```

### Target Machine (VM/Server)
- Fresh Ubuntu 20.04/22.04 or Debian 10/11 installation
- SSH access with sudo privileges
- Minimum 2GB RAM, 10GB disk space (recommended: 4GB RAM, 50GB disk)
- Internet connection for package downloads

## Quick Start

### 1. Clone the Repository

```bash
git clone https://github.com/yourusername/Ansible-for-Dev-Environment-Setup.git
cd Ansible-for-Dev-Environment-Setup
```

### 2. Configure Inventory

Edit `inventory/hosts` to add your target machines:

```ini
[datasience_vms]
vm1 ansible_host=192.168.1.10 ansible_user=ubuntu ansible_ssh_private_key_file=~/.ssh/id_rsa
```

Or for local setup:
```bash
# Use the localhost entry in inventory/hosts
```

### 3. Customize Variables (Optional)

Edit `inventory/group_vars/all.yml` to customize your installation:

```yaml
python_version: "3.11"
install_conda: true
install_docker: true
jupyter_port: 8888
install_aws_cli: true
# ... and more
```

### 4. Run the Playbook

```bash
# For remote VMs
ansible-playbook -i inventory/hosts playbooks/setup-datasience-env.yml

# For local machine
ansible-playbook -i inventory/hosts playbooks/setup-datasience-env.yml --limit localhost

# With specific tags (only install Python and Jupyter)
ansible-playbook -i inventory/hosts playbooks/setup-datasience-env.yml --tags python,jupyter

# Check mode (dry run)
ansible-playbook -i inventory/hosts playbooks/setup-datasience-env.yml --check
```

## Configuration

### Available Variables

Key variables in `inventory/group_vars/all.yml`:

| Variable | Default | Description |
|----------|---------|-------------|
| `python_version` | `3.11` | Python version to install |
| `install_conda` | `true` | Install Miniconda |
| `conda_env_name` | `datasience` | Name of conda environment |
| `install_docker` | `true` | Install Docker |
| `jupyter_port` | `8888` | Jupyter Lab port |
| `install_aws_cli` | `true` | Install AWS CLI |
| `install_gcloud_cli` | `false` | Install Google Cloud SDK |
| `install_azure_cli` | `false` | Install Azure CLI |
| `install_code_server` | `false` | Install VSCode Server |

### Python Packages

Customize the `pip_packages` list in `inventory/group_vars/all.yml` to add or remove packages.

## Usage Examples

### Running with Tags

```bash
# Only install Docker
ansible-playbook -i inventory/hosts playbooks/setup-datasience-env.yml --tags docker

# Install everything except Docker
ansible-playbook -i inventory/hosts playbooks/setup-datasience-env.yml --skip-tags docker

# Update Python packages only
ansible-playbook -i inventory/hosts playbooks/setup-datasience-env.yml --tags python
```

### Multiple Hosts

```bash
# Run on specific host
ansible-playbook -i inventory/hosts playbooks/setup-datasience-env.yml --limit vm1

# Run on all hosts in parallel (use -f for forks)
ansible-playbook -i inventory/hosts playbooks/setup-datasience-env.yml -f 10
```

## Post-Installation

### Starting Jupyter Lab

```bash
# Manual start
jupyter lab --ip=0.0.0.0 --port=8888 --no-browser

# Using the provided script
~/start-jupyter.sh

# As a systemd service (if configured)
sudo systemctl start jupyter
sudo systemctl enable jupyter  # Auto-start on boot
```

Access Jupyter Lab at: `http://your-vm-ip:8888`

### Activating Conda Environment

```bash
# Activate the data science environment
conda activate datasience

# Verify installation
python --version
conda list
pip list
```

### Using Docker

```bash
# Verify Docker installation
docker --version
docker-compose --version

# Run a test container
docker run hello-world

# Note: You may need to log out and back in for group changes to take effect
```

## Project Structure

```
.
├── ansible.cfg                 # Ansible configuration
├── inventory/                  # Inventory files
│   ├── hosts                   # Host definitions
│   └── group_vars/
│       └── all.yml             # Global variables
├── playbooks/
│   └── setup-datasience-env.yml  # Main playbook
├── roles/                      # Ansible roles
│   ├── common/                 # Base system setup
│   ├── python-datascience/     # Python and DS packages
│   ├── docker/                 # Docker installation
│   ├── jupyter/                # Jupyter Lab configuration
│   └── dev-tools/              # Additional development tools
└── README.md                   # This file
```

## Available Roles

### 1. common
Base system configuration, essential tools, and directory structure.

### 2. python-datascience
Python installation, Conda setup, and data science packages.

### 3. docker
Docker CE, Docker Compose, and container management tools.

### 4. jupyter
Jupyter Lab with extensions, systemd service, and configuration.

### 5. dev-tools
Database clients, cloud CLIs, code-server, and development utilities.

## Troubleshooting

### Common Issues

**1. SSH Connection Failed**
```bash
# Test SSH connection
ssh -i ~/.ssh/id_rsa ubuntu@your-vm-ip

# Check inventory configuration
ansible-inventory -i inventory/hosts --list
```

**2. Permission Denied**
```bash
# Ensure your user has sudo privileges
# Add to sudoers: username ALL=(ALL) NOPASSWD:ALL
```

**3. Package Installation Failed**
```bash
# Update apt cache manually
sudo apt update

# Check internet connectivity
ping -c 4 google.com
```

**4. Python Version Not Available**
```bash
# Check available Python versions in deadsnakes PPA
apt-cache search python3.
```

### Verification Commands

```bash
# Check installed versions
python3 --version
conda --version
docker --version
jupyter --version
aws --version

# Verify Jupyter installation
jupyter lab --version
jupyter labextension list

# Check Python packages
pip list | grep -E "(numpy|pandas|tensorflow|torch)"
```

## Advanced Usage

### Custom Variable Files

Create custom variable files for different environments:

```bash
# Create production variables
cat > vars/production.yml <<EOF
install_code_server: true
jupyter_port: 443
EOF

# Run with custom variables
ansible-playbook -i inventory/hosts playbooks/setup-datasience-env.yml -e @vars/production.yml
```

### Using Ansible Vault

Encrypt sensitive variables:

```bash
# Create encrypted file
ansible-vault create inventory/group_vars/vault.yml

# Edit encrypted file
ansible-vault edit inventory/group_vars/vault.yml

# Run playbook with vault
ansible-playbook -i inventory/hosts playbooks/setup-datasience-env.yml --ask-vault-pass
```

### Extending the Playbook

Add custom roles or tasks:

```yaml
# In playbooks/setup-datasience-env.yml
roles:
  - role: common
  - role: python-datascience
  - role: your-custom-role  # Add your role here
```

## Performance Tips

1. **Use Pipelining**: Already enabled in `ansible.cfg`
2. **Parallel Execution**: Use `-f` flag for multiple hosts
3. **Fact Caching**: Already configured in `ansible.cfg`
4. **Targeted Runs**: Use tags to run specific parts
5. **Local Package Cache**: Consider setting up apt-cacher-ng

## Security Considerations

1. **SSH Keys**: Use SSH key authentication, not passwords
2. **Firewall**: Configure UFW or iptables for Jupyter/code-server ports
3. **Jupyter Security**: Set up Jupyter password or token authentication
4. **Docker Security**: Keep Docker updated, use non-root containers
5. **Regular Updates**: Run `apt update && apt upgrade` regularly

## Contributing

Contributions are welcome! Please:

1. Fork the repository
2. Create a feature branch (`git checkout -b feature/amazing-feature`)
3. Commit your changes (`git commit -m 'Add amazing feature'`)
4. Push to the branch (`git push origin feature/amazing-feature`)
5. Open a Pull Request

## License

This project is licensed under the MIT License - see the LICENSE file for details.

## Acknowledgments

- Built for data scientists and machine learning engineers
- Inspired by various data science environment setups
- Uses Ansible best practices and role-based organization

## Support

- **Issues**: [GitHub Issues](https://github.com/yourusername/Ansible-for-Dev-Environment-Setup/issues)
- **Discussions**: [GitHub Discussions](https://github.com/yourusername/Ansible-for-Dev-Environment-Setup/discussions)
- **Documentation**: Check the [Wiki](https://github.com/yourusername/Ansible-for-Dev-Environment-Setup/wiki)

## Roadmap

- [ ] Add support for RHEL/CentOS
- [ ] R and RStudio Server installation
- [ ] GPU support (CUDA, cuDNN)
- [ ] Kubernetes tools (kubectl, helm)
- [ ] Monitoring tools (Prometheus, Grafana)
- [ ] JupyterHub for multi-user environments
- [ ] Automated backup configuration

---

**Happy Data Science! 🚀**
