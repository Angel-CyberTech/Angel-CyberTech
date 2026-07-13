# Windows Server 2019 Active Directory & Network Services Home Lab

This repository documents the step-by-step deployment and configuration of a virtualized Windows Server 2019 Active Directory environment. The purpose of this lab is to build a secure, isolated private network that includes a fully functional Domain Controller (DC) capable of providing routing (NAT) and dynamic IP addressing (DHCP) to client machines.

## Lab Goals

- Create a working Windows Server 2019 Domain Controller in VirtualBox
- Establish a private internal lab network using VirtualBox internal networking
- Configure Active Directory Domain Services (AD DS)
- Implement routing, NAT, and DHCP services for client connectivity
- Document the process for future expansion, including Windows client deployment

## Environment Architecture

- Hypervisor: Oracle VM VirtualBox
- Virtual Machine Name: DC
- Operating System: Windows Server 2019
- Storage Allocation: 50 GB
- Memory Allocation: 4 GB RAM
- Network Layout: Internal Network (intnet)

## Deployment Checklist

### Phase 1: Virtual Machine Provisioning & Troubleshooting

- [x] Sourced the Windows Server 2019 ISO and staged deployment files
- [x] Created the DC virtual machine in VirtualBox
- [x] Attached the Server 2019 ISO to the secondary optical drive
- [x] Configured the primary network adapter to use Internal Network
- [x] Resolved initial BIOS/boot issues by enabling EFI support
- [x] Completed the base Windows Server 2019 installation
- [x] Initialized the local administrator credentials

### Phase 2: Post-Installation & Network Baseline

- [x] Assigned a static IP address and preferred DNS loopback to the internal interface
- [x] Renamed the system to DC and rebooted successfully

### Phase 3: Active Directory Domain Services Deployment

- [x] Installed the AD DS role via Server Manager
- [x] Promoted the server to a Domain Controller
- [x] Created a new forest with the root domain name MyDomain.com
- [x] Configured the Directory Services Restore Mode (DSRM) password
- [x] Verified the domain sign-in environment and login format

### Phase 4: Identity & Access Management Setup

- [x] Created an Organizational Unit (OU) named Admins in Active Directory Users and Computers
- [x] Created a dedicated user account inside the Admins OU
- [x] Added the account to the Domain Admins security group
- [x] Successfully signed in with the new dedicated domain admin account

### Phase 5: Routing, NAT, and DHCP Configuration

- [x] Installed the Remote Access role with Routing services
- [x] Configured Routing and Remote Access (RRAS) for NAT support
- [x] Installed and authorized the DHCP Server role
- [x] Configured an IPv4 DHCP scope with a default gateway and DNS server pointing to the DC

## Detailed Configuration Notes

### 1. Initial VM Setup & UEFI Troubleshooting

During the initial boot attempt, the VM failed to load the installation media and displayed the error:

> Option ROM requires DDIM support

Resolution:
1. Powered off the VM and opened VirtualBox Settings.
2. Navigated to System > Motherboard.
3. Enabled EFI support for the virtual machine.
4. Adjusted the boot order so the optical drive was prioritized ahead of the virtual hard disk.
5. Rebooted and completed the Windows Server installation successfully.

### 2. Network Interface Baseline & System Naming

Before promoting the server to a Domain Controller, the internal NIC was configured manually to avoid IP conflicts.

- IP Assignment: Static
- Preferred DNS: Local loopback / dedicated DC IP
- Computer Name: DC

### 3. Active Directory Installation & Domain Promotion

The Active Directory Domain Services role was added through Server Manager, and the server was promoted to a Domain Controller with the following settings:

- Deployment Operation: Add a new forest
- Root Domain Name: MyDomain.com
- DSRM Password: Configured and stored securely

### 4. Administrative Account Delegation

To follow least-privilege principles, the built-in administrator account was not used for day-to-day operations.

1. Open Active Directory Users and Computers (dsa.msc).
2. Create an OU named Admins.
3. Create a new user account inside that OU.
4. Add the account to the Domain Admins group from the Member Of tab.

### 5. Routing, NAT, and DHCP Setup

The Domain Controller was configured to act as a gateway for future client machines in the lab environment.

```text
[Internal Client VM] ---> [DC (NAT + DHCP Server)] ---> [External Internet]
```

- NAT: Enabled through Routing and Remote Access on the external interface
- DHCP: Configured with an IPv4 scope to automatically assign IP addresses to clients
- Default Gateway and DNS Server: Pointed to the DC's static IP address

## Next Steps

A Windows 10 client machine can be added next to validate end-to-end connectivity, domain join operations, and user authentication. Once that client is deployed, the next step will be to document the client-side configuration and domain join process.
