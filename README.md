# 21. Configuration Management — Ansible ⭐⭐⭐

Ansible is an important **Configuration Management and Automation tool** used by DevOps engineers to automate the configuration, deployment, and management of servers.

Instead of manually connecting to every server and performing the same tasks, Ansible allows you to define the tasks once and execute them automatically on one or many servers.

Example:

```text
                    Ansible
                       ↓
              ┌────────┼────────┐
              ↓        ↓        ↓
             EC2      EC2      EC2
              ↓        ↓        ↓
           Install  Install  Install
           Nginx    Nginx    Nginx
              ↓        ↓        ↓
           Configure Configure Configure
           Nginx     Nginx     Nginx
              ↓        ↓        ↓
           Start    Start    Start
           Service  Service  Service
```

---

# 1. What is Configuration Management?

**Configuration Management** means automatically configuring and maintaining servers in a consistent and repeatable way.

For example, suppose a company has 10 EC2 servers.

On every server, you need to:

```text
Install Nginx
Install Git
Create users
Copy configuration files
Configure services
Start services
Enable services
Update packages
```

Doing this manually on every server is time-consuming.

Instead, Ansible can automate these operations.

```text
                    Ansible
                       ↓
        ┌──────────────┼──────────────┐
        ↓              ↓              ↓
      EC2-1          EC2-2          EC2-3
        ↓              ↓              ↓
      Nginx          Nginx          Nginx
        ↓              ↓              ↓
    Configure      Configure      Configure
        ↓              ↓              ↓
      Start          Start          Start
```

### Main purpose of Configuration Management

Configuration management helps ensure that servers are:

* Configured consistently
* Properly maintained
* Automatically managed
* Repeatable
* Less dependent on manual work

---

# 2. What is Ansible?

**Ansible is an open-source automation and configuration-management tool.**

It can be used to:

* Configure servers
* Install software
* Remove software
* Start and stop services
* Copy files
* Create users
* Modify configurations
* Deploy applications
* Automate repetitive administrative tasks

For example:

```text
Ansible
   ↓
EC2
   ↓
Install Nginx
   ↓
Configure Nginx
   ↓
Start Nginx
```

Instead of manually logging into the EC2 server and executing each command, Ansible performs the tasks automatically.

---

# 3. Why is Ansible Used?

Imagine you have:

```text
100 EC2 servers
```

You need to install Nginx on all 100 servers.

### Without Ansible

You might need to:

```text
SSH → Server 1 → Install Nginx
SSH → Server 2 → Install Nginx
SSH → Server 3 → Install Nginx
...
SSH → Server 100 → Install Nginx
```

This is slow and difficult to maintain.

### With Ansible

You can define the task once:

```text
Install Nginx
```

and execute it against all servers:

```text
                 Ansible
                    ↓
       ┌────────────┼────────────┐
       ↓            ↓            ↓
     EC2-1        EC2-2        EC2-3
       ↓            ↓            ↓
     Nginx        Nginx        Nginx
```

This is one of the main reasons configuration-management tools are useful in DevOps.

---

# 4. Ansible Architecture

Ansible commonly follows a **Control Node → Managed Nodes** architecture.

```text
                  Control Node
                   Ansible
                      |
          ┌───────────┼───────────┐
          ↓           ↓           ↓
       Managed     Managed     Managed
        Node        Node        Node
         EC2         EC2         EC2
```

### Control Node

The **Control Node** is the machine where Ansible is installed and from which Ansible commands are executed.

For example:

```text
Your Mac
   ↓
Ansible
```

or:

```text
Linux Server
   ↓
Ansible
```

### Managed Node

A **Managed Node** is a server that Ansible manages.

For example:

```text
AWS EC2
```

---

# 5. How Ansible Communicates with Linux Servers

For Linux systems, Ansible commonly uses **SSH** to connect to remote servers.

The basic architecture is:

```text
Ansible Control Node
          ↓
         SSH
          ↓
      EC2 Server
```

Ansible sends tasks to the remote server through the connection.

For example:

```text
Ansible
   ↓
SSH
   ↓
EC2
   ↓
Install Nginx
```

---

# 6. Inventory

An **Inventory** tells Ansible which servers it should manage.

It can contain:

* IP addresses
* Hostnames
* Groups of servers
* Connection information

Example inventory:

```ini
[webservers]
server1
server2
server3
```

Here:

```text
webservers
    ↓
server1
server2
server3
```

Ansible can execute tasks against all hosts in the `webservers` group.

---

# 7. Inventory with IP Addresses

You can also use IP addresses.

Example:

```ini
[webservers]
192.168.1.10
192.168.1.11
192.168.1.12
```

For AWS EC2 servers, you could use their reachable IP addresses or hostnames.

Example:

```ini
[webservers]
13.xx.xx.xx
15.xx.xx.xx
18.xx.xx.xx
```

The exact addresses will depend on your EC2 instances.

---

# 8. Inventory Groups

Inventory allows you to group servers according to their purpose.

Example:

```ini
[webservers]
server1
server2

[dbservers]
server3
server4

[appservers]
server5
server6
```

This gives:

```text
webservers
   ↓
server1
server2

dbservers
   ↓
server3
server4

appservers
   ↓
server5
server6
```

You can then run different tasks against different groups.

For example:

```text
webservers → Install Nginx

dbservers → Install Database

appservers → Deploy Application
```

---

# 9. Playbook

An **Ansible Playbook** is a YAML file that contains instructions describing what Ansible should do on managed servers.

Playbooks are usually stored with:

```text
.yml
```

or:

```text
.yaml
```

extensions.

Example:

```text
install-nginx.yml
```

A simple playbook:

```yaml
---
- name: Install Nginx
  hosts: webservers

  tasks:

    - name: Install Nginx
      ansible.builtin.package:
        name: nginx
        state: present
```

This playbook tells Ansible:

```text
Go to webservers
      ↓
Install Nginx
```

---

# 10. What is YAML?

Ansible Playbooks are normally written in **YAML**.

YAML is a human-readable data serialization format.

Example:

```yaml
name: Install Nginx
hosts: webservers
```

YAML relies heavily on indentation.

For example:

```yaml
tasks:
  - name: Install Nginx
    ansible.builtin.package:
      name: nginx
      state: present
```

Incorrect indentation can cause errors.

Therefore, YAML indentation is very important when writing Ansible Playbooks.

---

# 11. Task

A **Task** is a single unit of work in an Ansible Playbook.

For example:

```yaml
tasks:

  - name: Install Nginx
    ansible.builtin.package:
      name: nginx
      state: present
```

The task:

```text
Install Nginx
```

performs one specific operation.

A playbook can contain multiple tasks.

Example:

```yaml
tasks:

  - name: Install Nginx
    ...

  - name: Copy configuration
    ...

  - name: Start Nginx
    ...
```

So:

```text
Playbook
   ↓
Tasks
   ↓
Individual operations
```

---

# 12. Module

A **Module** is a reusable unit of code that Ansible uses to perform a particular operation.

Examples of Ansible modules are used for:

```text
Package installation
File management
Service management
User management
Copying files
Running commands
Managing cloud resources
```

For example:

```yaml
ansible.builtin.package:
```

is a module used for package management.

Another example:

```yaml
ansible.builtin.service:
```

is used to manage services.

Another example:

```yaml
ansible.builtin.copy:
```

is used to copy files.

---

# 13. Common Ansible Modules

Some useful modules for beginners are:

```text
package
service
copy
file
user
command
shell
template
```

### package

Used to install or remove software packages.

Example:

```yaml
ansible.builtin.package:
  name: nginx
  state: present
```

---

### service

Used to manage services.

Example:

```yaml
ansible.builtin.service:
  name: nginx
  state: started
```

---

### copy

Used to copy files from the Ansible control node to a managed server.

Example:

```yaml
ansible.builtin.copy:
  src: index.html
  dest: /var/www/html/index.html
```

---

### file

Used to manage files and directories.

Example:

```yaml
ansible.builtin.file:
  path: /var/www/html
  state: directory
```

---

### user

Used to create or manage users.

Example:

```yaml
ansible.builtin.user:
  name: devops
  state: present
```

---

### command

Used to execute commands on managed servers.

Example:

```yaml
ansible.builtin.command:
  cmd: uptime
```

---

### shell

Used when shell-specific features are required.

Example:

```yaml
ansible.builtin.shell:
  cmd: echo "Hello"
```

The `command` and `shell` modules should not be used unnecessarily when a dedicated Ansible module can perform the task more safely and declaratively.

---

# 14. Variables

**Variables** allow you to store values that can be reused in your Ansible Playbooks.

For example:

```yaml
vars:
  package_name: nginx
```

Then:

```yaml
ansible.builtin.package:
  name: "{{ package_name }}"
  state: present
```

Here:

```text
{{ package_name }}
```

references the variable.

The value is:

```text
nginx
```

---

# 15. Why are Variables Useful?

Variables make Playbooks more reusable and flexible.

For example:

```yaml
vars:
  package_name: nginx
```

You can later change:

```text
nginx
```

to another package without changing the task structure.

Variables can be used for:

```text
Package names
Ports
User names
File paths
Application versions
Environment-specific values
```

---

# 16. Facts

**Facts** are information that Ansible automatically gathers about managed systems.

For example, Ansible can discover information about:

```text
Operating system
IP addresses
CPU
Memory
Hostname
Architecture
Kernel
Network interfaces
```

When Ansible starts a playbook, it commonly gathers facts automatically unless fact gathering has been disabled.

Example:

```yaml
- name: Display system information
  hosts: webservers

  tasks:

    - name: Display operating system
      ansible.builtin.debug:
        var: ansible_facts.distribution
```

The actual output depends on the managed server.

---

# 17. Why are Facts Useful?

Facts allow Ansible to make decisions based on the target server.

For example:

```text
If server is Ubuntu
      ↓
Use apt

If server is Red Hat-based
      ↓
Use dnf/yum
```

Facts help Ansible understand the environment before performing tasks.

Conceptually:

```text
Managed Server
      ↓
Gather Facts
      ↓
OS / CPU / Memory / Network
      ↓
Ansible uses the information
```

---

# 18. Roles

A **Role** is a structured way to organize and reuse Ansible code.

A large Playbook can become difficult to manage if everything is placed in one file.

Roles provide a standard directory structure.

Example:

```text
roles/
└── nginx/
    ├── tasks/
    │   └── main.yml
    ├── handlers/
    │   └── main.yml
    ├── templates/
    ├── files/
    ├── vars/
    ├── defaults/
    └── meta/
```

A role can contain:

```text
Tasks
Handlers
Variables
Templates
Files
Defaults
Metadata
```

---

# 19. Why are Roles Useful?

Roles are useful for:

* Organizing large projects
* Reusing configuration
* Separating different components
* Maintaining cleaner repositories
* Working with team members

For example:

```text
roles/
├── nginx/
├── mysql/
├── docker/
└── application/
```

Each role can manage a different part of the infrastructure.

---

# 20. Idempotency

**Idempotency** is one of the most important Ansible concepts.

### Simple Definition

> Running the same Ansible task multiple times should produce the same desired final state instead of repeatedly changing the system unnecessarily.

For example:

```yaml
ansible.builtin.package:
  name: nginx
  state: present
```

The desired state is:

```text
Nginx must be installed
```

If Nginx is not installed:

```text
First run
    ↓
Install Nginx
```

If you run the Playbook again:

```text
Second run
    ↓
Nginx already installed
    ↓
No change required
```

This is idempotency.

---

# 21. Why is Idempotency Important?

Suppose you run:

```bash
ansible-playbook install-nginx.yml
```

The first time:

```text
Nginx installed
```

Run it again:

```text
Nginx already installed
No change required
```

This allows you to safely run configuration automation repeatedly.

---

# 22. Idempotency Example

Good Ansible approach:

```yaml
- name: Ensure Nginx is installed
  ansible.builtin.package:
    name: nginx
    state: present
```

The task describes the desired state:

```text
Nginx → must be present
```

This is better for configuration management than simply running raw installation commands every time.

---

# 23. SSH-Based Management

Ansible commonly uses SSH to manage Linux servers.

The basic flow is:

```text
Ansible Control Node
        ↓
       SSH
        ↓
    EC2 Server
```

The control node needs the appropriate SSH access to the managed server.

For AWS EC2, this commonly involves:

```text
EC2
 ↓
Security Group
 ↓
Allow SSH
 ↓
Port 22
```

The exact authentication method depends on your setup.

---

# 24. SSH Key Authentication

AWS EC2 commonly uses an SSH key pair for Linux instance access.

For example:

```bash
ssh -i my-key.pem ec2-user@PUBLIC_IP
```

Ansible can use the corresponding private key to establish the SSH connection.

Example inventory configuration:

```ini
[webservers]
13.xx.xx.xx ansible_user=ec2-user ansible_private_key_file=./my-key.pem
```

The username depends on the operating system/AMI.

For example, common defaults include:

```text
Amazon Linux → ec2-user
Ubuntu → ubuntu
```

The correct username should always be verified for the AMI you are using.

---

# 25. Ansible and AWS EC2

A common beginner project is:

```text
AWS EC2
   ↓
SSH
   ↓
Ansible
   ↓
Install Nginx
   ↓
Configure Nginx
   ↓
Start Nginx
```

You can use Terraform to create the infrastructure and Ansible to configure it.

This creates a very useful DevOps workflow:

```text
Terraform
   ↓
Create AWS Infrastructure
   ↓
EC2
   ↓
Ansible
   ↓
Configure EC2
   ↓
Install Application
```

---

# 26. Terraform vs Ansible

Terraform and Ansible are both DevOps tools, but they solve different problems.

### Terraform

Terraform is mainly used for **Infrastructure as Code**.

For example:

```text
Create VPC
Create Subnet
Create Security Group
Create EC2
Create Load Balancer
```

### Ansible

Ansible is mainly used for **Configuration Management and Automation**.

For example:

```text
Connect to EC2
Install Nginx
Copy configuration
Create users
Start services
Deploy application
```

---

# 27. Terraform + Ansible

They can be used together.

```text
                 Terraform
                     ↓
               AWS Infrastructure
                     ↓
                    EC2
                     ↓
                  Ansible
                     ↓
              Server Configuration
                     ↓
              Install Nginx
                     ↓
             Configure Nginx
                     ↓
               Start Nginx
```

A common separation is:

```text
Terraform → Provision infrastructure

Ansible → Configure infrastructure
```

---

# 28. Example Project Architecture

Your project can look like:

```text
                     AWS
                      │
                      ▼
                     EC2
                      │
                      │ SSH
                      ▼
                  Ansible
                      │
          ┌───────────┼───────────┐
          ↓           ↓           ↓
     Install Nginx  Configure   Start
                    Nginx       Nginx
```

---

# 29. Basic Ansible Project Structure

A beginner project can look like:

```text
ansible-nginx/
│
├── inventory
│
├── install-nginx.yml
│
└── README.md
```

Later, when you learn roles:

```text
ansible-nginx/
│
├── inventory
│
├── site.yml
│
├── roles/
│   └── nginx/
│       ├── tasks/
│       │   └── main.yml
│       ├── handlers/
│       │   └── main.yml
│       ├── templates/
│       ├── files/
│       ├── vars/
│       ├── defaults/
│       └── meta/
│
└── README.md
```

---

# 30. Basic Inventory Example

Create a file called:

```text
inventory
```

Example:

```ini
[webservers]
13.xx.xx.xx ansible_user=ec2-user ansible_private_key_file=./my-key.pem
```

Replace:

```text
13.xx.xx.xx
```

with your EC2 server's reachable IP address.

Replace:

```text
my-key.pem
```

with your actual SSH private key file.

---

# 31. Test the Ansible Connection

Before running a Playbook, you can test connectivity using:

```bash
ansible webservers -i inventory -m ansible.builtin.ping
```

If the connection is successful, Ansible should return a successful result.

Conceptually:

```text
Ansible
   ↓
Inventory
   ↓
webservers
   ↓
SSH
   ↓
EC2
   ↓
Ping
   ↓
Success
```

The Ansible `ping` module does not perform an ICMP network ping. It checks whether Ansible can connect to the host and execute its Python-based module machinery successfully.

---

# 32. Basic Nginx Playbook

Create:

```text
install-nginx.yml
```

Example:

```yaml
---
- name: Configure Nginx Web Server
  hosts: webservers
  become: true

  tasks:

    - name: Install Nginx
      ansible.builtin.package:
        name: nginx
        state: present

    - name: Start Nginx
      ansible.builtin.service:
        name: nginx
        state: started
        enabled: true
```

---

# 33. Understanding the Playbook

Let's understand each section.

### Name

```yaml
- name: Configure Nginx Web Server
```

This gives the play a readable name.

---

### Hosts

```yaml
hosts: webservers
```

This tells Ansible to run the play against the `webservers` group from the inventory.

---

### Become

```yaml
become: true
```

This allows Ansible to perform tasks with elevated privileges when required.

Installing packages and managing services commonly requires elevated privileges.

---

### Tasks

```yaml
tasks:
```

This contains the tasks that Ansible should perform.

---

### Install Nginx

```yaml
- name: Install Nginx
  ansible.builtin.package:
    name: nginx
    state: present
```

This means:

```text
Make sure Nginx is installed.
```

---

### Start Nginx

```yaml
- name: Start Nginx
  ansible.builtin.service:
    name: nginx
    state: started
    enabled: true
```

This means:

```text
Make sure Nginx is running
and enabled to start automatically.
```

---

# 34. Run the Playbook

Use:

```bash
ansible-playbook -i inventory install-nginx.yml
```

The flow is:

```text
ansible-playbook
      ↓
inventory
      ↓
webservers
      ↓
SSH
      ↓
EC2
      ↓
Install Nginx
      ↓
Start Nginx
```

---

# 35. Complete Ansible Workflow

A beginner workflow looks like:

```text
1. Install Ansible
        ↓
2. Create EC2
        ↓
3. Configure SSH access
        ↓
4. Create Inventory
        ↓
5. Test Connection
        ↓
6. Create Playbook
        ↓
7. Run Playbook
        ↓
8. Verify Nginx
```

Commands:

```bash
ansible --version
```

Then:

```bash
ansible webservers -i inventory -m ansible.builtin.ping
```

Then:

```bash
ansible-playbook -i inventory install-nginx.yml
```

---

# 36. Verify Nginx

After Ansible completes successfully, verify the service on the EC2 server.

You can connect using SSH:

```bash
ssh -i my-key.pem ec2-user@PUBLIC_IP
```

Then:

```bash
sudo systemctl status nginx
```

You can also test the web server from a browser:

```text
http://EC2-PUBLIC-IP
```

Your EC2 Security Group must allow HTTP traffic on port 80 for public access.

---

# 37. Ansible Variables Example

Example:

```yaml
---
- name: Install Web Server
  hosts: webservers
  become: true

  vars:
    web_package: nginx

  tasks:

    - name: Install web server
      ansible.builtin.package:
        name: "{{ web_package }}"
        state: present
```

Here:

```text
web_package
```

is a variable.

Its value is:

```text
nginx
```

---

# 38. Ansible Facts Example

Example:

```yaml
---
- name: Display Server Information
  hosts: webservers

  tasks:

    - name: Display operating system
      ansible.builtin.debug:
        var: ansible_facts.distribution
```

Ansible can gather information about the managed server and make that information available through facts.

---

# 39. Ansible Roles Example

A role-based project can look like:

```text
ansible-project/
│
├── inventory
├── site.yml
│
└── roles/
    └── nginx/
        ├── tasks/
        │   └── main.yml
        ├── handlers/
        │   └── main.yml
        ├── templates/
        ├── files/
        ├── vars/
        ├── defaults/
        └── meta/
```

The role can contain all configuration required for Nginx.

This is more organized than keeping everything in one large Playbook.

---

# 40. Important Ansible Concepts

## Ansible

Automation and configuration-management tool.

```text
Ansible
   ↓
Automate server management
```

---

## Inventory

Defines the servers Ansible manages.

```text
Inventory
   ↓
Servers
```

---

## Playbook

Defines what Ansible should do.

```text
Playbook
   ↓
Tasks
```

---

## Task

A single unit of work.

```text
Task
   ↓
Install Nginx
```

---

## Module

Performs a specific operation.

```text
Module
   ↓
Install package
Copy file
Manage service
Create user
```

---

## Variable

Stores reusable values.

```text
Variable
   ↓
Input
```

---

## Facts

Information automatically gathered about managed systems.

```text
Facts
   ↓
OS
CPU
Memory
IP
Hostname
etc.
```

---

## Role

Organizes reusable Ansible code.

```text
Role
   ↓
Tasks
Handlers
Templates
Files
Variables
```

---

## Idempotency

Repeated execution should maintain the desired state without unnecessary changes.

```text
Run 1 → Change required
Run 2 → No change
Run 3 → No change
```

---

## SSH

Common connection method for Linux managed nodes.

```text
Ansible
   ↓
SSH
   ↓
Linux Server
```

---

# 41. Important Ansible Commands

Check Ansible version:

```bash
ansible --version
```

Test connection:

```bash
ansible all -i inventory -m ansible.builtin.ping
```

Test a specific group:

```bash
ansible webservers -i inventory -m ansible.builtin.ping
```

Run a Playbook:

```bash
ansible-playbook -i inventory install-nginx.yml
```

List hosts:

```bash
ansible all -i inventory --list-hosts
```

---

# 42. Ansible vs Shell Scripts

You can also configure servers using shell scripts.

For example:

```bash
sudo apt update
sudo apt install nginx -y
sudo systemctl start nginx
```

But Ansible provides a structured automation framework with:

```text
Inventory
Playbooks
Modules
Variables
Facts
Roles
Idempotency
```

This makes Ansible suitable for managing larger groups of servers.

---

# 43. Ansible vs Terraform

This distinction is very important in DevOps.

### Terraform

Terraform is primarily used for **provisioning infrastructure**.

Example:

```text
Terraform
   ↓
VPC
   ↓
Subnet
   ↓
Security Group
   ↓
EC2
```

### Ansible

Ansible is primarily used for **configuring and automating servers**.

Example:

```text
Ansible
   ↓
EC2
   ↓
Install Nginx
   ↓
Configure Nginx
   ↓
Start Nginx
```

---

# 44. Terraform + Ansible Workflow

A common DevOps workflow is:

```text
                    Terraform
                        ↓
                AWS Infrastructure
                        ↓
                       VPC
                        ↓
                       EC2
                        ↓
                  SSH Connectivity
                        ↓
                     Ansible
                        ↓
                Server Configuration
                        ↓
                  Install Nginx
                        ↓
                 Configure Nginx
                        ↓
                   Start Nginx
```

In simple terms:

```text
Terraform → Create infrastructure

Ansible → Configure infrastructure
```

---

# 45. Practical Project

## Project: Configure Nginx on AWS EC2 using Ansible

### Objective

Use Ansible to:

```text
1. Connect to an EC2 server
2. Install Nginx
3. Configure Nginx
4. Start Nginx
5. Enable Nginx
6. Verify the web server
```

Architecture:

```text
                   Ansible
                      ↓
                     SSH
                      ↓
                    EC2
                      ↓
                Install Nginx
                      ↓
               Configure Nginx
                      ↓
                Start Nginx
                      ↓
                 Web Server
```

---

# 46. Project Requirements

You need:

```text
Ansible Control Node
AWS Account
EC2 Linux Instance
SSH Key
Security Group
```

The EC2 Security Group should allow SSH access from your trusted source.

For a public web server, allow:

```text
HTTP
Port 80
```

SSH:

```text
SSH
Port 22
```

For security, SSH should preferably be restricted to your trusted IP rather than opening port 22 to the entire internet.

---

# 47. Project Directory

Create:

```text
ansible-nginx/
```

Inside:

```text
ansible-nginx/
│
├── inventory
├── install-nginx.yml
└── README.md
```

---

# 48. Inventory

Example:

```ini
[webservers]
EC2_PUBLIC_IP ansible_user=ec2-user ansible_private_key_file=./my-key.pem
```

Replace:

```text
EC2_PUBLIC_IP
```

with your EC2 public IP.

Replace:

```text
my-key.pem
```

with your private key filename.

The SSH username depends on the operating system.

For example:

```text
Amazon Linux → ec2-user
Ubuntu → ubuntu
```

---

# 49. Test Connection

Run:

```bash
ansible webservers -i inventory -m ansible.builtin.ping
```

If the connection is working, Ansible should report a successful result.

If the connection fails, check:

```text
EC2 is running
Correct IP address
Correct SSH username
Correct private key
Security Group allows SSH
Network connectivity
Private key permissions
```

---

# 50. Create the Playbook

Create:

```text
install-nginx.yml
```

Use:

```yaml
---
- name: Configure Nginx Web Server
  hosts: webservers
  become: true

  tasks:

    - name: Install Nginx
      ansible.builtin.package:
        name: nginx
        state: present

    - name: Start and enable Nginx
      ansible.builtin.service:
        name: nginx
        state: started
        enabled: true
```

---

# 51. Run the Playbook

Run:

```bash
ansible-playbook -i inventory install-nginx.yml
```

Ansible will:

```text
Read Inventory
      ↓
Find webservers
      ↓
Connect using SSH
      ↓
Install Nginx
      ↓
Start Nginx
      ↓
Enable Nginx
```

---

# 52. Verify the Result

Connect to EC2:

```bash
ssh -i my-key.pem ec2-user@EC2_PUBLIC_IP
```

Check Nginx:

```bash
sudo systemctl status nginx
```

Then open:

```text
http://EC2_PUBLIC_IP
```

If HTTP access is correctly configured, you should see the Nginx web page.

---

# 53. Important Security Notes

When working with Ansible and AWS:

### Do not expose SSH unnecessarily

Avoid:

```text
0.0.0.0/0 → Port 22
```

when you can restrict SSH to your trusted IP.

Prefer:

```text
Your IP → Port 22
```

### Protect private keys

Never commit:

```text
*.pem
```

files to GitHub.

Add them to `.gitignore`.

Example:

```gitignore
*.pem
```

### Do not commit secrets

Avoid storing:

```text
Passwords
Private keys
API tokens
Cloud credentials
Secrets
```

directly in Git repositories.

---

# 54. Recommended Ansible Learning Order

Follow this order as a beginner:

```text
1. What is Configuration Management?
        ↓
2. What is Ansible?
        ↓
3. Control Node
        ↓
4. Managed Node
        ↓
5. SSH
        ↓
6. Inventory
        ↓
7. Playbook
        ↓
8. Task
        ↓
9. Module
        ↓
10. Variables
        ↓
11. Facts
        ↓
12. Idempotency
        ↓
13. Roles
        ↓
14. Ansible Commands
        ↓
15. AWS EC2 Project
```

---

# 55. Key Ansible Architecture

Remember:

```text
                     CONTROL NODE
                         |
                      Ansible
                         |
              ┌──────────┼──────────┐
              ↓          ↓          ↓
             SSH        SSH        SSH
              ↓          ↓          ↓
            EC2-1      EC2-2      EC2-3
              ↓          ↓          ↓
           Nginx       Nginx       Nginx
```

---

# 56. Complete Ansible Workflow

```text
Install Ansible
       ↓
Create EC2
       ↓
Configure SSH
       ↓
Create Inventory
       ↓
Test Connection
       ↓
Create Playbook
       ↓
Define Tasks
       ↓
Run Playbook
       ↓
Ansible connects through SSH
       ↓
Install Nginx
       ↓
Configure Nginx
       ↓
Start Nginx
       ↓
Verify Web Server
```

---

# 57. One-Line Revision Notes

```text
Ansible
→ Automation and configuration-management tool

Inventory
→ List/group of managed servers

Playbook
→ YAML file containing automation instructions

Task
→ Individual unit of work

Module
→ Performs a specific operation

Variable
→ Stores reusable values

Facts
→ Information gathered about managed systems

Role
→ Organized and reusable Ansible code

Idempotency
→ Repeated execution maintains the desired state without unnecessary changes

SSH
→ Common connection method for Linux managed nodes

Control Node
→ Machine where Ansible is installed and executed

Managed Node
→ Server managed by Ansible
```

---

# 58. Important Commands Cheat Sheet

Check Ansible version:

```bash
ansible --version
```

Test all hosts:

```bash
ansible all -i inventory -m ansible.builtin.ping
```

Test a specific group:

```bash
ansible webservers -i inventory -m ansible.builtin.ping
```

List hosts:

```bash
ansible all -i inventory --list-hosts
```

Run a Playbook:

```bash
ansible-playbook -i inventory install-nginx.yml
```

---

# 59. Ansible vs Terraform — Quick Revision

| Terraform                        | Ansible                                                            |
| -------------------------------- | ------------------------------------------------------------------ |
| Infrastructure as Code           | Configuration Management & Automation                              |
| Mainly provisions infrastructure | Mainly configures existing systems                                 |
| Creates VPC                      | Configures EC2                                                     |
| Creates Subnet                   | Installs software                                                  |
| Creates Security Group           | Configures services                                                |
| Creates EC2                      | Deploys applications                                               |
| Uses Terraform configuration     | Uses YAML Playbooks                                                |
| Maintains Terraform state        | Primarily uses desired-state modules without Terraform-style state |

A common workflow is:

```text
Terraform
   ↓
Create AWS Infrastructure
   ↓
EC2
   ↓
Ansible
   ↓
Configure EC2
   ↓
Install Application
```

---

# 60. Final Goal

After learning this topic, you should be able to understand and explain:

```text
Configuration Management
Ansible
Control Node
Managed Node
Inventory
Playbook
Task
Module
Variables
Facts
Roles
Idempotency
SSH-based Management
```

You should also be able to perform a practical task like:

```text
                 Ansible
                    ↓
                   SSH
                    ↓
                   EC2
                    ↓
              Install Nginx
                    ↓
             Configure Nginx
                    ↓
               Start Nginx
                    ↓
              Nginx Web Server
```

---

# 61. Final Takeaway

> **Ansible is an automation and configuration-management tool that allows DevOps engineers to manage multiple servers consistently and automatically.**

The most important concepts to remember are:

```text
Inventory
   ↓
Playbook
   ↓
Tasks
   ↓
Modules
   ↓
Variables
   ↓
Facts
   ↓
Roles
   ↓
Idempotency
   ↓
SSH
```

The most important practical workflow is:

```text
Ansible Control Node
        ↓
      SSH
        ↓
     EC2 Server
        ↓
   Install Nginx
        ↓
  Configure Nginx
        ↓
   Start Nginx
        ↓
   Web Server
```

And when combined with Terraform:

```text
Terraform
    ↓
Provision AWS Infrastructure
    ↓
EC2
    ↓
Ansible
    ↓
Configure EC2
    ↓
Install & Configure Applications
```

This combination gives you a strong foundation for **DevOps infrastructure provisioning and configuration management**.

