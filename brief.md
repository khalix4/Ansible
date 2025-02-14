Absolutely! Below is a **comprehensive documentation** on how to provision a server using the **Ansible setup** we've structured.  

---

# **🚀 Ansible Server Provisioning Guide**
This guide details how to provision a secure server with **Docker, Coolify, Monitoring (Prometheus + Grafana), and UFW Firewall** using Ansible.  

---

## **📌 1. Prerequisites**
### **🔹 Install Ansible on Your Local Machine**
Run the following command based on your OS:  

**Ubuntu/Debian:**  
```sh
sudo apt update && sudo apt install -y ansible
```
**MacOS:**  
```sh
brew install ansible
```
**Windows (via WSL):**  
Install Ubuntu WSL and follow the Ubuntu steps.

---

### **🔹 Ensure SSH Access to the Server**
Make sure you can SSH into the server:  
```sh
ssh root@your_server_ip
```
If using an SSH key, ensure it's configured:  
```sh
ssh-add ~/.ssh/id_rsa
```

---

## **📌 2. Project Structure**
```
ansible-project/
│── inventory.ini       # Defines target servers
│── playbook.yml        # Main playbook
│── group_vars/         # Global variables
│   ├── all.yml
│── host_vars/          # Per-host variables (optional)
│── roles/              # Modular roles
│   ├── common/         # Basic setup (packages)
│   ├── docker/         # Docker installation
│   ├── ufw/            # Firewall rules
│   ├── monitoring/     # Node Exporter, Prometheus, Grafana
│   ├── coolify/        # Coolify setup
│── ansible.cfg         # Optional Ansible configuration
```

---

## **📌 3. Configuration Files**
### **🔹 Inventory File (`inventory.ini`)**
Defines the target server(s).
```ini
[servers]
your_server_ip ansible_user=root ansible_ssh_private_key_file=~/.ssh/id_rsa
```

### **🔹 Global Variables (`group_vars/all.yml`)**
```yaml
---
new_user: deploy
ssh_public_key: "{{ lookup('file', '~/.ssh/id_rsa.pub') }}"

# Docker Configuration
docker_version: "latest"
docker_compose_version: "latest"

# Coolify Configuration
coolify_repo: "https://github.com/coollabsio/coolify.git"
coolify_dir: "/opt/coolify"

# Monitoring Configuration
node_exporter_version: "1.6.0"
prometheus_version: "2.45.0"
grafana_version: "10.2.0"

# Firewall Rules
ufw_allowed_ports:
  - 22    # SSH
  - 80    # HTTP
  - 443   # HTTPS
  - 9100  # Node Exporter
  - 9090  # Prometheus
  - 3000  # Grafana
```

---

## **📌 4. Running the Playbook**
### **🔹 Step 1: Clone the Repository**
```sh
git clone https://your-repo-url.com/ansible-project.git
cd ansible-project
```

### **🔹 Step 2: Run the Playbook**
```sh
ansible-playbook -i inventory.ini playbook.yml
```
If running as a non-root user:  
```sh
ansible-playbook -i inventory.ini playbook.yml --ask-become-pass
---

## **📌 7. Verifying the Setup**
### **🔹 Check Docker Installation**
```sh
docker --version
```
Expected output:
```
Docker version XX.XX.XX, build XXXXXXX
```

### **🔹 Test Prometheus**
Access **http://your_server_ip:9090** to check Prometheus.

### **🔹 Test Grafana**
Access **http://your_server_ip:3000**, default login:
- **User:** `admin`
- **Password:** `admin`

### **🔹 Test Coolify**
Access **http://your_server_ip:3000**.
