Basic Extended ACL Lab


Objective - 
Block the "Intern Workstation" from accessing the Internal File Server while allowing only the "Finance Manager Workstation" to reach it. All other traffic is permitted normally.


Devices Used

| Device | Model | Role |

| Core Gateway Router | Cisco 1841 | Central router |
| Switch0 | Cisco 2960-24TT | LAN switch |
| Finance Manager Workstation | PC-PT | Allowed client |
| Intern Workstation | PC-PT | Blocked client |
| Internal File Server | Server-PT | Protected server |

IP Addressing Table

| Device | Interface | IP Address | Subnet Mask | Default Gateway |

| Core Gateway Router | Fa0/0 | 192.168.1.1 | 255.255.255.0 | — |
| Core Gateway Router | Fa0/1 | 192.168.10.1 | 255.255.255.0 | — |
| Finance Manager Workstation | Fa0 | 192.168.1.10 | 255.255.255.0 | 192.168.1.1 |
| Intern Workstation | Fa0 | 192.168.1.20 | 255.255.255.0 | 192.168.1.1 |
| Internal File Server | Fa0 | 192.168.10.10 | 255.255.255.0 | 192.168.10.1 |

Interface Connections

| From | Interface | To | Interface |

| Core Gateway Router | Fa0/0 | Switch0 | Fa0/3 |
| Core Gateway Router | Fa0/1 | Internal File Server | Fa0 |
| Switch0 | Fa0/1 | Finance Manager Workstation | Fa0 |
| Switch0 | Fa0/2 | Intern Workstation | Fa0 |



## ACL Scenario

Policy:
- Finance Manager (`192.168.1.10`) → File Server (`192.168.10.10`) Allowed
- Intern (`192.168.1.20`) → File Server (`192.168.10.10`) Blocked
- All other traffic → Allowed

ACL Type: Extended ACL (numbered — I used ACL 100)  
Applied on:Router Fa0/1 — inbound


Configuration Commands

Step 1 — Assign IP addresses on the router


Router(config)# interface FastEthernet0/0
Router(config-if)# ip address 192.168.1.1 255.255.255.0
Router(config-if)# no shutdown

Router(config)# interface FastEthernet0/1
Router(config-if)# ip address 192.168.10.1 255.255.255.0
Router(config-if)# no shutdown


Step 2 — Create the Extended ACL


Router(config)# access-list 100 permit ip host 192.168.1.10 host 192.168.10.10
Router(config)# access-list 100 deny ip any host 192.168.10.10
Router(config)# access-list 100 permit ip any any


Step 3 — Apply ACL to the interface


Router(config)# interface FastEthernet0/1
Router(config-if)# ip access-group 100 in


## Test Results

| Source | Destination | Expected | Result |

| Finance Manager (192.168.1.10) | File Server (192.168.10.10) | Allowed | Pass |
| Intern (192.168.1.20) | File Server (192.168.10.10) | Blocked | Blocked |
| Any PC | Any other destination | Allowed | Pass |

