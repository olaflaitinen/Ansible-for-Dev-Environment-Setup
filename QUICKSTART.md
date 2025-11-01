# Quick Start Guide

Get your data science environment up and running in 5 minutes!

## Prerequisites Check

```bash
# Check Ansible installation
ansible --version

# If not installed, install Ansible
sudo apt update && sudo apt install -y ansible
# OR
pip3 install ansible
```

## Step-by-Step Setup

### 1. Clone the Repository

```bash
git clone https://github.com/yourusername/Ansible-for-Dev-Environment-Setup.git
cd Ansible-for-Dev-Environment-Setup
```

### 2. Install Ansible Dependencies (Optional but Recommended)

```bash
ansible-galaxy install -r requirements.yml
```

### 3. Configure for Local Machine

For a quick local setup, use the default configuration:

```bash
# No changes needed - localhost is already configured
```

For remote machines, edit `inventory/hosts`:

```bash
nano inventory/hosts
```

Add your VM:
```ini
[datasience_vms]
myvm ansible_host=192.168.1.10 ansible_user=ubuntu ansible_ssh_private_key_file=~/.ssh/id_rsa
```

### 4. Test Connection

```bash
# For local setup
ansible -i inventory/hosts localhost -m ping

# For remote setup
ansible -i inventory/hosts myvm -m ping
```

### 5. Run the Playbook

```bash
# Full installation on local machine (will prompt for sudo password)
ansible-playbook -i inventory/hosts playbooks/setup-datasience-env.yml --limit localhost -K

# For remote machine
ansible-playbook -i inventory/hosts playbooks/setup-datasience-env.yml --limit myvm

# Dry run first (recommended)
ansible-playbook -i inventory/hosts playbooks/setup-datasience-env.yml --limit localhost --check
```

## What Gets Installed

- ✅ Python 3.11 with pip
- ✅ Miniconda with datasience environment
- ✅ 40+ data science packages (pandas, numpy, scikit-learn, tensorflow, pytorch, etc.)
- ✅ Jupyter Lab with extensions
- ✅ Docker & Docker Compose
- ✅ Git, vim, tmux, and essential dev tools
- ✅ PostgreSQL and MySQL clients
- ✅ AWS CLI
- ✅ Pre-configured directory structure

## Post-Installation

### Start Jupyter Lab

```bash
# Option 1: Using the convenience script
~/start-jupyter.sh

# Option 2: Manual start
jupyter lab --ip=0.0.0.0 --port=8888 --no-browser

# Option 3: As a service (needs configuration)
sudo systemctl start jupyter
```

Access at: `http://localhost:8888` (or your VM's IP)

### Activate Conda Environment

```bash
source ~/miniconda3/bin/activate
conda activate datasience

# Verify
python --version
conda list
```

### Verify Docker

```bash
docker --version
docker run hello-world
```

## Quick Customization

Want to change what gets installed? Edit `inventory/group_vars/all.yml`:

```yaml
# Disable Docker installation
install_docker: false

# Change Python version
python_version: "3.10"

# Disable AWS CLI
install_aws_cli: false

# Change Jupyter port
jupyter_port: 9999
```

Then re-run the playbook.

## Troubleshooting

**Connection refused?**
```bash
# Check SSH access first
ssh ubuntu@your-vm-ip

# Verify inventory
ansible-inventory -i inventory/hosts --list
```

**Permission denied?**
```bash
# Add -K flag to prompt for sudo password
ansible-playbook -i inventory/hosts playbooks/setup-datasience-env.yml -K
```

**Playbook fails partway?**
```bash
# Ansible is idempotent - just run it again
ansible-playbook -i inventory/hosts playbooks/setup-datasience-env.yml
```

## Next Steps

- Read the full [README.md](README.md) for advanced usage
- Customize variables in `inventory/group_vars/all.yml`
- Add your own roles in the `roles/` directory
- Set up Jupyter password protection
- Configure firewall rules for exposed ports

## Need Help?

- Check the [README.md](README.md) for detailed documentation
- Review role-specific tasks in `roles/*/tasks/main.yml`
- Open an issue on GitHub

Happy coding! 🎉
