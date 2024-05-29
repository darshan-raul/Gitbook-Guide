# Index

**I. Linux Fundamentals**

* **Basic Commands:**
  * `ls`, `cd`, `pwd`, `mkdir`, `rm`, `cp`, `mv`, `cat`, `less`, `more`, `head`, `tail`
  * Redirection (`>`, `>>`, `<`, `|`) and Pipes
  * Wildcards (`*`, `?`, `[]`)
  * Command History (`history`, `!`, `!!`)
  * Tab Completion
* **File System:**
  * Understanding file system hierarchy (e.g., `/`, `/etc`, `/home`, `/var`)
  * Inodes and Links (hard vs. soft)
  * File Permissions (owner, group, others) and Octal Notation (chmod)
  * Common File Types (regular, directory, block, character, symbolic link)
  * Disk Usage (`df`, `du`)
* **Processes:**
  * Process Management (`ps`, `top`, `htop`, `kill`, `killall`)
  * Process Signals (`SIGKILL`, `SIGTERM`, etc.)
  * Daemons and Background Processes
  * `pstree` and Process Relationships
* **Users & Groups:**
  * User Management (`useradd`, `userdel`, `usermod`, `passwd`)
  * Group Management (`groupadd`, `groupdel`, `groupmod`)
  * `/etc/passwd`, `/etc/shadow`, `/etc/group` files
  * Sudo and Privileged Access Management
* **Package Management:**
  * Using package managers (e.g., `apt`, `yum`, `dnf`, `pacman`)
  * Updating, Installing, Removing Packages
  * Package Repositories
  * Managing Dependencies
* **Text Editors (vim/nano):**
  * Basic navigation, editing, saving, and searching commands
  * Configuration customization

**II. System Configuration & Administration**

* **Networking:**
  * `ifconfig`, `ip` commands
  * Network Interfaces and IP Addresses
  * Routing Tables (`route`)
  * Name Resolution (`/etc/hosts`, DNS)
  * Network Services (`netstat`, `ss`)
  * Firewalls (e.g., `iptables`, `firewalld`)
* **Systemd (or other init systems):**
  * Service management (`systemctl`)
  * Unit files (service, socket, timer, etc.)
  * Target management and runlevels
  * Troubleshooting Systemd issues
* **Log Management:**
  * Common log files (`/var/log/`)
  * Log rotation (`logrotate`)
  * Syslog and Journald (`journalctl`)
  * Centralized logging (e.g., rsyslog, ELK stack)
* **Disk Management:**
  * Partitioning Tools (e.g., `fdisk`, `parted`, `gparted`)
  * Filesystems (ext4, xfs, etc.)
  * Logical Volume Management (LVM)
  * RAID (hardware and software)
  * Monitoring Disk Usage and Health
* **System Monitoring:**
  * `top`, `htop`, `vmstat`, `iostat`, `sar`
  * SNMP and Monitoring Tools (e.g., Nagios, Zabbix)
  * Performance Tuning and Optimization

**III. Advanced Topics**

* **Shell Scripting (Bash):**
  * Variables: Scoping, local/global, environment variables, arrays, special parameters
  * Advanced Operators: Arithmetic, logical, string, bitwise
  * Control Flow: `if-elif-else`, `case`, `for`, `while`, `until`, `select`, `break`, `continue`
  * Functions: Defining, calling, passing arguments, return values
  * Regular Expressions: Basic syntax and usage with `grep`, `sed`, `awk`
  * Input/Output: Redirection, pipes, command substitution, here documents
  * File Operations: Reading, writing, appending, deleting, renaming, permissions
  * Error Handling: `trap`, exit codes, stderr
  * Debugging: `set -x`, `echo`, `printf`
  * Advanced Features: Subshells, process substitution, co-processes
  * Best Practices: Readability, maintainability, commenting
  * Practical Examples: Log parsing, file manipulation, system administration tasks
* **Security:**
  * Hardening:
    * SSH Configuration: Disabling password authentication, key-based auth, port change, fail2ban
    * Firewall Configuration (iptables/firewalld): Restricting inbound/outbound traffic, port knocking
    * User Management: Strong passwords, password aging, account lockout policies, least privilege principle
    * System Updates: Patch management, vulnerability scanning
  * Security Auditing:
    * `auditd` configuration and log analysis
    * File Integrity Monitoring (e.g., Tripwire, AIDE)
    * System activity monitoring (e.g., `last`, `lastb`)
  * Intrusion Detection/Prevention Systems (IDS/IPS):
    * Snort, OSSEC, Fail2ban
  * Security Frameworks:
    * SELinux (enforcing, permissive modes, policy management)
    * AppArmor (profile management)
* **Virtualization:**
  * KVM:
    * Installation and configuration
    * Guest OS management (virt-manager, virsh)
    * Networking and Storage Options
  * VirtualBox:
    * Guest OS setup and configuration
    * Shared folders and networking
  * Docker:
    * Images, containers, volumes
    * Dockerfile and Docker Compose
    * Orchestration with Kubernetes or Docker Swarm
* **Cloud Technologies (AWS, Azure, GCP):**
  * Core Concepts: IaaS, PaaS, SaaS
  * Virtual Machines (EC2, VMs)
  * Storage Options (S3, EBS, Blob Storage)
  * Networking (VPC, Subnets, Security Groups)
  * Managed Services (RDS, DynamoDB, Azure SQL, etc.)
  * Infrastructure as Code (CloudFormation, Terraform)
* **Performance Tuning:**
  * Profiling Tools: `perf`, `top`, `vmstat`, `iostat`, `strace`
  * Bottleneck Analysis: CPU, Memory, Disk I/O, Network
  * Kernel Tuning: Sysctl parameters
  * Database Tuning: Query optimization, indexing, caching
* **Troubleshooting:**
  * Systematic Approach: Gather information, isolate the issue, formulate hypotheses, test, document
  * Debugging Tools: `strace`, `ltrace`, `gdb`
  * Common Issues: Service failures, network connectivity, disk problems, performance bottlenecks
