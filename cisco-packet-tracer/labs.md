# Cisco Packet Tracer Labs

> **Lab Files:** All `.pkt` files are available for 
> download in this folder. Open with Cisco Packet Tracer 
> to interact with the lab configurations directly.

## Lab 1 — Simple LAN (Star Topology)
**File:** A simple network.pkt

### What I Built
Configured a simple LAN for a small startup — connecting 
2 PCs, 2 laptops, and a server through a Cisco 2960 switch 
using a star topology. All devices assigned static IPs on 
the 192.168.1.0/24 subnet.

### Key Concepts
- Star topology — all devices connect to a central switch
- Static IP assignment on 192.168.1.0/24 subnet
- Verified full communication via ping

### What I Learned
Every device needs an IP in the same subnet to communicate.
The switch acts as the central traffic manager.

---

## Lab 2 — Hub vs Switch Comparison
**File:** Hub Build.pkt + switch.pkt

### What I Built
Built the same star topology network twice — first using 
a hub, then a switch — to observe how traffic flows 
differently between the two.

### The Problem I Solved
**Question:** Why did switches replace hubs in modern networks?

**With the Hub:**
- Every frame was flooded to ALL devices regardless of destination
- No intelligence — every device sees every packet
- Security risk — any device can sniff all traffic
- Inefficient — bandwidth wasted on unnecessary traffic

**With the Switch:**
- Traffic goes ONLY to the intended recipient
- Switch learns each device's MAC address automatically
- Communication is efficient and private

### How I Verified It
Used Cisco Packet Tracer simulation mode to watch packets 
travel step by step — seeing the hub flood traffic to all 
ports while the switch sent it only to the correct port.

### SOC Relevance
Understanding hub vs switch behaviour explains why 
network sniffing attacks (like ARP poisoning) are harder 
on switched networks — and why attackers use techniques 
like ARP spoofing to force traffic their way.

---

## Lab 3 — Two Connected Networks (Inter-network Routing)
**File:** Two connected network.pkt

### What I Built
Two separate LANs connected through a Cisco 1941 Router:
- Network One: 192.168.1.0/24 — Server, Switch, PCs, Laptops
- Network Two: 192.168.2.0/24 — Switch, PCs, Laptops
- DNS Server handling name-to-IP resolution across both networks
- Router connecting both via configured GigabitEthernet interfaces

### The Problem I Solved
**Challenge:** Making two separate networks communicate 
through a router.

Key steps:
- Configured GigabitEthernet interfaces on the router
  with correct IPs for each network
- Set default gateway on each device pointing to the router
- Configured DNS server for hostname resolution

**The test that made theory click:**
Removing the default gateway from one PC instantly broke 
cross-network communication. Restoring it restored 
connectivity immediately. Simple test — powerful lesson.

### Key Concepts Learned
- Default gateway — the router IP devices use to reach 
  other networks
- Subnet masks must match within the same network
- DNS resolves hostnames to IPs across networks
- Routers operate at Layer 3 — they route between networks
- Switches operate at Layer 2 — they switch within a network

### SOC Relevance
Understanding routing is essential for reading network 
logs. Knowing which gateway traffic should pass through 
helps identify anomalous routing — a common indicator 
of man-in-the-middle attacks or network compromise.
