# 🏢 Active Directory Home Lab

## 📝 Description
A virtual Active Directory home lab showcasing domain controller setup, user/group management, custom Group Policy testing, and Windows system administration. 

## 🛠️ Environment & Tools
* **Hypervisor:** VMware Workstation Pro
* **Operating Systems:** Windows Server 2022 (`DC01`), Windows 11 Enterprise (`CLIENT01`)
* **Core Technologies:**
  * Active Directory Domain Services (AD DS)
  * Group Policy Management Console (GPMC)
  * DNS Manager (Reverse Lookup Zones, PTR Records)
  * NTFS Permissions & SMB File Sharing


## 🚀 Lab Walk-through

### 🔹 Phase 1: Initial Server & Network Configuration
1. Installed Windows Server 2022 on a virtual machine named `DC01`. *(Figure 1.1)*

2. Configured a permanent static IPv4 address on the server. *(Figure 1.2)*

<br>

<details>
 <summary>📸 Click to view Phase 1 Screenshots</summary>
  <br>
  <p align="center">
   <img width="602" height="155" alt="VM_systeminfo" src="https://github.com/user-attachments/assets/c4ba08e6-bb30-4b47-ab76-cd633450de8f" />
   <br>
   <b>Figure 1.1</b>
   <br><br>
   <img width="397" height="451" alt="DNS_config_after" src="https://github.com/user-attachments/assets/977262a1-b54f-4462-a4b4-bbc3720e70cf" />
   <br>
   <b>Figure 1.2</b>
   <br><br>
</p>
</details>

<br>

### 🔹 Phase 2: Active Directory Installation & Structure Creation
1. Installed Active Directory Domain Services (AD DS) and DNS Server roles via Server Manager. *(Figure 2.1)*

2. Promoted the server to Domain Controller and established a new AD forest. *(Figure 2.2)*

3. Built a simple and scalable Organizational Unit (OU) framework with tier-1 OUs as: `Users`, `Computers`, and `Groups`, along with their respective sub-OUs. *(Figure 2.3)*

<br>
<details>
 <summary>📸 Click to view Phase 2 Screenshots</summary>
  <br>
  <p align="center">
   <img width="783" height="549" alt="AD_roles-features" src="https://github.com/user-attachments/assets/ce98308f-35f4-452b-b79b-14df405eb90c" />
   <br>
   <b>Figure 2.1</b>
   <br><br>
   <img width="757" height="554" alt="deployment_config" src="https://github.com/user-attachments/assets/89cc6db7-ff9e-408e-b06d-52004fa0088f" />
   <br>
   <b>Figure 2.2</b>
   <br><br>
   <img width="790" height="540" alt="ad_structure_groupmembership" src="https://github.com/user-attachments/assets/2632ac0b-5b7e-4364-b85e-f1ad8f265f39" />
   <br>
   <b>Figure 2.3</b>
   <br><br>
</p>
</details>

<br>

### 🔹 Phase 3: Client Deployment & Domain Join
1. Deployed a Windows 11 Enterprise virtual machine named `CLIENT01`.

2. Updated the client's IPv4 configuration to point its primary DNS directly to the static IP of `DC01`.

3. Executed an `nslookup` and `ping` from the client to verify name resolution and connection to the domain. *(Figure 3.1)*

> [!NOTE]
> **Troubleshooting: Server Unknown**
> 
> During this step, `nslookup` returned the correct IP address but listed `Server: Unknown`. While my lab could still function, I researched the issue and discovered that I had not yet configured a Reverse Lookup Zone on my DNS server.
>
> To resolve this, I logged back into `DC01`, opened **DNS Manager**, and created a new zone under **Reverse Lookup Zones** for my network. I then turned on the **"Update associated pointer (PTR) record"** setting for both `DC01` and `CLIENT01`. The change worked immediately, and upon testing again from the client, it showed the Domain Controller's full name. *(Figure 3.2)*

<br>

4. Joined `CLIENT01` to the Active Directory domain and then moved the client from the default Computer container into my custom sub-OU structure: `Computers` -> `Workstations`. *(Figure 3.3)*

<br>

<details>
 <summary>📸 Click to view Phase 3 Screenshots</summary>
  <br>
  <p align="center">
   <img width="835" height="476" alt="client_ping_nslookup" src="https://github.com/user-attachments/assets/188c8077-fbf5-4a5c-b229-f9fca036b312" />
   <br>
   <b>Figure 3.1</b>
   <br><br>
   <img width="837" height="388" alt="reverse_lookup   PTR" src="https://github.com/user-attachments/assets/d33d8165-060a-4088-9615-ed11d3ee2437" />
   <br>
   <b>Figure 3.2</b>
   <br><br>
   <img width="783" height="522" alt="moved_client01_to_workstations" src="https://github.com/user-attachments/assets/2f1097bf-fe2a-4a7c-989b-c97ab8613918" />
   <br>
   <b>Figure 3.3</b>
   <br><br>
</p>
</details>

<br>

### 🔹 Phase 4: Group Policy Implementation & Testing
1. Modified the Account Lockout parameters inside the Default Domain Policy to enforce a strict lockout policy. *(Figure 4.1)*

2. Created a `Restrict Control Panel` GPO and linked it directly to the `Users` OU to block access for everyone in the `IT`, `Sales`, and `Marketing` sub-OUs. *(Figure 4.2)*

3. Verified enforcement on the client machine by intentionally triggering an account lockout and attempting to open the Windows Control Panel as a standard domain user. *(Figure 4.3 & Figure 4.4)*

<br>

<details>
 <summary>📸 Click to view Phase 4 Screenshots</summary>
  <br>
  <p align="center">
   <img width="887" height="561" alt="account_lockout_policy" src="https://github.com/user-attachments/assets/477acb77-17ab-4f77-8d1c-af16bc753c1f" />
   <br>
   <b>Figure 4.1</b>
   <br><br>
   <img width="323" height="391" alt="group_policy_structure" src="https://github.com/user-attachments/assets/1844d3e8-2f65-4367-809b-b1e8004ffaa4" />
  <br>
  <b>Figure 4.2</b>
  <br><br>
  <img width="873" height="647" alt="lockout_test" src="https://github.com/user-attachments/assets/2ec68612-cb3a-42fa-9f41-daf15504e7ab" />
  <br>
  <b>Figure 4.3</b>
  <br><br>
  <img width="876" height="485" alt="ctrl_panel_test" src="https://github.com/user-attachments/assets/d4d2da33-d315-434c-909a-de7dfd923016" />
  <br>
  <b>Figure 4.4</b>
  <br><br>
</p>
</details>

<br>

### 🔹 Phase 5: Network File Sharing & Drive Mapping
1. Created a new folder on the `DC01` C: drive for the IT department (`\\DC01\IT`).

2. Configured the folder's NTFS settings to grant explicit Full Control to the `IT-Admins` security group while removing access for standard users. *(Figure 5.1)*

3. Utilized the GPMC (Group Policy Management Console) to map the network folder path as the `I:` drive. *(Figure 5.2)*
4. Linked the drive mapping GPO to the `IT` sub-OU.

5. **Verified Admin Access:** Logged into `CLIENT01` under an IT admin account to confirm the `IT (\\DC01) (I:)` drive successfully mapped with full write/creation permissions, verified by creating a dummy text document. *(Figure 5.3)*

6. **Verified Security Restrictions:** Logged into `CLIENT01` with a standard IT user account to confirm that attempting to open the mapped drive triggered an "Access Denied" error. *(Figure 5.4)*

<br>

<details>
 <summary>📸 Click to view Phase 5 Screenshots</summary>
  <br>
  <p align="center">
    <img width="775" height="578" alt="creation_shared_drive" src="https://github.com/user-attachments/assets/28d5b89e-d03a-4d0f-af8d-f8a4755a550c" />
   <br>
   <b>Figure 5.1</b>
   <br><br>
  <img width="397" height="453" alt="drive_map_creation" src="https://github.com/user-attachments/assets/08451d1f-c0f8-4d60-8d96-6a04b0e7678e" />
  <br>
  <b>Figure 5.2</b>
  <br><br>
  <img width="771" height="237" alt="admin_test_shared_drive" src="https://github.com/user-attachments/assets/fabac0d1-2281-4f89-9295-c6c0ae9d6fbf" />
  <br>
  <b>Figure 5.3</b>
  <br><br>
  <img width="841" height="593" alt="reg_IT_user_test" src="https://github.com/user-attachments/assets/741912a7-1fa9-4315-a91f-c02afd9a25c6" />
  <br>
  <b>Figure 5.4</b>
  <br><br>
</p>
</details>
