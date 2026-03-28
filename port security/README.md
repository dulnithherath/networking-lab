# Port Security (Static MAC Address)

## Overview

This lab demonstrates how to configure **port security** on a Cisco switch using a static MAC address. The goal is to allow only a specific authorized device to connect on a switch port, and automatically shut down the port if an unauthorized device attempts to connect.

---

## Topology

| Device | IP Address | Connected Port |
|---|---|---|
| PC (User 1) | 192.168.1.1 | Fa0/1 |
| PC (User 2) | 192.168.1.2 | Fa0/2 |
| Network Admin PC | 192.168.1.3 | Fa0/3 |
| Unauthorized Laptop | 192.168.1.10 | (not allowed) |


---

## Objectives

- Configure port security on Fa0/3 (Network Admin port)
- Allow only the Network Admin MAC address (`0010.1157.9427`)
- Set violation mode to **Shutdown**
- Test that an unauthorized device triggers a port shutdown
- Recover the port after a violation

---

## Configuration Commands

```bash
Switch> enable
Switch# configure terminal

Switch(config)# interface fastEthernet 0/3
Switch(config-if)# switchport mode access
Switch(config-if)# switchport port-security
Switch(config-if)# switchport port-security mac-address 0010.1157.9427
Switch(config-if)# switchport port-security maximum 1
Switch(config-if)# switchport port-security violation shutdown
Switch(config-if)# end
```

---

## Verification Commands

```bash
# View all secured ports
Switch# show port-security

# View detailed status for Fa0/3
Switch# show port-security interface fa0/3
```

---

## Port Recovery (After Violation)

```bash
Switch# configure terminal
Switch(config)# interface fastEthernet 0/3
Switch(config-if)# shutdown
Switch(config-if)# no shutdown
Switch(config-if)# end
```
