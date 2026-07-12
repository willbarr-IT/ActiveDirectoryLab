# Active Directory Home Lab

## Description
A virtual Active Directory home lab showcasing domain controller setup, user/group management, and custom Group Policy testing.
## Environment
* **Domain Controller:** Windows Server 2022 (`DC01`)
* **Client Workstation:** Windows 11 Enterprise (`CLIENT01`)
* **Hypervisor:** VMware Workstation Pro

## Lab Walk-through

### Phase 1: Initial Server & Network Configuration
1. Installed Windows Server 2022 on a virtual machine named `DC01`.
2. Configured a permanent static IPv4 address on the server.

### Phase 2: Active Directory Installation & Structure Creation
1. Installed Active Directory Domain Services (AD DS) and DNS Server roles via Server Manager.
2. Promoted the server to Domain Controller and established a new AD forest.
3. Built simple and scalable Organizational Unit (OU) framework with tier-1 OUs as: `Users`, `Computers`, and `Groups`, along with their respective sub-OUs.

### Phase 3: Client Deployment & Domain Join
1. Deployed a Windows 11 Enterprise virtual machine named `CLIENT01`.
2. Updated the client's IPv4 configuration to point its primary DNS directly to the static IP of `DC01`.
3. Executed an `nslookup` and `ping` from the client to verify name resolution and connection to the domain.
4. Joined `CLIENT01` to the Active Directory domain and then moved the client from the default Computer container into my custom sub-OU structure:`Computers` -> `Workstations`.
