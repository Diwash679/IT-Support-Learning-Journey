
# Enterprise Active Directory & Infrastructure Lab

## Environment Overview
- **Hypervisor:** Microsoft Hyper-V on Windows Host
- **Domain Controller:** Windows Server 2025 (`DC01`)
- **Domain Name:** `az802lab.local`
- **Workstation Client:** Windows 11 Enterprise (`CLIENT01` / `DESKTOP-5LR3I4U`) — **Domain Joined & Active**
- **Network Configuration:** Isolated Virtual Switch (`AZ802-LAB`), Static Subnet (`192.168.10.0/24`)

---

## 1. Active Directory Architecture & User Provisioning

Configured a core Domain Controller (`DC01` at `192.168.10.10`) and deployed a structured Organizational Unit (OU) design reflecting a multi-department office environment.

### OU Structure
```text
az802lab.local
└── PerthOffice
    ├── Users
    ├── Computers
    └── Groups 

## Users Accounts Created
- john.smith (John Smith)
- sarah.jones (Sarah Jones)
- mike.tan (Mike Tan)
- priya.patel (Priya Patel)
- david.lee (David Lee)

## Help Desk Tasks Practiced
1. Password reset for john.smith
2. Account unlock for sarah.jones
3. Disable/enable account for mike.tan

## Workstation Domain Join & Troubleshooting
Deployment Steps
1. Configured static IP addressing (192.168.10.20) and set primary DNS to 192.168.10.10 (DC01).
2. Tested network layer connectivity using ping dc01.az802lab.local and nslookup az802lab.local.
3. Joined CLIENT01 to az802lab.local domain and initiated initial domain user authentication.

Real-World Help Desk Ticket: Password Change Loop
1. Problem: Logging in as AZ802LAB\john.smith triggered an endless password change prompt that rejected new entries.
2. Root Cause: Conflict between the "User must change password at next logon" flag in ADUC and the default Domain Minimum Password Age policy (1 day).
3. Resolution: Cleared the forced password change flag in ADUC → Account properties. Set Group Policy Minimum Password Age to 0 days for standard self-service password workflows.


## Screenshots
- [Screenshot 1: PerthOffice OU structure]
  <img width="1033" height="795" alt="Screenshot 2026-10-08 153327" src="https://github.com/user-attachments/assets/cd52c453-6bb2-4efc-8003-4fa014285bb6" />

- [Screenshot 2: Five users in Users OU]
- <img width="1043" height="833" alt="Screenshot 2026-10-08 154029" src="https://github.com/user-attachments/assets/d92c4af4-9450-4e01-8223-b0af1e085d77" />

- [Screenshot 3: Password reset dialog]
- <img width="1004" height="784" alt="Screenshot 2026-10-08 154415" src="https://github.com/user-attachments/assets/e107d612-2873-4d30-83b4-a3fe8d15bbc1" />

- [Screenshot 4: Account unlock]
- <img width="864" height="775" alt="image" src="https://github.com/user-attachments/assets/cb70e96d-6e7e-4a9c-bb48-77b27f71dca3" />

- [Screenshot 5: Disabled account]
-<img width="1007" height="801" alt="Screenshot 2026-10-08 154444" src="https://github.com/user-attachments/assets/228a6b7f-0c80-4076-9f3e-ffe8c0c65b8a" />
- <img width="991" height="781" alt="Screenshot 2026-10-08 154502" src="https://github.com/user-attachments/assets/4ba2db33-1078-470e-b061-7855e4772721" />

- [Screenshot 6: Account Unlock using powershell]
- <img width="909" height="769" alt="image" src="https://github.com/user-attachments/assets/21c1aaf7-fa9a-4c4d-86b3-a3e38b21b14f" />

##Enterprise Group Policy & DHCP Infrastructure
Group Policy Object (GPO) Deployment
- GPO Name: Perth-Security-Baseline
- Target Scope: Linked to PerthOffice OU.
- Policy Setting Enforced: Computer Configuration → Security Settings → Account Lockout Policy → Account Lockout Threshold = 5 invalid attempts.
- Management: Updated and forced policy application across workstations using Invoke-GPUpdate -Computer "DESKTOP-5LR3I4U" -Force.

DHCP Scope Implementation
- Server Role: Windows Server 2025 DHCP Server Role installed and authorized on DC01.
- Scope Name: Perth-Lab-Scope
- IP Address Pool: 192.168.10.100 – 192.168.10.200 (Subnet: 255.255.255.0)
- Scope Options Configured:
    - Router / Default Gateway: 192.168.10.1
    - DNS Server: 192.168.10.10 (DC01)
    - DNS Domain Name: az802lab.local
-Client Lease Verification: Reconfigured CLIENT01 interface to DHCP. Confirmed dynamic lease acquisition of 192.168.10.100 via ipconfig /renew and ipconfig /all.

- [Screenshot 7: Group policy management (Setting account lockout policy)]
- <img width="859" height="593" alt="image" src="https://github.com/user-attachments/assets/a0556941-efb9-4c85-ac5c-1b3d302c407d" />

- [Screenshot 8: Setting up DHCP Scope on the server]
- <img width="901" height="693" alt="image" src="https://github.com/user-attachments/assets/7072ea12-a4ca-47ee-bf6c-c54ebcf8badf" />

- [Screenshot 9: Client IP through DHCP]
- <img width="1021" height="870" alt="image" src="https://github.com/user-attachments/assets/3469b49d-92e0-4c0d-a154-8ea4c666da18" />



