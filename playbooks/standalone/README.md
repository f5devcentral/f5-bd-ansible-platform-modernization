# Standalone — F5 Platform Modernization (Ansible)

This Ansible playbook automates the modernization of F5 platforms, enabling seamless migration from older F5 BIG-IP models to newer ones. It facilitates the backup, migration, upgrade, and restoration of critical configurations, including UCS (User Configuration Set) files, crypto files, and other essential components. Additionally, this playbook supports version upgrades, such as migrating from lower versions (e.g., 15.1.x) to more recent releases (e.g., 17.5.x).

This folder contains consolidated Ansible playbooks and supporting files for **standalone** F5 BIG-IP systems. The playbooks automate backup, migration, upgrade, and restoration of critical configurations (UCS, crypto, tenants, etc.) for single-node F5 deployments.

---

## ⚠️ PLEASE READ FIRST!

- This code is a **WORK IN PROGRESS**.  
- Tested on **iSeries Platforms, VCMP Guests, Virtual Instances** as the source and a **Velos with BX110 Blade System** and **rSeries R5800** as the destination.  
- Firmware on the Velos Chassis Controller and partition uses **F5OS 1.8.1 GA** due to management IP constraints.  
- This code has been validated with source systems that use VLANs on the BIG-IP Systems, **it hasnt been tested with untagged networks**

---

## 🛠 Prerequisites

Before using this playbook, ensure you have:

- Ansible Automation Platform configured in your environment.  
- Access to both the source and destination F5 BIG-IP devices.  
- A **backup host** for storing UCS and crypto files.  
- SSH access to the backup host and F5 devices.  
- The necessary F5 BIG-IP Collections installed on the Execution Environment `f5networks.f5_modules, f5networks.f5_bigip, f5networks.f5os`

---

## Quickstart (Standalone)

1. Import the repository into Ansible Automation Platform as a Project.
2. Configure inventory and credential objects for:
   - source BIG-IP (source_host)
   - destination BIG-IP (destination_host)
   - backup host (backup_host)
3. Examine `host_vars/*.yml` files to determine if changes are needed (Fork of Code) to operationalize your environment.
4. Create a Job Template referencing the `migrate_standalone.yaml` playbook and the `standalone` inventory group with the appropriate Execution Engine.
5. Add additional variables in your Job Template based on your environment (examine `extra_vars_in_aap` folder for Destination Type/Backup Path and Build Version)
6. Run the Job Template.

---

## 📂 Standalone Directory Structure

- `migrate_standalone.yml` — Canonical orchestrator playbook (entry point).
- **../roles/** — Reusable roles referenced by the playbook (backup, restore, tenant_create, validate, etc.).  
- **host_vars/** — — variables for standalone runs.
- **README.md** — This document.

---

## 📖 Notes

- Ensure all roles and prerequisites are properly configured before running the playbook to prevent errors.  
