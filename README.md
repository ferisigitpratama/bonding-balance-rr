# B2B Connectivity Redundancy with MikroTik Bonding Balance RR over Dual Metro Ethernet

This project demonstrates a **B2B connectivity simulation** using **MikroTik Bonding in balance-rr mode** over **dual Metro Ethernet paths** to improve **service availability, link redundancy, and traffic resiliency** between the provider side and the customer side.

The lab is designed to simulate how an ISP or network engineer can deliver a more reliable **enterprise / B2B connection** by combining **two active transport links**, **bonding-based load distribution**, **ARP link monitoring**, and **VLAN-based Metro Ethernet transport**. The main objective is to keep customer connectivity running even when one of the transport paths fails.

---

## Project Overview

In many B2B environments, connectivity requirements go beyond basic internet access. Enterprise customers often require **higher availability**, **reduced downtime**, and **better service continuity** for branch offices, corporate operations, and dedicated services.

To address this scenario, this lab uses **MikroTik Bonding (balance-rr)** to aggregate two Metro Ethernet links between the provider and customer routers. Traffic is distributed across both active links, while **ARP monitoring** is used to detect path failures and maintain connectivity through the remaining available path.

This project focuses on the practical implementation of **redundant customer connectivity** in a B2B service environment, where network stability and failover capability are just as important as bandwidth delivery.

---

## Objectives

The purpose of this project is to simulate a **redundant B2B customer connection** that can provide:

- **Higher service availability** between provider and customer
- **Traffic distribution across multiple active links**
- **Path redundancy** in case one Metro Ethernet transport fails
- **Service continuity** for enterprise / business customers
- **A practical lab example** of MikroTik Bonding for B2B connectivity delivery

---

## Key Features

- **MikroTik Bonding in balance-rr mode**
- **Dual Metro Ethernet transport path**
- **ARP-based link monitoring** for path failure detection
- **Provider-to-customer redundant connectivity design**
- **VLAN-based service transport**
- **LACP trunk between intermediate switches**
- **Failover testing by shutting down one transport path**
- **B2B / enterprise service simulation**

---

## Topology Overview

This project simulates a connection between the **provider side** and the **customer side** using **two separate Metro Ethernet paths**. The provider router and customer router are connected through intermediate switches, with both transport paths carried simultaneously to create a redundant design.

The overall concept is:

- **Provider Router** and **Customer Router** are connected using a bonded interface
- The bonded interface uses **two physical uplinks**
- Each uplink traverses a separate **Metro Ethernet path**
- Intermediate switches carry the transport using **VLAN separation**
- **LACP trunking** is used on the switching side to maintain stable uplink connectivity
- If one Metro Ethernet path fails, traffic continues through the remaining active path

---

## High-Level Topology

```text
+----------------+                               +----------------+
| Provider Router|                               | Customer Router|
|   RO-MNG1      |                               |   RO-CLIENT    |
+--------+-------+                               +--------+-------+
         |                                                |
         |<----------- MikroTik Bonding (balance-rr) ---->|
         |                                                |
         +-----------------+        +---------------------+
                           |        |
                     Metro Ethernet Path 1
                     Metro Ethernet Path 2
                           |        |
                  +--------+--------+--------+
                  |   Intermediate Switches  |
                  |   VLAN + LACP Transport  |
                  +--------------------------+
```

---

## Devices / Components

This lab uses the following components:

- **MikroTik Router (Provider Side / RO-MNG1)**
- **MikroTik Router (Customer Side / RO-CLIENT)**
- **Intermediate Switches** for Metro Ethernet transport
- **Dual transport links** representing Metro Ethernet 1 and Metro Ethernet 2
- **VLAN-based transport path**
- **Bonded interfaces** on both routers

---

## Bonding Design

The bonding implementation uses **balance-rr (Round Robin)** mode. In this mode, traffic is transmitted sequentially across all active slave interfaces. This allows traffic to be distributed across both links and provides a simple way to utilize multiple paths simultaneously.

### Bonding Parameters Used

- **Mode:** `balance-rr`
- **Link Monitoring:** `ARP Monitoring`
- **ARP Interval:** `100ms`
- **ARP Targets:** `192.168.11.2`, `192.168.12.2`

Example bonding reference used in this project:

```rsc
/interface bonding
add name=bond1 mode=balance-rr arp=enabled arp-interval=100ms \
arp-ip-targets=192.168.11.2,192.168.12.2 slaves=ether1,ether2
```

> ARP monitoring is used to detect whether the remote path is still reachable. If one transport path becomes unavailable, the bonded interface can stop using the failed link and continue forwarding traffic through the remaining active path.

---

## Metro Ethernet Transport Design

The transport side of this project is built to simulate **Metro Ethernet delivery for B2B services**. Instead of connecting the two MikroTik routers directly, the links are carried across intermediate switches to represent a provider transport network.

The design includes:

- **Two transport paths** to simulate dual Metro Ethernet delivery
- **VLAN-based separation** for service transport
- **Switch interconnection using LACP trunk**
- **Multiple VLAN segments** for B2B service testing

This approach reflects a more realistic enterprise connectivity scenario where customer services are delivered through a provider switching infrastructure rather than a direct point-to-point cable.

---

## B2B Use Case

This project is intended to represent a **B2B / enterprise connectivity service**, where the customer requires better reliability than a standard single-link connection.

Example use cases include:

- Branch office connectivity
- Corporate WAN access
- Dedicated enterprise transport
- Redundant customer uplink delivery
- Business services that require reduced downtime and stable connectivity

In this type of environment, the focus is not only on bandwidth, but also on **redundancy**, **failover capability**, and **service continuity**.

---

## Testing Scenario

To validate the redundancy design, a failover test was performed by **shutting down one of the transport paths** during active forwarding.

### Test Goal

The goal of the test was to verify that:

- the bonding interface remains operational,
- traffic continues through the remaining available path,
- the service is not interrupted when one Metro Ethernet link fails.

### Expected Result

When one transport path is disconnected or shut down:

- ARP monitoring detects the failure
- the failed path is no longer used by the bonding interface
- traffic is forwarded through the remaining active path
- connectivity between provider and customer remains available

This test demonstrates how dual-path bonding can improve service resiliency for B2B customer delivery.

---

## Why Balance RR?

`balance-rr` was selected in this lab to demonstrate **active-active traffic distribution** across two available transport paths. It is useful for showing how traffic can be spread across multiple links while also maintaining a level of redundancy.

### Advantages in this Lab

- Uses both links actively
- Demonstrates traffic distribution across dual uplinks
- Can provide redundancy when combined with monitoring
- Suitable for simulation and lab testing of resilient transport design

### Notes

In production, the choice of bonding mode depends on the service design, transport behavior, upstream devices, and application requirements. This lab specifically focuses on the **concept of B2B redundancy and failover simulation**, rather than defining a universal production standard.

---

## Configuration Notes

The project uses the following main logic:

- Create a bonding interface on the provider router
- Create a bonding interface on the customer router
- Add two physical interfaces as bonding slaves
- Enable ARP monitoring for path health detection
- Carry transport traffic over dual Metro Ethernet paths
- Use VLANs to separate service traffic on the switching side
- Perform failover testing by disabling one path

---

## Repository Contents

This repository contains documentation and configuration references for the lab, including:

- Router configuration files
- Switch configuration files
- Topology screenshots
- Bonding interface screenshots
- IP addressing screenshots
- Failover test screenshots
- Project documentation

---

## Screenshots / Documentation

You can include the following screenshots in this repository to make the documentation easier to understand:

- **Topology Diagram**
- **Bonding Configuration on RO-MNG1**
- **Bonding Configuration on RO-CLIENT**
- **Interface Status on Provider Router**
- **Interface Status on Customer Router**
- **Switch Interface / VLAN / LACP Configuration**
- **IP Addressing on Both Routers**
- **Failover Test / Link Shutdown Result**

If the screenshots are stored in an `images/` folder, you can reference them like this:

```md
## Topology
![Topology](images/topo.png)

## Bonding on Provider Router
![Bonding Provider](images/bonding-ro-mng1.png)

## Bonding on Customer Router
![Bonding Customer](images/bonding-ro-client.png)

## Failover Test
![Failover Test](images/shutdown.png)
```

---

## Suggested Folder Structure

```text
bonding-b2b-balance-rr/
├── README.md
├── configs/
│   ├── RO-MNG1.cfg
│   ├── RO-CLIENT.cfg
│   ├── SW-MNG1.cfg
│   └── SW-MNG2.cfg
├── images/
│   ├── topo.png
│   ├── bonding-ro-mng1.png
│   ├── bonding-ro-client.png
│   ├── interface-ro-mng1.png
│   ├── interface-ro-client.png
│   ├── interface-sw-mng1.png
│   ├── interface-sw-mng2.png
│   ├── ip-ro-mng1.png
│   ├── ip-ro-client.png
│   └── shutdown.png
└── docs/
    └── testing-notes.md
```

---

## Learning Points

This project highlights several important concepts in enterprise and ISP service delivery:

- **Bonding implementation on MikroTik**
- **Active-active link utilization with balance-rr**
- **ARP-based link monitoring**
- **Redundant transport design for B2B services**
- **VLAN-based Metro Ethernet service delivery**
- **LACP switching design for transport stability**
- **Failover validation and service continuity testing**

---

## Conclusion

This lab demonstrates how **MikroTik Bonding (balance-rr)** can be used to simulate a **redundant B2B connectivity service** over **dual Metro Ethernet paths**. By combining active transport links, ARP monitoring, VLAN-based transport, and failover testing, the project provides a practical example of how enterprise customer connectivity can be designed with better resilience and higher service availability.

For B2B and enterprise environments, reliable connectivity is not only about bandwidth delivery, but also about ensuring that services remain available when network failures occur. This project was built to reflect that principle through a simple but practical redundancy lab.

---

## Author

**Feri Pratama**  
Network Engineer | NOC / ISP | Routing & Switching | B2B Connectivity | MikroTik Lab
