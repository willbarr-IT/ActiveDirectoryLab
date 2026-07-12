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
<img width="60%" alt="VM_systeminfo" src="https://github.com/user-attachments/assets/30a86a64-7c8d-4a0f-b4e3-6092b8c95b68" />

2. Configured a permanent static IPv4 address on the server.
<img width="350" alt="DNS_config_after" src="https://github.com/user-attachments/assets/2a52c1bc-95c6-4860-a3a4-1b43879ab965" />

### Phase 2: Active Directory Installation & Structure Creation
1. Installed Active Directory Domain Services (AD DS) and DNS Server roles via Server Manager.
2. Promoted the server to Domain Controller and established a new AD forest.
<img width="60%" alt="AD_roles-features" src="https://github.com/user-attachments/assets/1f041eb1-9f75-49e2-be19-d74285333e2f" />
<img width="60%" alt="deployment_config" src="https://github.com/user-attachments/assets/97210827-e000-4d7d-8cb1-792ba36cfa65" />

3. Built simple and scalable Organizational Unit (OU) framework with tier-1 OUs as: `Users`, `Computers`, and `Groups`, along with their respective sub-OUs.
<img width="60%" alt="ad_structure_groupmembership" src="https://github.com/user-attachments/assets/29651fb0-82ae-4ad6-ac23-2ab932a9c9a6" />

### Phase 3: Client Deployment & Domain Join
1. Deployed a Windows 11 Enterprise virtual machine named `CLIENT01`.
2. Updated the client's IPv4 configuration to point its primary DNS directly to the static IP of `DC01`.
<img width="70%" alt="creation_client01" src="https://github.com/user-attachments/assets/721ae617-0179-4be8-815c-bb1204a995ad" />
<img width="350" alt="CLIENT01_dnsconfig" src="https://github.com/user-attachments/assets/0b0f64db-9601-4ff7-bbaa-a5306f46b927" />

3. Executed network ping to the domain from the client to ensure successful communication
