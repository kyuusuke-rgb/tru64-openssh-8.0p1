# OpenSSH 8.0p1 for Tru64 UNIX V5.1B (Alpha Architecture)

This repository contains pre-compiled OpenSSH 8.0p1 binaries, essential shared libraries, and configuration files tailored for Tru64 UNIX V5.1B running on DEC Alpha systems.

## Overview & Assets

- **`openssh8-openssl111-tru64-v51b-portable.tar.gz`** (11.7 MB)
  - Portable OpenSSH 8.0p1 binaries (`sshd`, `scp`, `ssh-keygen`, etc.), OpenSSL 1.1.1 libraries, and configuration templates.
- **`tru64_essential_libs.tar.gz`** (10.3 MB)
  - A refined, minimal archive of missing runtime shared libraries required by modern OpenSSH and GCC (`libmpc`, `libdb-3.1`, `libgcc_s`, `libgmp`, `libmpfr`, `libz`, etc.).

---

## Installation Guide

### 1. Extract Essential Shared Libraries
Extract the runtime libraries into `/usr/local/lib`:

# mkdir -p /usr/local/lib
# cd /usr/local/lib
# tar -zxvf /path/to/tru64_essential_libs.tar.gz

### 2. Extract OpenSSH Package & Set Permissions
Extract the OpenSSH package to the system root:

# cd /
# tar -zxvf /path/to/openssh8-openssl111-tru64-v51b-portable.tar.gz

Ensure binary paths and permissions are correctly set:

# cp -p /usr/local/bin/scp /usr/bin/scp
# chmod 755 /usr/local/sbin/sshd /usr/local/bin/scp /usr/bin/scp

### 3. Setup Privilege Separation User
Create the unprivileged `sshd` user required for OpenSSH privilege separation:

# adduser

Follow the interactive prompts:
- **Username**: `sshd`
- **Shell**: `/bin/false`
- **Home Directory**: `/var/empty`
- **Group**: `sshd` (or `system`)

Set required ownership and permissions on `/var/empty`:

# chown root:system /var/empty
# chmod 755 /var/empty

### 4. Host Key Generation & Configuration
Generate the RSA and DSA host keys:

# mkdir -p /usr/local/etc/ssh
# /usr/local/bin/ssh-keygen -t rsa -f /usr/local/etc/ssh/ssh_host_rsa_key -N ""
# /usr/local/bin/ssh-keygen -t dsa -f /usr/local/etc/ssh/ssh_host_dsa_key -N ""

Configure `/usr/local/etc/ssh/sshd_config` with the host key paths and root access settings:

# echo "HostKey /usr/local/etc/ssh/ssh_host_rsa_key" >> /usr/local/etc/ssh/sshd_config
# echo "HostKey /usr/local/etc/ssh/ssh_host_dsa_key" >> /usr/local/etc/ssh/sshd_config
# echo "PermitRootLogin yes" >> /usr/local/etc/ssh/sshd_config

---

## System Integration & Auto-Start

### 1. Create Startup Script
Create `/sbin/init.d/sshd` to manage the daemon process:

# cat << 'EOF' > /sbin/init.d/sshd
#!/bin/sh
case "$1" in
    start)
        if [ -f /usr/local/sbin/sshd ]; then
            echo "Starting OpenSSH daemon..."
            /usr/local/sbin/sshd -f /usr/local/etc/ssh/sshd_config
        fi
        ;;
    stop)
        echo "Stopping OpenSSH daemon..."
        kill `ps -ef | grep "/usr/local/sbin/sshd" | grep -v grep | awk '{print $2}'` 2>/dev/null
        ;;
    restart)
        $0 stop
        sleep 1
        $0 start
