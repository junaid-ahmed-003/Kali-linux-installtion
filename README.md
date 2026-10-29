# Kali Linux Setup Guide for Cybersecurity Beginners

A step-by-step guide to installing, configuring, and verifying Kali Linux in a virtual machine for cybersecurity learning, ethical hacking, and authorized security testing.

## Table of Contents

- [Overview](#overview)
- [Prerequisites](#prerequisites)
- [Step 1: Download VirtualBox](#step-1-download-virtualbox)
- [Step 2: Download Kali Linux](#step-2-download-kali-linux)
- [Step 3: Create the Virtual Machine](#step-3-create-the-virtual-machine)
- [Step 4: Start Kali Linux](#step-4-start-kali-linux)
- [Step 5: Update the System](#step-5-update-the-system)
- [Step 6: Install Essential Tools](#step-6-install-essential-tools)
- [Step 7: Configure Networking](#step-7-configure-networking)
- [Step 8: Verify the Installation](#step-8-verify-the-installation)
- [Troubleshooting](#troubleshooting)
- [Security Best Practices](#security-best-practices)
- [Learning Resources](#learning-resources)

## Overview

Kali Linux is a Debian-based Linux distribution designed for penetration testing, security auditing, digital forensics, and cybersecurity education.

This guide uses Oracle VirtualBox to run Kali Linux as a virtual machine. Virtualization allows Kali to run separately from the host operating system and makes it easier to create snapshots and revert changes.

**Target environment:** Kali Linux running in VirtualBox on a Windows host.

## Prerequisites

Recommended host specifications:

| Component      | Recommendation                                      |
| -------------- | --------------------------------------------------- |
| Host OS        | Windows 10/11 or a supported host OS                |
| RAM            | 8 GB minimum; 16 GB recommended                     |
| CPU            | Modern 64-bit processor with virtualization support |
| Disk space     | At least 40 GB free                                 |
| Virtualization | Intel VT-x or AMD-V enabled                         |
| Internet       | Required for downloads and updates                  |

Suggested virtual machine configuration:

| Setting          | Value                           |
| ---------------- | ------------------------------- |
| RAM              | 4 GB                            |
| Virtual CPUs     | 2                               |
| Virtual disk     | 40–60 GB, dynamically allocated |
| Graphics memory  | 128 MB if supported             |
| Network adapter  | NAT for initial setup           |
| Operating system | Kali Linux 64-bit               |

Adjust the VM resources according to your computer's available memory and CPU capacity.

## Step 1: Download VirtualBox

1. Visit [https://www.virtualbox.org/wiki/Downloads](https://www.virtualbox.org/wiki/Downloads)
2. Download the installer for your host operating system.
3. Run the installer and follow the setup wizard.
4. Open VirtualBox after installation.

If virtualization is disabled, check your computer's BIOS/UEFI settings for Intel Virtualization Technology or AMD SVM.

## Step 2: Download Kali Linux

Official download page:

[https://www.kali.org/get-kali/](https://www.kali.org/get-kali/)

Two common options are available:

- **Pre-built VirtualBox image:** Recommended for beginners because the VM is already configured.
- **Installer ISO:** Suitable for creating a VM and performing a manual installation.

For the pre-built image, download the VirtualBox-compatible archive and extract it using a suitable archive utility. Open the extracted folder and import the provided virtual machine file using VirtualBox's import functionality.

For the ISO route, download the official 64-bit installer image and create a new virtual machine.

Always download Kali Linux from its official website. Verify the downloaded file's checksum or signature using the official verification instructions:

[https://www.kali.org/docs/introduction/download-official-kali-linux-images/](https://www.kali.org/docs/introduction/download-official-kali-linux-images/)

## Step 3: Create the Virtual Machine

If installing from an ISO:

1. Open VirtualBox and select **New**.
2. Enter the VM name `Kali Linux`.
3. Select the appropriate Linux/Debian 64-bit operating system type.
4. Allocate 4 GB RAM if the host has sufficient memory.
5. Assign two virtual CPUs.
6. Create a virtual disk of 40–60 GB.
7. Attach the Kali installer ISO to the virtual optical drive.
8. Set the network adapter to NAT for initial internet access.
9. Start the VM and choose the graphical installer.
10. Follow the installer prompts to configure language, keyboard, timezone, hostname, user account, disk partitioning, and bootloader.
11. Restart the VM and detach the installer ISO when prompted.

**Important:** Select the correct virtual disk during installation. Do not select or format your host computer's disk.

For a pre-built VirtualBox image, import the supplied VM instead of repeating the ISO installation process. Follow the credentials and first-boot instructions provided with the official image.

## Step 4: Start Kali Linux

Log in using the credentials configured during installation or those documented for the official pre-built image.

Open the terminal from the desktop environment.

Check the operating system:

```bash
cat /etc/os-release
```

Check the kernel:

```bash
uname -r
```

Check disk usage:

```bash
df -h
```

Check available memory:

```bash
free -h
```

## Step 5: Update the System

Refresh the package repository metadata:

```bash
sudo apt update
```

Upgrade installed packages:

```bash
sudo apt full-upgrade
```

Restart if a kernel or other system update requires it:

```bash
sudo reboot
```

After restarting, confirm that the desktop and terminal work correctly.

Keep the system updated and review package changes before confirming installations or upgrades.

## Step 6: Install Essential Tools

Install a small set of commonly used tools:

```bash
sudo apt install -y \
  curl \
  wget \
  git \
  vim \
  nano \
  htop \
  net-tools \
  dnsutils \
  iproute2 \
  nmap \
  tcpdump \
  wireshark
```

Tool purposes:

| Tool          | Purpose                                        |
| ------------- | ---------------------------------------------- |
| Git           | Version control and source management          |
| Curl and Wget | HTTP requests and file downloads               |
| Nmap          | Network discovery and authorized port scanning |
| Tcpdump       | Packet capture and network troubleshooting     |
| Wireshark     | Graphical packet analysis                      |
| DNS utilities | DNS lookup and troubleshooting                 |
| Htop          | Process and resource monitoring                |
| Vim and Nano  | Terminal text editors                          |

If Wireshark asks whether non-root users should be allowed to capture packets, make an informed choice. Packet capture permissions should be restricted to authorized users.

Verify the tools:

```bash
git --version
nmap --version
tcpdump --version
wireshark --version
```

## Step 7: Configure Networking

For initial setup, NAT is a practical default because it permits outbound connectivity without directly exposing the VM as a host on the physical network.

Check network interfaces:

```bash
ip -br addr
```

Check the routing table:

```bash
ip route
```

Check DNS resolution:

```bash
getent hosts kali.org
```

Check internet connectivity:

```bash
curl -I https://www.kali.org/
```

A successful response indicates that the HTTP connection reached the website. If connectivity fails, check the VM network adapter, DNS configuration, and host internet connection.

### Network isolation for security testing

For a dedicated lab:

- Use an internal or host-only virtual network for isolated practice.
- Keep vulnerable practice machines separate from your personal and workplace networks.
- Do not bridge a vulnerable test machine onto a production network.
- Use NAT only when outbound access is needed and consistent with the lab's security requirements.
- Scan only systems you own or have explicit permission to test.

VirtualBox networking documentation:

[https://www.virtualbox.org/manual/ch06.html](https://www.virtualbox.org/manual/ch06.html)

## Step 8: Verify the Installation

Run the following checks:

```bash
cat /etc/os-release
uname -r
free -h
df -h
ip -br addr
git --version
nmap --version
```

Check that:

- Kali Linux boots and the desktop loads.
- The terminal opens and commands execute.
- Package updates complete without unresolved errors.
- The network is configured as intended.
- Essential tools are installed.
- Available disk space and memory are sufficient.
- The VM can shut down and restart normally.

For a harmless initial Nmap test, scan the local loopback interface:

```bash
nmap -sT -F 127.0.0.1
```

This tests the local machine only. An empty result can be normal when no TCP services are listening.

## Troubleshooting

### Virtual machine does not start

- Confirm hardware virtualization is enabled.
- Check whether another hypervisor configuration is interfering.
- Reduce VM memory allocation if the host is low on RAM.
- Review the VirtualBox error message before changing system settings.

### Kali has no internet connection

- Confirm that the virtual network adapter is enabled.
- Check the adapter's attachment mode.
- Inspect the routing table and DNS resolution.
- Restart the VM after correcting the configuration.

### Package updates fail

```bash
sudo apt update
```

Review the error output, system clock, DNS configuration, repository settings, and internet connectivity. Avoid adding unknown repositories or disabling package verification to bypass errors.

### Disk space is low

```bash
df -h
```

Review which filesystems are full before deleting files or expanding the virtual disk.

## Security Best Practices

- Use unique, strong passwords.
- Keep Kali and installed packages updated.
- Create a VirtualBox snapshot after completing the initial configuration.
- Do not store credentials, private keys, tokens, or sensitive logs in GitHub.
- Never commit confidential internship documents or company data.
- Keep lab targets isolated and use written authorization for testing.
- Review files and Git history before making a repository public.

## Learning Resources

- Kali Linux: [https://www.kali.org/docs/](https://www.kali.org/docs/)
- Kali Linux Downloads: [https://www.kali.org/get-kali/](https://www.kali.org/get-kali/)
- VirtualBox Manual: [https://www.virtualbox.org/manual/](https://www.virtualbox.org/manual/)
- Nmap Documentation: [https://nmap.org/book/man.html](https://nmap.org/book/man.html)
- Wireshark Documentation: [https://www.wireshark.org/docs/](https://www.wireshark.org/docs/)

## Project Status

- [x] Kali Linux environment configured
- [x] Initial installation procedure documented
- [x] Installation and verification commands tested
- [x] Screenshots added
- [x] Troubleshooting notes reviewed
- [x] Repository reviewed for sensitive information

This project is intended for cybersecurity education and authorized security testing only.
