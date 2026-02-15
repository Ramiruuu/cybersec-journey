# Kali-Linux Setup

Category: Home Lab
Date: 15/02/2026
Status: Finish
Time Invested: 1.5 Hours

## Quick Facts

Goal: Deploy Kali Linux in a virtualized environment for safe security practice
Tools Used: VirtualBox (or VMware), Kali Linux ISO, VM Guest Additions
Skill Used: Virtual machine creation, resource allocation (RAM/CPU), ISO mounting, network configuration, snapshot management
Difficulty: Beginner

### Wht I did this?

I wanted a dedicated, isolated environment to learn cybersecurity tools and techniques without risking my main operating system. A virtual machine lets me practice freely, take snapshots before breaking things, and easily roll back if something goes wrong.

## Setup Environment

### Hardware Used

- Host: Windows 10
- CPU : Intel(R) Core(TM) i5-5200U CPU @ 2.20GHz
- RAM : 4 GB (4096 Mb)
- Storage Allocated: 20GB Dynamically allocated

### Software/tools

VirtualBox 7.2.6
Kali Linux 2025.4 (64-bit ISO)
VirtualBox Extension Pack (optional, for USB support)

### Network/IP details

Network Mode: NAT (for internet access with isolation)
Internal VM IP: 10.0.2.15 (assigned via DHCP)

### Configuration files

N/A (standard VM settings)

## Screenshots

### Allocating 4GB RAM and 2 CPU cores to the Kali VM

### First successful boot into Kali Linux desktop environment

### Verifying internet connectivity with "ping google.com" command

## What I did

### Task # 1 - Download Kali Linux ISO

Step # 1 - Go to https:///www.kali.org
Step # 2 - Click the download button
Step # 3 - Go to Virtual Machines
Step # 4 - Download the recommended Virtual Box for Kali Linux

### Task # 2 - Created Virtual Machine

Step # 5 - New VM → Type: Linux → Version: Debian (64-bit)
Step # 6 - Memory: 4096 MB
Step # 7 - Hard disk: 40GB (VDI, dynamically allocated)
Step # 8 - Storage: Mounted Kali ISO to virtual optical drive

### Output

So now the Virtual Machine is already created and booted successfuly

### Notes

You should enable EFI in the virtual machine settings for better compatibility

## Task 3 - Installed Kali & Guest Additions

Commands used

### After installation, mounted Guest Additions CD image

sudo mount /dev/cdrom /mnt
cd mnt
sudo ./VBoxLinuxAdditions.run

### Verified Installation

sudo reboot

### Check ip address

ip a

## Output/Result

- Full Kali desktop with proper screen resolution
- Shared clipboard and drag-and-drop enabled
- VM IP: 10.0.2.15 confirmed

### Note # 2

Guest Additions significantly improves usability (copy/paste between host/VM)

## Key Findings

- Finding 1: Virtualization must be enabled in BIOS/UEFI or VMs won't boot
- Finding 2: NAT mode gives internet access while keeping VM isolated from local network
- Finding 3: Snapshots are a lifesaver — took one immediately after clean install
