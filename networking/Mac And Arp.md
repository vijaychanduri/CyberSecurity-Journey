MAC Addresses, Ethernet & ARP 🌐
This chapter connects the concepts from Chapter 2 (IP addressing) to what actually happens on a local network (LAN).
The three big things we'll learn are:
MAC address → Ethernet → ARP
These are extremely important for understanding switches, packet captures, and many network-security concepts.
1. Where Chapter 3 Fits
Previously, we learned:
IP address
   ↓
Network
   ↓
Subnet
   ↓
Host
Now we'll go one level deeper.
Suppose:
PC-A
IP: 192.168.1.10

PC-B
IP: 192.168.1.20
How does PC-A actually deliver data to PC-B on the local Ethernet/Wi-Fi network?
It needs link-layer addressing.
That's where MAC addresses come in.
2. What Is a MAC Address? ⭐
MAC stands for Media Access Control.
A MAC address is a link-layer address associated with a network interface.
Example:
00:1A:2B:3C:4D:5E
Another common representation is:
00-1A-2B-3C-4D-5E
A MAC address is normally written as six hexadecimal pairs.
00 : 1A : 2B : 3C : 4D : 5E
Each pair represents 8 bits:
6 × 8 = 48 bits
So a typical MAC address is 48 bits.
3. What Is Hexadecimal?
You'll see hexadecimal constantly in networking and cybersecurity.
Hexadecimal is base 16.
It uses:
0 1 2 3 4 5 6 7 8 9 A B C D E F
Where:
A = 10
B = 11
C = 12
D = 13
E = 14
F = 15
For example:
FF
represents:
255
And:
0A
represents:
10
You don't need to become an expert in hexadecimal yet. Just recognize it.
4. MAC Address vs IP Address ⭐⭐⭐
This is one of the most important distinctions in networking.
MAC Address	IP Address
Link-layer address	Network-layer address
Used on local network technologies	Used for IP networking
Commonly 48 bits for Ethernet	IPv4 is 32 bits
Written in hexadecimal	IPv4 written in decimal
Associated with network interface	Assigned/configured at the IP layer
Used by switches for Ethernet forwarding	Used by routers for IP forwarding
Example:
Computer
│
├── MAC: AA:BB:CC:DD:EE:FF
│
└── IP: 192.168.1.10
Think of it this way:
IP  → Where is the destination in the IP network?

MAC → Which local link-layer interface should receive this frame?
This is simplified, but it's a useful starting mental model.
5. Is a MAC Address Permanent?
A common beginner statement is:
"A MAC address is permanently burned into the computer."
That's not always true.
A network interface can have a factory-assigned MAC address, but operating systems and network software can sometimes use a different or randomized MAC address.
For example, Wi-Fi devices may use MAC randomization for privacy.
So remember:
A MAC address identifies a network interface at the link layer, but it should not be treated as an unchangeable identity.
This distinction matters in cybersecurity.
6. What Is Ethernet? ⭐
Ethernet is a family of technologies used for networking, especially LANs.
For example:
PC ───── Ethernet cable ───── Switch
Ethernet defines things such as:
How devices communicate on the local link
Frame formats
MAC addressing
How Ethernet switches forward traffic
Ethernet operates primarily at OSI Layer 2 — Data Link.
7. Frame vs Packet ⭐⭐⭐
This is a very important distinction.
At different layers, network data has different names.
Simplified:
Application
    ↓
Data

Transport
    ↓
TCP segment / UDP datagram

Network
    ↓
IP packet

Data Link
    ↓
Ethernet frame
So an Ethernet frame can carry an IP packet:
┌─────────────────────────────────────┐
│ Ethernet Frame                      │
│                                     │
│  Ethernet Header                    │
│  ┌───────────────────────────────┐  │
│  │ IP Packet                     │  │
│  │                               │  │
│  │ IP Header + Data              │  │
│  └───────────────────────────────┘  │
│                                     │
│ Ethernet Trailer/FCS                │
└─────────────────────────────────────┘
This is part of encapsulation, which we introduced in Chapter 1.
8. Ethernet Frame
An Ethernet frame contains information needed for local link-layer delivery.
Simplified:
┌───────────────────────────────┐
│ Destination MAC               │
├───────────────────────────────┤
│ Source MAC                    │
├───────────────────────────────┤
│ Type / Length                 │
├───────────────────────────────┤
│ Payload                       │
├───────────────────────────────┤
│ FCS                           │
└───────────────────────────────┘
The important fields for now are:
Destination MAC
Who should receive the frame?
Source MAC
Who sent the frame?
Payload
The data carried by the frame.
FCS
A field used for detecting certain transmission errors.
9. Example Ethernet Frame
Suppose:
PC-A
MAC: AA:AA:AA:AA:AA:AA

PC-B
MAC: BB:BB:BB:BB:BB:BB
PC-A sends an Ethernet frame to PC-B:
Source MAC:
AA:AA:AA:AA:AA:AA

Destination MAC:
BB:BB:BB:BB:BB:BB
The switch looks at the destination MAC to determine where to forward the frame.
10. What Is a Switch? ⭐⭐⭐
A network switch connects devices on a LAN.
Example:
          ┌── PC-A
          │
PC-B ── Switch ── PC-C
          │
          └── Server
The switch uses MAC addresses to make forwarding decisions.
This is why switches are closely connected to MAC addresses.
11. How Does a Switch Know Where a MAC Address Is?
This is an important process.
A switch maintains a MAC address table.
Imagine:
MAC Address             Port

AA:AA:AA:AA:AA:AA       Port 1
BB:BB:BB:BB:BB:BB       Port 2
CC:CC:CC:CC:CC:CC       Port 3
The switch learns this information from the source MAC addresses of incoming Ethernet frames.
12. Switch Learning
Suppose PC-A sends a frame.
PC-A
MAC = AA:AA:AA:AA:AA:AA
      │
      ↓
   Switch
The switch receives the frame on Port 1.
It sees:
Source MAC =
AA:AA:AA:AA:AA:AA
So it learns:
AA:AA:AA:AA:AA:AA → Port 1
Later, if the switch receives traffic destined for that MAC, it knows where to send it.
13. What If the Switch Doesn't Know the Destination?
Suppose the switch knows:
AA → Port 1
but doesn't yet know where:
BB
is located.
If a frame arrives for BB, the switch may flood it out relevant ports, except the port on which it arrived.
Conceptually:
             PC-B
              ↑
              │
PC-A → Switch ─┼→ PC-C
              │
              └→ Server
The switch is trying to discover/reach the unknown destination.
Once it learns the destination's MAC location, future unicast traffic can be forwarded specifically.
14. Unicast, Broadcast, Multicast
You'll encounter these terms frequently.
Unicast
One sender → one receiver.
A ─────→ B
Broadcast
One sender → all relevant devices in the broadcast domain.
       ┌→ B
A ─────┼→ C
       ├→ D
       └→ E
IPv4 has the broadcast address concept.
The Ethernet broadcast MAC address is:
FF:FF:FF:FF:FF:FF
Multicast
One sender → a group of interested receivers.
       ┌→ B
A ─────┼→ D
       └→ E
Not every device necessarily receives/processes the multicast at the application level.
15. What Is ARP? ⭐⭐⭐
Now we reach one of the most important concepts in this chapter.
ARP = Address Resolution Protocol
ARP is used with IPv4 to discover the MAC address associated with an IPv4 address on the local network.
Suppose:
PC-A
IP: 192.168.1.10
MAC: AA:AA:AA:AA:AA:AA
wants to communicate with:
PC-B
IP: 192.168.1.20
MAC: BB:BB:BB:BB:BB:BB
PC-A knows:
Destination IP = 192.168.1.20
But it needs the destination's local MAC address to construct an Ethernet frame.
So it uses ARP.
16. ARP Step-by-Step ⭐⭐⭐
Step 1 — PC-A checks its ARP cache
PC-A asks:
"Do I already know the MAC address for 192.168.1.20?"
If yes, it can use it.
If not, it sends an ARP request.
Step 2 — ARP Request
PC-A sends a broadcast:
"Who has 192.168.1.20?"
The Ethernet destination is:
FF:FF:FF:FF:FF:FF
So devices on that local broadcast domain receive the frame.
Conceptually:
              PC-B
               ↑
               │
PC-A → Switch ─┼→ PC-C
               │
               └→ PC-D

"Who has 192.168.1.20?"
Step 3 — PC-B Responds
PC-B sees:
"That's my IP address."
It responds to PC-A:
"192.168.1.20 is at
 BB:BB:BB:BB:BB:BB"
Now PC-A knows:
192.168.1.20
       ↓
BB:BB:BB:BB:BB:BB
Step 4 — PC-A sends the actual Ethernet frame
Now PC-A can construct a frame:
Destination MAC:
BB:BB:BB:BB:BB:BB

Source MAC:
AA:AA:AA:AA:AA:AA
Inside that Ethernet frame is the IP packet:
Source IP:
192.168.1.10

Destination IP:
192.168.1.20
So we have:
Ethernet
┌─────────────────────────────┐
│ Dest MAC: BB:BB:...         │
│ Src MAC:  AA:AA:...         │
│                             │
│   IP                        │
│   ┌─────────────────────┐   │
│   │ Src: 192.168.1.10   │   │
│   │ Dst: 192.168.1.20   │   │
│   └─────────────────────┘   │
└─────────────────────────────┘
This relationship is very important.
17. ARP Cache
Computers don't normally want to perform ARP every time they communicate.
They maintain an ARP cache/table.
Conceptually:
IP Address       MAC Address

192.168.1.1      11:11:11:11:11:11
192.168.1.20     BB:BB:BB:BB:BB:BB
192.168.1.30     CC:CC:CC:CC:CC:CC
Entries can expire and be refreshed.
You can inspect ARP information using operating-system networking commands, such as arp or ip neigh on Linux systems.
18. Very Important: ARP Only Works Locally
Here's a common misunderstanding.
Suppose:
Your PC:
192.168.1.10

Internet server:
8.8.8.8
Your computer does not normally ARP for:
8.8.8.8
Why?
Because 8.8.8.8 isn't on your local subnet.
Instead, your computer needs the MAC address of the next-hop device, typically the default gateway.
For example:
Your PC
192.168.1.10
      │
      │ Ethernet frame
      │ Destination MAC = Router's MAC
      ↓
Router
192.168.1.1
      │
      ↓
Internet
      │
      ↓
8.8.8.8
This is a critical networking concept.
Your IP destination can remain the remote server while the Ethernet destination MAC is the next-hop device's MAC.
19. IP Destination vs MAC Destination ⭐⭐⭐
This can feel confusing at first.
Suppose:
PC:
IP = 192.168.1.10
MAC = AA:AA:AA:AA:AA:AA

Router:
IP = 192.168.1.1
MAC = RR:RR:RR:RR:RR:RR

Internet Server:
IP = 8.8.8.8
PC wants to communicate with:
8.8.8.8
The IP packet might contain:
Source IP:
192.168.1.10

Destination IP:
8.8.8.8
But the Ethernet frame on the local link might contain:
Source MAC:
AA:AA:AA:AA:AA:AA

Destination MAC:
RR:RR:RR:RR:RR:RR
Notice:
IP destination → 8.8.8.8
MAC destination → Router
Why?
Because the router is the next hop.
When the router forwards the packet onto another link, it creates a new link-layer frame appropriate for that next link.
This is one of the foundations of understanding routing.
20. ARP and the Default Gateway
Suppose:
PC:
192.168.1.10/24

Gateway:
192.168.1.1
PC wants to access:
10.10.10.10
First, it determines:
Is 10.10.10.10 in my local subnet?
No.
So:
Destination is remote
        ↓
Send to default gateway
        ↓
Need gateway's MAC
        ↓
Use ARP for 192.168.1.1
Then:
PC
 ↓
Ethernet frame
 ↓
Router
 ↓
Routing
 ↓
Next network
21. What Happens When Two Computers Communicate on the Same LAN?
Let's put everything together.
Suppose:
PC-A
IP: 192.168.1.10
MAC: AA:AA:AA:AA:AA:AA

PC-B
IP: 192.168.1.20
MAC: BB:BB:BB:BB:BB:BB
Both are:
192.168.1.0/24
Step 1
PC-A wants to communicate with:
192.168.1.20
Step 2
PC-A checks whether it's local.
Yes.
Step 3
PC-A checks its ARP cache.
Suppose it doesn't know the MAC.
Step 4
PC-A sends:
ARP Request:
"Who has 192.168.1.20?"
Step 5
PC-B responds:
192.168.1.20 →
BB:BB:BB:BB:BB:BB
Step 6
PC-A creates an Ethernet frame:
Destination MAC:
BB:BB:BB:BB:BB:BB

Source MAC:
AA:AA:AA:AA:AA:AA
Step 7
The switch receives the frame.
It checks its MAC address table.
Step 8
The switch forwards the frame toward PC-B.
Step 9
PC-B receives it and processes the IP packet.
The whole process is:
             ARP
              ↓
IP address → MAC address
              ↓
       Ethernet frame
              ↓
           Switch
              ↓
       Destination host
22. What Happens Through a Router?
Now let's make it slightly harder.
PC-A
192.168.1.10
     │
     ↓
 Switch
     │
     ↓
 Router
     │
     ↓
 Internet
     │
     ↓
Server
10.10.10.10
PC-A determines:
10.10.10.10
is not local.
Therefore:
PC-A
  ↓
Default gateway
  ↓
Router
PC-A needs the router's MAC address on the local link.
It uses ARP to discover it if necessary.
Then:
Ethernet Frame
    ↓
Router
The router removes the incoming link-layer framing, examines the IP packet, determines the next hop, and sends the packet onward using a new link-layer frame on the next link.
This is why:
MAC addresses are local-hop addressing, while IP addresses provide end-to-end network-layer addressing.
That's a simplified but extremely useful mental model.
23. ARP Security ⭐⭐⭐
Now we enter cybersecurity.
ARP does not have strong built-in authentication.
An attacker on a local IPv4 LAN can potentially send false ARP information.
This can lead to:
ARP spoofing / ARP poisoning
Conceptually:
Victim
   │
   │ "Who is the gateway?"
   ↓
Attacker
   ↑
   │ false ARP information
   │
Victim believes attacker is the gateway
This can potentially allow traffic interception or disruption, depending on the network and other security controls.
You don't need to learn attack procedures yet.
For now, understand:
ARP is trusted local address-resolution information, and that trust can create security risks.
24. Why ARP Matters to Cybersecurity
Security analysts may investigate:
Unexpected ARP traffic
Duplicate IP/MAC mappings
Suspicious ARP replies
Changes in gateway MAC addresses
Local network interception attempts
Security mechanisms can include:
Dynamic ARP Inspection
DHCP snooping
Static/controlled ARP entries in appropriate environments
Network segmentation
Monitoring
We'll study these later.
25. MAC Spoofing
Another security concept is MAC spoofing.
A device can sometimes configure its network interface to use a different MAC address.
For example, instead of:
AA:AA:AA:AA:AA:AA
it may present:
CC:CC:CC:CC:CC:CC
This can have legitimate purposes, such as testing or privacy, but it can also be abused.
Important:
A MAC address should not be treated as a strong authentication mechanism.
26. Broadcast Domain
A broadcast domain is the set of devices that can receive a Layer-2 broadcast within a given network segment.
For example:
       Switch
     /   |   \
    PC  PC   PC
A broadcast sent by one host can reach the other hosts in that broadcast domain.
Routers normally separate broadcast domains.
This becomes important when studying:
VLANs
Network segmentation
ARP
Broadcast traffic
Network design
27. VLAN — Introduction ⭐
You don't need to master VLANs yet, but you should know the term.
VLAN = Virtual Local Area Network
A VLAN allows a physical switching infrastructure to be divided into separate logical Layer-2 networks.
For example:
             Switch
          /    |     \
         /     |      \
       VLAN10 VLAN20 VLAN30
       Users  Servers Guests
This can help with:
Segmentation
Security
Organization
Broadcast-domain separation
We'll have a dedicated chapter for VLANs later.
28. Ethernet vs IP
Keep these concepts separate:
Ethernet
   ↓
Local/link-layer communication
   ↓
MAC addresses
   ↓
Frames
versus:
IP
   ↓
Network-layer communication
   ↓
IP addresses
   ↓
Packets
Together:
Ethernet Frame
┌─────────────────────────────┐
│ MAC addresses               │
│                             │
│   IP Packet                 │
│   ┌─────────────────────┐   │
│   │ IP addresses        │   │
│   │                     │   │
│   │ Data                │   │
│   └─────────────────────┘   │
└─────────────────────────────┘
29. The Full Local Communication Process ⭐⭐⭐
Let's memorize this flow.
Application
     ↓
TCP/UDP
     ↓
IP
     ↓
"Is destination local?"
     ↓
   ┌───────┴────────┐
   │                │
  YES               NO
   │                │
   ↓                ↓
ARP for          ARP for
destination      gateway
   │                │
   └───────┬────────┘
           ↓
    Ethernet frame
           ↓
         Switch
           ↓
     Next destination
This is a very useful mental model.
30. Cybersecurity View of Chapter 3 🛡️
When you eventually inspect traffic in Wireshark, you'll see things such as:
Ethernet II
    Source:      AA:AA:AA:AA:AA:AA
    Destination: BB:BB:BB:BB:BB:BB

Internet Protocol
    Source:      192.168.1.10
    Destination: 192.168.1.20

Transmission Control Protocol
    Source Port: ...
    Destination Port: ...
Now you can understand the hierarchy:
Ethernet
  └── MAC addresses
       ↓
IP
  └── IP addresses
       ↓
TCP/UDP
  └── Ports
       ↓
Application
  └── HTTP/DNS/SSH/etc.
This is exactly the kind of layered thinking you'll need for network security.
