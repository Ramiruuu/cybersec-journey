# Part 1: Basic-Network-Setup (Beginner)

- Category : Home Lab
- Status : Finish
- Time Invested : 2 Hours

## Goal

My goal is to eliminate network congestion between the IT and HR departments by separating them into two different VLANs, allowing each department to have its own dedicated network for better performance and security.

## Why I did this?

I did this project to learn and explore networking fundamentals that will help me on my journey to becoming a cybersecurity professional. Understanding how to segment networks with VLANs teaches me how to isolate traffic and control access—skills that are essential for protecting systems from attacks.

## So lets start!!! 

## Topology 

![topology](./assets/image.png)

## Current Situation

The company has two departments (IT and HR) sharing the same network 192.168.1.0/24, causing congestion and performance issues.

## Task

Configure a BEST solution as possible to this kind of problem

### Requirements

- Configure hostnames on all devices
- Set up console passwords (cisco)
- Configure enable passwords (class)
- Configure Sub-Interfaces in R1
- Configure vlans for IT and HR
- Set the ports to trunking mode and access mode
- Save the configuration to NVRAM
- Ensure all PCs can ping each other (currently all each subnets)

## Walkthrough

So first thing that I would do is to assign ip addresses to each pc in each departments

![PC-1](./assets/pc1-config.png)

![PC-2](./assets/pc2-config.png)

![PC-3](./assets/pc3-config.png)

![PC-4](./assets/pc4-config.png)    

After that, go to SW1 and SW2 and I used this following commands to set the hostname, passwords and ensuring that each pc can communicate to each other

```javascript
nable
configure terminal
hostname SW1
enable secret class

! Create VLANs
vlan 10
 name IT-Dept
vlan 20
 name HR-Dept
exit

! Configure PC ports for IT Dept (VLAN 10)
interface fastEthernet 0/1
 description Connection to PC-1
 switchport mode access
 switchport access vlan 10
 no shutdown
exit

interface fastEthernet 0/2
 description Connection to PC-2
 switchport mode access
 switchport access vlan 10
 no shutdown
exit

! Configure trunk to SW2
interface gigabitEthernet 0/1
 description Trunk to SW2
 switchport mode trunk
 no shutdown
exit

! Configure trunk to R1
interface gigabitEthernet 0/0
 description Trunk to R1
 switchport mode trunk
 no shutdown
exit

! Console password
line console 0
 password cisco
 login
exit

! Save configuration
copy running-config startup-config
```

### Verify Configurations

![alt text](./assets/showrun.png)

the " show run " command will show the current running configurations in the switch, i used this always to verify what i configured.

![copy run start](./assets/image-7.png)

the " copy run start " command is basically copy running-config startup-config, this command will save the running configuration to the startup-config or basically the NVRAM part of the switch.

REPEAT THE STEPS IN SW1 TO SW2.

## R1 Configurations

```javascript
enable
configure terminal
hostname R1
enable secret class

! Enable physical interface
interface gigabitEthernet 0/0
 description Connection to SW1
 no shutdown
exit

! Configure sub-interface for VLAN 10 (IT Dept)
interface gigabitEthernet 0/0.10
 description Gateway for IT Dept
 encapsulation dot1Q 10
 ip address 192.168.10.1 255.255.255.0
exit

! Configure sub-interface for VLAN 20 (HR Dept)
interface gigabitEthernet 0/0.20
 description Gateway for HR Dept
 encapsulation dot1Q 20
 ip address 192.168.20.1 255.255.255.0
exit

! Console password
line console 0
 password cisco
 login
exit

! Telnet/SSH access
line vty 0 4
 password cisco
 login
exit

! Save configuration
copy running-config startup-config
```

### Key takeaways

My solution addresses network congestion by implementing VLAN segmentation to separate the IT and HR departments into two distinct broadcast domains. VLAN 10 is created for the IT Department with the network 192.168.10.0/24, while VLAN 20 is established for the HR Department with the network 192.168.20.0/24. This isolation ensures that broadcast traffic from one department no longer reaches the other, significantly reducing unnecessary network congestion. The router R1 is configured with sub-interfaces to serve as the default gateway for both VLANs (192.168.10.1 for IT and 192.168.20.1 for HR), enabling inter-VLAN routing only when necessary. This approach improves network performance, enhances security through logical separation, and provides better scalability for future growth

### Ping Results

![alt text](./assets/pingresult.png)

PC - 1 Command Prompt

![alt text](./assets/pingresult2.png)

PC - 3 Command Prompt

### Conclusion

The VLAN implementation successfully segregates the IT and HR departments into their own broadcast domains, effectively resolving the network congestion issues caused by the shared 192.168.1.0/24 network. With VLAN 10 (192.168.10.0/24) dedicated to IT and VLAN 20 (192.168.20.0/24) dedicated to HR, each department now enjoys improved performance, reduced broadcast traffic, and enhanced security through logical separation. The router-on-a-stick configuration on R1 ensures proper inter-VLAN routing only when necessary, while maintaining isolation between departments. This solution provides a scalable foundation for future network growth and can be easily expanded to accommodate additional departments or devices. The network is now optimized for both current operations and future business needs.

- Document prepared by: Ramiruuu
- Date: February 19, 2026


