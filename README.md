# Ansible Learning Collection

A comprehensive collection of Ansible playbooks organized by skill level, designed to help you learn Ansible from basic concepts to advanced real-world automation scenarios.

## 📂 Repository Structure

This repository is organized into two main learning paths:

### 🎓 [01-beginner/](./01-beginner/)
**Perfect for getting started with Ansible**

- **12 essential playbooks** covering fundamental concepts
- Step-by-step progression from basic connectivity to roles
- Well-commented examples with clear learning objectives
- Configured for localhost testing to get started immediately

**Key Topics**: Ping, fact gathering, file operations, package management, services, users, permissions, templating, loops, conditionals, variables, and roles.

### 🚀 [02-advanced/](./02-advanced/)
**Real-world automation scenarios**

- **12 production-oriented playbooks** for advanced use cases
- Container management, cloud provisioning, security, and CI/CD
- Includes custom roles for complex automation tasks
- Templates for AWS, Kubernetes, and enterprise tools

**Key Topics**: Docker, Kubernetes, AWS EC2/S3, secrets management, SSL configuration, database setup, system patching, and rolling deployments.

## 🎯 Getting Started

### For Ansible Beginners
Start with the **[01-beginner/](./01-beginner/)** directory:
```bash
cd 01-beginner/
ansible-playbook -i inventory.ini 01_ping_hosts.yml
```

### For Experienced Users
Jump to **[02-advanced/](./02-advanced/)** for production scenarios:
```bash
cd 02-advanced/
ansible-playbook -i inventory.ini 01_docker_install.yml
```

## 📋 Prerequisites

- **Python 3.x**
- **Ansible 4.0+** (`pip install ansible`)
- **SSH access** to target hosts (or use localhost for learning)
- For advanced playbooks: Additional collections and cloud provider credentials

## 📖 Learning Path

1. **Start** → [01-beginner/](./01-beginner/) for fundamentals
2. **Progress** → [02-advanced/](./02-advanced/) for real-world applications
3. **Explore** → Modify playbooks for your specific environment

## 🤝 Contributing

Each directory has its own detailed README with specific contribution guidelines. Feel free to:
- Add new beginner-friendly examples
- Improve advanced playbook implementations
- Fix issues and enhance documentation

## 📄 License

This collection is provided for educational purposes. Please review each directory's documentation for specific requirements and limitations.

---

**Quick Start**: Choose your path → [Beginner](./01-beginner/) | [Advanced](./02-advanced/)
