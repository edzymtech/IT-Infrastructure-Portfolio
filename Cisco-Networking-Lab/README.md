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

1. **VLSM / CIDR Subnetting:** Efficient IPv4 address space allocation custom-sized for individual departmental host capacities.
2. **VLAN Segmentation & Trunking (802.1Q):** Dynamic isolation of department traffic across MDF/IDF trunk links.
3. **Centralized DHCP Relay Agent:** `MAIN_ROUTER` captures Layer 2 broadcast DHCP requests on subinterfaces and forwards them as unicast packets to the central `Server0` via `ip helper-address`.
4. **Selective Layer 2 Port Security:** Enforced on workstation access ports (`mac-address sticky`, `maximum 1`, `violation shutdown`) to prevent unauthorized hardware insertion while allowing transparent Layer 2 AP bridging where appropriate.

---

## 🧪 Simulated Real-World Break/Fix Scenarios

To model real-world enterprise IT support tickets, this lab includes specific "Broken vs. Fixed" troubleshooting scenarios:

### Scenario 1: Baseline Operational State
* **Status:** All trunk links active, subinterfaces operational, and end devices across all VLANs successfully acquire lease IPs from `Server0`.

### Scenario 2: Rogue DHCP / Consumer AP Conflict in HR Department
* **Symptom:** HR workstations failing to reach the central gateway or receiving unauthorized IP pool leases.
* **Root Cause:** A consumer router (`PT-AC`) placed in HR was running active DHCP/NAT out of the box, conflicting with `Server0`.
* **Fix Applied:** Disabled local DHCP/NAT on the home router, bypassed the WAN port, connected via LAN port `G0/2` to convert it into a pure **Layer 2 Access Point**, and adjusted Layer 2 Port Security policies on `HR_SWITCH (Fa0/2)` to allow bridged wireless client traffic.

### Scenario 3: Trunk Link Misconfiguration & VLAN Pruning
* **Symptom:** Inter-VLAN communication loss between IDF switches and `MDF_SWITCH`.
* **Root Cause:** Misconfigured 802.1Q trunking encapsulation or missing allowed VLAN lists on switch-to-switch links.
* **Fix Applied:** Re-established `switchport mode trunk` and verified native VLAN alignment across all MDF/IDF links.

### Scenario 4: Guest Wi-Fi Access Point Port Misconfiguration
* **Symptom:** Wireless guest users unable to pull IP addresses or connect to the Helpdesk Access Point (`AP 1`).
* **Root Cause:** The switch port connecting `HELPDESK_SWITCH` to `AP 1` was left in default VLAN 1 instead of Guest VLAN 40.
* **Fix Applied:** Reconfigured the port as `switchport access vlan 40`, allowing guest DHCP requests to relay cleanly to `Server0`.

---

## 💡 Practical Skills Practiced

* **Layer 2 & Layer 3 Troubleshooting:** Isolating faults across physical, data link, and network layers.
* **Cisco IOS CLI Mastery:** Commands including `show ip dhcp binding`, `show interfaces trunk`, `show port-security interface`, and `show ip route`.
* **Enterprise Infrastructure Design:** Implementing structured cabling standards (MDF/IDF) and security hardening.
* **Technical Documentation:** Maintaining clean network topologies, IP addressing tables, and ticket resolution logs.

---
