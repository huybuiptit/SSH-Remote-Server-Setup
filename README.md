# Project: SSH Remote Server Setup
https://roadmap.sh/projects/ssh-remote-server-setup

## 1. Project Overview
The goal of this project is to set up a remote Linux server and configure secure, passwordless SSH access using multiple SSH key pairs. This setup ensures high security and practices standard DevOps workflows for server management.

## 2. Infrastructure Environment
- **Server OS:** Ubuntu Linux (Running on VMware)
- **Network Interface:** eth0 with IP `160.xxx.xxx.xxx`
- **SSH Service:** OpenSSH Server

## 3. Implementation Steps

### Step 1: Server-Side Installation
First, I updated the package list and installed the OpenSSH server on the remote machine:
```bash
sudo apt update
sudo apt install openssh-server -y

I also ensured the service starts automatically upon boot:
Bash

sudo systemctl enable --now ssh

### Step 2: SSH Key Generation

On my local machine, I generated two separate SSH key pairs using the modern Ed25519 algorithm:
Bash

ssh-keygen -t ed25519 -f ~/.ssh/key_du_an_1
ssh-keygen -t ed25519 -f ~/.ssh/key_du_an_2

### Step 3: Authorizing Keys on Server

I transferred the public keys to the remote server. This process required a one-time password authentication to append the keys to the ~/.ssh/authorized_keys file:
Bash

ssh-copy-id -i ~/.ssh/key_du_an_1.pub backup@160.xxx.xxx.xxx
ssh-copy-id -i ~/.ssh/key_du_an_2.pub backup@160.xxx.xxx.xxx

### Step 4: Simplifying Connections

To meet the requirement of connecting via a simple command, I configured the local ~/.ssh/config file:
Plaintext

Host server-key1
    HostName 160.xxx.xxx.xxx
    User backup
    IdentityFile ~/.ssh/key_du_an_1

Host server-key2
    HostName 160.xxx.xxx.xxx
    User backup
    IdentityFile ~/.ssh/key_du_an_2

### Step 5: Security Hardening (Verified)

After confirming both keys worked, I verified the server's security status. The server is now configured to prefer Public Key Authentication over traditional passwords.
### Step 4: Verification

The setup was successfully verified by executing the following shorthand commands without any password prompts:

    ssh server-key1

    ssh server-key2
