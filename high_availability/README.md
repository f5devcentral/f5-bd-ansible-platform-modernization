# F5 Platform Modernization Ansible Automation - High Availability

This Ansible playbook automates the modernization of F5 platforms, enabling seamless migration from older F5 BIG-IP models to newer ones. It facilitates the backup, migration, upgrade, and restoration of critical configurations, including UCS (User Configuration Set) files, crypto files, and other essential components. Additionally, this playbook supports version upgrades, such as migrating from lower versions (e.g., 15.1.x) to more recent releases (e.g., 17.5.x).

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

## 📂 High_Availability Directory Structure

- **group_vars/** — Variables used to Connect to the Source and Destination Environments, also the `ha_pair_destination_chassis` contains information on the build of the F5OS Tenant OS - This is for migrate_ha_sequential code
- **migrate_ha_combined_...** — Code is specific for the migration of HA Pairs migrating both units at the same time **(will cause outage)**
- **migrate_ha_sequential_...** — Code is specific for the migration of HA Pairs migrating both units in sequence (Standby then Active) This should mitigate any outage issues.
- **README.md** — This documentation.  

---

## ⚡ Highlights

- **High Availability Workflow:**  
  There are 2 different sets of Code, Combined (Where both HA Units are migrated at the same time), and Sequential (where Standby is migrated first then failed over and then the Previous Active Unit is migrated)

---

## 📖 Notes

- Ensure all roles and prerequisites are properly configured before running the playbook to prevent errors.  

