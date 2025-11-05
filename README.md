# F5 Platform Modernization Ansible Automation

This Ansible playbook automates the modernization of F5 platforms, enabling seamless migration from older F5 BIG-IP models to newer ones. It facilitates the backup, migration, upgrade, and restoration of critical configurations, including UCS (User Configuration Set) files, crypto files, and other essential components. Additionally, this playbook supports version upgrades, such as migrating from lower versions (e.g., 15.1.x) to more recent releases (e.g., 17.5.x).

---

## ⚠️ PLEASE READ FIRST!

- This code is a **WORK IN PROGRESS**, if there are any issues please Report.
- The **Standalone code** has been tested for both **rSeries** and **Velos** Destination Deployments.  
- The **High Availability code** has been tested for **rSeries** Destination Deployments.  (will be tested on Velos Soon)

---

## 🛠 Prerequisites

Before using this playbook, ensure you have:

- Ansible Automation Platform configured in your environment.  
- Access to both the source and destination F5 BIG-IP devices.  
- A **backup host** for storing UCS and crypto files.  
- SSH access to the backup host and F5 devices.  
- The necessary F5 BIG-IP Collections installed on the Execution Environment `f5networks.f5_modules, f5networks.f5_bigip, f5networks.f5os`

---

## 📂 Repository Structure

- **standalone/** — Consolidated and ready-to-use playbooks for standalone F5 deployments.  
- **high_availability/** — Playbooks for high-availability (HA) F5 deployments.  
- **roles/** — Contains all Ansible roles used across playbooks (backup, restore, disable, create tenants, etc.).  
- **ansible.cfg** — Ansible configuration file.  
- **README.md** — This documentation.  
- **LICENSE** — GPL‑3.0 license.

---

## ⚡ Highlights

- **High Availability Code**  
  The HA code can be done in two different ways `combined` which means both units will be upgraded at the same time, or `sequential` where the standby unit would be upgraded first then the active unit would be failed over and the new standby would be upgraded.

---

## ▶ Usage (Standalone)

1. In **Ansible Automation Platform** import the git Repository as a Project.
2. Configure your inventory with source/destination device IPs and credentials.
3. In **Ansible Automation Platform** Create Templates based on your folder and code `standalone, ha_combined, ha_sequential`  
3. Run the playbook using **Ansible Automation Platform**.

---

## 📖 Notes
