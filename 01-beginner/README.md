
# Ansible Beginner Essentials

A comprehensive collection of simple, clean, and well-commented Ansible playbooks designed to help beginners learn Ansible step-by-step. Each playbook demonstrates specific Ansible concepts with practical examples.

## 📦 Prerequisites
- **Python 3.x** installed on your system
- **Ansible** installed (`pip install ansible`)
- Basic understanding of YAML syntax
- SSH access to target hosts (or use localhost for testing)

## 🚀 Quick Start
```bash
# Clone the repository
git clone <repository-url>
cd ansible

# Test the setup with a simple ping
ansible-playbook -i inventory.ini 01_ping_hosts.yml

# Run any playbook using this pattern
ansible-playbook -i inventory.ini <playbook-name>.yml
```

## 📂 Repository Structure

### Core Configuration Files
- **`ansible.cfg`** - Ansible configuration settings
- **`inventory.ini`** - Host inventory (configured for localhost testing)

### 📖 Playbooks (Beginner to Intermediate)

| File | Concept | Description |
|------|---------|-------------|
| `01_ping_hosts.yml` | **Basic Connectivity** | Test connection to all hosts using the ping module |
| `02_gather_facts.yml` | **Fact Gathering** | Collect and display system information from target hosts |
| `03_copy_file.yml` | **File Operations** | Copy files from local machine to remote hosts |
| `04_install_package.yml` | **Package Management** | Install packages with OS-specific conditionals |
| `05_manage_service.yml` | **Service Management** | Start, stop, and manage system services |
| `06_create_user.yml` | **User Management** | Create and manage system users |
| `07_file_permissions.yml` | **File System** | Create directories and set file permissions |
| `08_template_file.yml` | **Templating** | Deploy Jinja2 templates to remote hosts |
| `09_loop_example.yml` | **Loops** | Install multiple packages using loops |
| `10_when_condition.yml` | **Conditionals** | Execute tasks based on conditions |
| `11_vars_and_vault.yml` | **Variables** | Use variables in playbooks |
| `12_roles_demo.yml` | **Roles** | Demonstrate role-based playbook organization |

### 📁 Supporting Directories

#### `files/`
- **`sample.txt`** - Sample file for copy operations
- **`sample_template.j2`** - Jinja2 template example

#### `roles/sample_role/`
- **`tasks/main.yml`** - Role task definitions
- **`templates/sample_template.j2`** - Role-specific templates
- **`vars/main.yml`** - Role variables

## 🎯 Learning Path

### Phase 1: Basic Operations (Playbooks 1-3)
Start with connectivity testing, fact gathering, and simple file operations.

### Phase 2: System Management (Playbooks 4-7)
Learn package management, service control, user creation, and file permissions.

### Phase 3: Advanced Concepts (Playbooks 8-12)
Master templating, loops, conditionals, variables, and roles.

## 💡 Usage Examples

### Running Individual Playbooks
```bash
# Test connectivity
ansible-playbook -i inventory.ini 01_ping_hosts.yml

# Install packages with privilege escalation
ansible-playbook -i inventory.ini 04_install_package.yml --ask-become-pass

# Deploy templates
ansible-playbook -i inventory.ini 08_template_file.yml
```

### Using Different Inventory
```bash
# Use custom inventory file
ansible-playbook -i my_inventory.ini 01_ping_hosts.yml

# Target specific hosts
ansible-playbook -i inventory.ini --limit webservers 04_install_package.yml
```

## 🔧 Customization

### Modifying Inventory
Edit `inventory.ini` to add your target hosts:
```ini
[webservers]
web1.example.com
web2.example.com

[databases]
db1.example.com
```

### Ansible Configuration
The `ansible.cfg` file includes sensible defaults:
- Host key checking disabled (for testing)
- Retry files disabled
- Custom roles path configured

## ⚠️ Important Notes

1. **Privilege Escalation**: Playbooks 4-7 require `sudo` privileges. Use `--ask-become-pass` flag when running these playbooks.

2. **OS Compatibility**: Some playbooks include OS-specific tasks (e.g., `04_install_package.yml` handles both Debian and RedHat families).

3. **Testing Environment**: The default inventory is configured for localhost testing. Modify `inventory.ini` for production use.

4. **File Dependencies**: Ensure the `files/` directory and its contents are present for playbooks that copy or template files.

## 🚨 Security Considerations

- Remove `host_key_checking = False` from `ansible.cfg` in production
- Use Ansible Vault for sensitive variables
- Implement proper SSH key management
- Review and test all playbooks before production deployment

## 📚 Next Steps

After completing these examples, consider exploring:
- **Ansible Galaxy** for community roles
- **Ansible Vault** for secrets management  
- **Dynamic Inventories** for cloud environments
- **Custom Modules** for specific requirements
- **Ansible AWX/Tower** for enterprise automation

## 🤝 Contributing

Feel free to submit issues, feature requests, or improvements to help other Ansible beginners!

---
*This collection is designed for educational purposes. Always test in a safe environment before applying to production systems.*
