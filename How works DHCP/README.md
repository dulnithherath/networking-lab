# Basic DHCP Lab – Cisco Packet Tracer

A beginner-level network lab demonstrating how to configure a **DHCP server on a Cisco router** to automatically assign IP addresses to end devices.

**I Have attched pictures of How to do it.**

---

## Lab Overview

| Detail | Value |
|---|---|
| Tool | Cisco Packet Tracer |
| Difficulty | Beginner |
| Topic | DHCP Configuration |
| Devices | 1 Router (1841), 1 Switch (2950), 4 PCs |

---

## Network Topology

```
         Router0 (1841)
         Fa0/0: 192.168.1.1/24
              |
           Switch
        /    |    \    \
      PC0   PC1   PC2   PC3
  (DHCP) (DHCP) (DHCP) (DHCP)
```

---

## Configuration

### Router Interface (Fa0/0)

```
Router> en
Router# configure terminal
Router(config)# interface fastEthernet 0/0
Router(config-if)# ip address 192.168.1.1 255.255.255.0
Router(config-if)# no shutdown
Router(config-if)# exit
```

### DHCP Pool Setup

```
Router(config)# ip dhcp pool Mainnetwork
Router(dhcp-config)# network 192.168.1.0 255.255.255.0
Router(dhcp-config)# default-router 192.168.1.1
Router(dhcp-config)# dns-server 8.8.8.8
```

---

## Verification

After setting the PCs to **DHCP mode**, they automatically received:

| Setting | Value |
|---|---|
| IPv4 Address | 192.168.1.2 (and onwards) |
| Subnet Mask | 255.255.255.0 |
| Default Gateway | 192.168.1.1 |
| DNS Server | 8.8.8.8 |

The PC desktop confirmed: **"DHCP request successful."**



