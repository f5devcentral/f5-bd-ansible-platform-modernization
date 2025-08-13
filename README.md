# F5 Platform Modernization Ansible Automation

This Ansible playbook automates the modernization of F5 platforms, enabling seamless migration from older F5 BIG-IP models to newer ones. It facilitates the backup, migration, upgrade, and restoration of critical configurations, including UCS (User Configuration Set) files, crypto files, and other essential components. Additionally, this playbook supports version upgrades, such as migrating from lower versions (e.g., 15.1.x) to more recent releases (e.g., 17.5).

---

## ⚠️ PLEASE READ FIRST!

- This code is a **WORK IN PROGRESS**.  
- Tested on an **i10800 platform** as the source and a **Velos CX410 with BX110 Blade System** and **rSeries R5800** as the destination.  
- Firmware on the Velos Chassis Controller and partition uses **F5OS 1.8.1 GA** due to management IP constraints.  
- The **Standalone code** has been tested for both **rSeries** and **iSeries** deployments.  

---

## 🛠 Prerequisites

Before using this playbook, ensure you have:

- Ansible Automation Platform configured in your environment.  
- Access to both the source and destination F5 BIG-IP devices.  
- A **backup host** for storing UCS and crypto files.  
- SSH access to the bastion host and F5 devices.  
- The necessary F5 BIG-IP management modules installed on your Ansible control node (`bigip_device`, `bigip_device_group`, `bigip_virtual_server`, etc.).  
- All required roles available and configured in the Ansible project directory.  

---

## 📂 Repository Structure

- **standalone/** — Consolidated and ready-to-use playbooks for standalone F5 deployments.  
- **high_availability/** — Playbooks for high-availability (HA) F5 deployments. **Currently in development and not yet ready for production use**.  
- **roles/** — Contains all Ansible roles used across playbooks (backup, restore, disable, create tenants, etc.).  
- **ansible.cfg** — Ansible configuration file.  
- **README.md** — This documentation.  
- **LICENSE** — GPL‑3.0 license.

---

## ⚡ Highlights

- **Standalone Code Consolidation:**  
  The standalone workflow has been cleaned up and consolidated to simplify deployment and reduce complexity.  

- **High Availability Workflow:**  
  The HA code is still under development. It will eventually support multi-device, synchronized deployments.

---

## ▶ Usage (Standalone)

1. Clone the repository and navigate to the `standalone/` folder.  
2. Configure your inventory with source/destination device IPs and credentials.  
3. Run the playbook using **Ansible Automation Platform** or `ansible-playbook`.  

---

## 📖 Notes

- For HA deployments, check back later as the high-availability playbooks are still under development.  
- Ensure all roles and prerequisites are properly configured before running the playbook to prevent errors.  

