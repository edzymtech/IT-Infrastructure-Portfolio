# Cisco Network Lab: Enterprise Inter-VLAN Routing, DHCP Relay & Break/Fix Simulation

## 📌 Project Overview
This repository contains a Cisco Packet Tracer simulation designed to showcase hands-on proficiency in enterprise network infrastructure, Layer 2/3 traffic management, and practical network troubleshooting. 

The primary objective of this laboratory is to demonstrate **centralized IP address distribution across multiple segmented VLANs using a single central DHCP Server via `ip helper-address` (DHCP Relay)** over an enterprise MDF/IDF switch hierarchy.

---

## 📐 Network Architecture & Hardware Role Inventory

The topology models a multi-department enterprise network using a standard **Main Distribution Frame (MDF) and Intermediate Distribution Frame (IDF)** hierarchy:

| Device Name | Device Model / Type | Network Role & Function |
| :--- | :--- | :--- |
| **`MAIN_ROUTER`** | Cisco ISR 4331/K9 | Central Layer 3 Gateway performing Router-on-a-Stick (ROAS) Inter-VLAN routing and acting as the **DHCP Relay Agent** via `ip helper-address`. |
| **`BRIDGE_SWITCH_1`** | Cisco Switch | Layer 2 bridge link connecting `MAIN_ROUTER` directly to the central **DHCP Server (`Server0`)**. |
| **`MDF_SWITCH`** *(Switch 6)* | Cisco 2960 Switch | **Main Distribution Frame (MDF)** core switch managing primary trunk connections across all departments. |
| **`HR_SWITCH`** *(Switch 7)* | Cisco 2960 Switch | **IDF** servicing the Human Resources Department (VLAN 20). |
| **`COMLAB_SWITCH`** *(Switch 11)* | Cisco 2960 Switch | **IDF** servicing the Computer Laboratory Room (VLAN 10). |
| **`HELPDESK_SWITCH`** *(Switch 9)* | Cisco 2960 Switch | **IDF** servicing the Physical Helpdesk Room (VLAN 30). |
| **`ITDEPT_SWITCH`** *(Switch 0)* | Cisco 2960 Switch | **IDF** servicing the IT Department (VLAN 50). |
| **`Guest AP`** *(AP 1)* | Lightweight Access Point | Wireless Access Point broadcasting Guest Wi-Fi (VLAN 40). |
| **`ComLab AP`** *(AP 2)* | Lightweight Access Point | Wireless Access Point broadcasting Computer Lab Wi-Fi (VLAN 10). |

---

## 🛠️ Key Technical Implementations

1. **VLSM / CIDR Subnetting:** IPv4 address space allocation custom-sized for individual departmental host capacities.
2. **VLAN Segmentation & Trunking (802.1Q):** Dynamic isolation of department traffic across MDF/IDF trunk links.
3. **Centralized DHCP Relay Agent:** `MAIN_ROUTER` captures Layer 2 broadcast DHCP requests on subinterfaces and forwards them as unicast packets to the central `Server0` via `ip helper-address`.
4. **Layer 2 Infrastructure Hardening (Unused Port Management):** To prevent unauthorized physical access and mitigate default VLAN 1 exploitation, all unused/unassigned ports across the MDF and IDF switches are assigned to a dedicated unused parking VLAN and administratively disabled (`shutdown`).
---

## 🧪 Simulated Real-World Break/Fix Scenarios

To model real-world enterprise IT support tickets, this lab includes specific "Broken vs. Fixed" troubleshooting scenarios:

### Scenario 1: Baseline Operational State
* **Status:** All trunk links active, subinterfaces operational, and end devices across all VLANs successfully acquire lease IPs from `Server0`.

### Scenario 2: Provisioning of home router as an AP to the HR Department
* **Root Cause:** A consumer router (`PT-AC`) placed in HR was running active DHCP/NAT out of the box, conflicting with `Server0`.
* **Fix Applied:** Disabled local DHCP/NAT on the home router, bypassed the WAN port, connected via LAN port `G0/2` to convert it into a pure **Layer 2 Access Point**, and adjusted Layer 2 Port Security policies on `HR_SWITCH (Fa0/2)` to allow bridged wireless client traffic.

### Scenario 3: VLAN Misconfiguration on PC0
* **Symptom:** PC0 getting APIPA.
* **Root Cause:** Misconfigured Fa0/5 interface on COMLAB_SWITCH, configured as shutdown and placed in VLAN 100 as unused port.
* **Fix Applied:** Re-configured the Fa0/5 interface, accessing the VLAN 10 to align with the computer laboratory default gateway.

### Scenario 4: Guest Wi-Fi Access Point Port Misconfiguration
* **Symptom:** Wireless guest users unable to pull IP addresses or connect to the Helpdesk Access Point (`AP 1`).
* **Root Cause:** The switch port connecting `HELPDESK_SWITCH` to `AP 1` was left in default VLAN 1 instead of Guest VLAN 40.
* **Fix Applied:** Reconfigured the port as `switchport access vlan 40`, allowing guest DHCP requests to relay cleanly to `Server0`.

---

## 💡 Practical Skills Practiced

1. **Integration of VLANs and inter-VLAN routing.** 
2. **DHCP Relay distribution to different VLANs.**
3. **VLSM/CIDR Subnetting.** 
4. **Proper documentation.**

---
