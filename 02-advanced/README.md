# Ansible Advanced Playbooks Collection

This repository contains a comprehensive collection of advanced Ansible playbooks demonstrating real-world automation scenarios including Docker, Kubernetes, cloud provisioning, security management, and CI/CD operations.

## 📋 Table of Contents
- [Overview](#overview)
- [Prerequisites](#prerequisites)
- [Installation](#installation)
- [Playbook Descriptions](#playbook-descriptions)
- [Usage Examples](#usage-examples)
- [Roles Documentation](#roles-documentation)
- [Known Issues & Limitations](#known-issues--limitations)
- [Troubleshooting](#troubleshooting)
- [Contributing](#contributing)

## 🎯 Overview

This collection includes 12 playbooks covering:
- **Container Management**: Docker installation and deployment
- **Orchestration**: Kubernetes application deployment
- **Cloud Services**: AWS EC2, S3 operations (templates)
- **Security**: Secrets management, firewall rules, SSL configuration
- **Infrastructure**: Database setup, web server configuration
- **Operations**: System patching, CI/CD deployment, rolling updates

## 🔧 Prerequisites

### System Requirements
- **Python**: 3.8+ 
- **Ansible**: 4.0+ (recommended: latest stable)
- **Operating Systems**: Ubuntu/Debian (primary), RHEL/CentOS (partial support)

### Required Collections
```bash
# Install required Ansible collections
ansible-galaxy collection install community.docker
ansible-galaxy collection install community.kubernetes
ansible-galaxy collection install ansible.posix
ansible-galaxy collection install amazon.aws  # For AWS playbooks
```

### Python Dependencies
```bash
# Install required Python packages
pip install ansible boto3 kubernetes hvac
```

### Cloud Provider Setup
- **AWS**: Configure AWS credentials (`aws configure` or environment variables)
- **Kubernetes**: Ensure valid kubeconfig at `~/.kube/config`
- **HashiCorp Vault**: Set `VAULT_TOKEN` environment variable

## 📦 Installation

1. **Clone the repository**
```bash
git clone <repository-url>
cd ansible/02-advanced
```

2. **Install dependencies**
```bash
# Install Ansible collections
ansible-galaxy install -r requirements.yml  # Create this file if needed

# Install Python packages
pip install -r requirements.txt  # Create this file if needed
```

3. **Configure inventory**
```bash
# Edit inventory.ini to add your target hosts
cp inventory.ini inventory.ini.backup
# Add your servers under appropriate groups
```

## 📖 Playbook Descriptions

### ✅ Production-Ready Playbooks

| Playbook | Purpose | Status | Dependencies |
|----------|---------|---------|--------------|
| `01_docker_install.yml` | Install Docker Engine and start service | ✅ Complete | docker_role |
| `02_docker_deploy_container.yml` | Deploy Nginx container with port mapping | ✅ Complete | community.docker |
| `03_k8s_deploy_app.yml` | Deploy application to Kubernetes cluster | ✅ Complete | kubernetes, k8s_role |
| `07_mysql_db_setup.yml` | Install and configure MySQL database | ✅ Complete | Debian/Ubuntu |
| `08_nginx_ssl_setup.yml` | Install Nginx with SSL configuration | ⚠️ Needs SSL certs | nginx_ssl_role |
| `09_firewall_rules.yml` | Configure UFW firewall rules | ✅ Complete | ansible.posix |
| `10_system_patch_update.yml` | Update system packages | ✅ Complete | Multiple OS support |
| `12_rolling_update.yml` | Rolling deployment for web servers | ✅ Complete | Git repository |

### 🚧 Template/Placeholder Playbooks

| Playbook | Purpose | Status | Notes |
|----------|---------|---------|-------|
| `04_manage_secrets_vault.yml` | HashiCorp Vault integration | 🚧 Template | Requires Vault server setup |
| `05_ec2_provision.yml` | AWS EC2 instance provisioning | 🚧 Template | Needs AWS credentials |
| `06_s3_upload_download.yml` | AWS S3 file operations | 🚧 Template | Needs AWS credentials |
| `11_jenkins_install.yml` | Jenkins CI/CD server setup | 🚧 Template | Incomplete implementation |

## 🚀 Usage Examples

### Basic Execution
```bash
# Run a simple playbook
ansible-playbook -i inventory.ini 01_docker_install.yml

# Run with verbose output
ansible-playbook -i inventory.ini -vvv 02_docker_deploy_container.yml

# Run with sudo privileges
ansible-playbook -i inventory.ini --become 07_mysql_db_setup.yml
```

### Advanced Usage
```bash
# Target specific hosts
ansible-playbook -i inventory.ini --limit webservers 12_rolling_update.yml

# Override variables
ansible-playbook -i inventory.ini -e "app_name=my-app" 03_k8s_deploy_app.yml

# Check mode (dry run)
ansible-playbook -i inventory.ini --check 10_system_patch_update.yml

# Run specific tags (if implemented)
ansible-playbook -i inventory.ini --tags "install" 01_docker_install.yml
```

### Production Deployment Example
```bash
# Complete web server setup
ansible-playbook -i production.ini 01_docker_install.yml
ansible-playbook -i production.ini 08_nginx_ssl_setup.yml
ansible-playbook -i production.ini 09_firewall_rules.yml
```

## 🏗️ Roles Documentation

### docker_role
**Purpose**: Install Docker Engine and dependencies
**Variables**: `docker_packages` (default: `[docker.io]`)
**Requirements**: Ubuntu/Debian system with sudo access

### k8s_role  
**Purpose**: Deploy applications to Kubernetes
**Variables**: `kubeconfig`, `app_name`, `image`
**Requirements**: Valid kubeconfig, community.kubernetes collection

### nginx_ssl_role
**Purpose**: Install Nginx with SSL configuration
**Files Required**: 
- `files/sample_cert.crt` (SSL certificate)
- `files/sample_key.key` (SSL private key)
**Note**: Creates self-signed certificates if files missing

## ⚠️ Known Issues & Limitations

### Critical Issues
1. **Missing SSL Certificates**: `nginx_ssl_role` references certificate files that don't exist
   ```bash
   # Create self-signed certificates for testing
   openssl req -x509 -newkey rsa:4096 -keyout files/sample_key.key -out files/sample_cert.crt -days 365 -nodes
   ```

2. **Nginx Configuration Template**: `nginx.conf.j2` template is not deployed by the role
   - **Solution**: Add template deployment task to nginx_ssl_role

3. **Placeholder Playbooks**: Several playbooks contain only debug messages
   - Playbooks 04, 05, 06, 11 need full implementation

### Limitations
- **OS Support**: Primarily tested on Ubuntu/Debian systems
- **Inventory**: Default inventory only includes localhost
- **Error Handling**: Limited error handling and rollback procedures
- **Testing**: No molecule tests or CI/CD validation

### Security Considerations
- SSL certificates should be properly managed (not in version control)
- Vault tokens should use proper authentication methods
- AWS credentials require proper IAM policies
- Database passwords should be encrypted with ansible-vault

## 🔧 Troubleshooting

### Common Issues

**1. Collection Not Found**
```bash
# Error: couldn't resolve module/action 'community.docker.docker_container'
ansible-galaxy collection install community.docker
```

**2. Permission Denied**
```bash
# Add --become flag for privilege escalation
ansible-playbook -i inventory.ini --become playbook.yml
```

**3. SSH Connection Issues**
```bash
# Disable host key checking (testing only)
export ANSIBLE_HOST_KEY_CHECKING=False
```

**4. Python Module Missing**
```bash
# Install on target hosts
ansible all -i inventory.ini -m pip -a "name=docker" --become
```

**5. Kubernetes Connection**
```bash
# Verify kubeconfig
kubectl config current-context
# Test connection
ansible-playbook -i inventory.ini --check 03_k8s_deploy_app.yml
```

### Debug Commands
```bash
# Test connectivity
ansible all -i inventory.ini -m ping

# Gather facts
ansible all -i inventory.ini -m setup

# Check disk space
ansible all -i inventory.ini -a "df -h"

# Verify installed packages
ansible all -i inventory.ini -a "docker --version"
```

## 🤝 Contributing

### Development Guidelines
1. **Follow Ansible Best Practices**
   - Use meaningful task names
   - Implement proper error handling
   - Add appropriate tags
   - Use variables for configuration

2. **Testing Requirements**
   - Test on multiple OS distributions
   - Verify idempotency
   - Add molecule tests where applicable

3. **Documentation**
   - Update README for new playbooks
   - Document role variables
   - Include usage examples

### Improvement Roadmap
- [ ] Complete placeholder playbooks (04, 05, 06, 11)
- [ ] Add SSL certificate generation to nginx_ssl_role
- [ ] Implement comprehensive error handling
- [ ] Add support for additional operating systems
- [ ] Create group_vars and host_vars structure
- [ ] Add Molecule testing framework
- [ ] Implement Ansible Vault for sensitive data
- [ ] Add CI/CD pipeline for testing

### File Structure
```
02-advanced/
├── ansible.cfg                 # Ansible configuration
├── inventory.ini              # Host inventory
├── files/                     # Static files
│   └── index.html
├── roles/                     # Ansible roles
│   ├── docker_role/
│   ├── k8s_role/
│   └── nginx_ssl_role/
├── 01_docker_install.yml      # Docker installation
├── 02_docker_deploy_container.yml
├── 03_k8s_deploy_app.yml
├── ...                        # Additional playbooks
└── README.md                  # This file
```

## 📄 License

This project is provided as-is for educational and demonstration purposes. Please review and adapt for your specific environment and security requirements.

---

**⚡ Quick Start**: `ansible-playbook -i inventory.ini 01_docker_install.yml`

For questions or issues, please check the troubleshooting section or create an issue in the repository.