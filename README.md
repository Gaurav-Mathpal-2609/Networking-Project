# Networking-Project
A basic multibranch office network simulator in Cisco Packet Tracer.

This project is a basic design for a very small office like environment that shows how multiple department establish connections and how everything is managed by the HQ.

## Network design: 
<pre><code>HQ: Contains 3 servers: 
                -DNS (192.168.1.3) 
                -DHCP(192.168.1.2) 
                -Web Server(192.168.1.4) 
                -Contains some PCs.
                
    The network for the HQ is 192.168.1.0/24
    
Branches: -HR, Sales and IT department.
          -Contains 5 PCs in each branch. 
          -Have been assigned different VLANs.
</code></pre>
## Concept used:
1. The DHCP server assigns to IP to each branch based on the VLAN. 

2. Each branch has been assigned a VLAN. Sales(VLAN 10), HR(VLAN 20), IT(VLAN 30). 

3. Static routing has been used.

4. The information regarding the IP assignment is as follows:
<pre>
     ip dhcp pool VLAN10
       network 192.168.2.0 255.255.255.192
       default-router 192.168.2.1
       dns-server 192.168.1.3
   
     ip dhcp pool VLAN20
       network 192.168.2.64 255.255.255.192
       default-router 192.168.2.65
       dns-server 192.168.1.3
   
     ip dhcp pool VLAN30
       network 192.168.2.128 255.255.255.192
       default-router 192.168.2.129
       dns-server 192.168.1.3 </pre>

6. The information regarding the VLAN assignment is as follows:
<pre>   
VLAN Name                             Status    Ports
---- -------------------------------- --------- -------------------------------
1    default                          active    Fa0/16, Fa0/17, Fa0/18, Fa0/19
                                                Fa0/20, Fa0/21, Fa0/22, Fa0/23
                                                Fa0/24, Gig0/2
10   sales                            active    Fa0/1, Fa0/2, Fa0/3, Fa0/4
                                                Fa0/5
20   hr                               active    Fa0/6, Fa0/7, Fa0/8, Fa0/9
                                                Fa0/10
30   IT                               active    Fa0/11, Fa0/12, Fa0/13, Fa0/14
                                                Fa0/15
1002 fddi-default                     active    
1003 token-ring-default               active    
1004 fddinet-default                  active    
1005 trnet-default                    active    
</pre>

6. The routing information is as follows:
<pre>
interface GigabitEthernet0/0/0
 ip address 192.168.1.1 255.255.255.0
 duplex auto
 speed auto
!
interface GigabitEthernet0/0/1
 no ip address
 duplex auto
 speed auto
!
interface GigabitEthernet0/0/1.10
 encapsulation dot1Q 10
 ip address 192.168.2.1 255.255.255.192
 ip helper-address 192.168.2.1
!
interface GigabitEthernet0/0/1.20
 encapsulation dot1Q 20
 ip address 192.168.2.65 255.255.255.192
 ip helper-address 192.168.2.65
!
interface GigabitEthernet0/0/1.30
 encapsulation dot1Q 30
 ip address 192.168.2.129 255.255.255.192
 ip helper-address 192.168.2.129
!
interface GigabitEthernet0/0/2
 no ip address
 duplex auto
 speed auto
 shutdown
</pre>

7. The main highlights from the Switch are as follows: 
<pre>
interface FastEthernet0/1
 switchport access vlan 10
 switchport mode access

interface FastEthernet0/6
 switchport access vlan 20
 switchport mode access

interface FastEthernet0/11
 switchport access vlan 30
 switchport mode access

interface GigabitEthernet0/1
 switchport mode trunk
</pre>
## Testing and Validation
1. Verified IP address assignment via DHCP for all PCs.

2. DNS resolution confirmed using the centralized DNS server.

3. Successful inter-VLAN routing verified using static routes.

4. Connectivity across all departments and HQ tested using ICMP (ping).

## Author
Gaurav Mathpal

B.Tech in Computer Science Engineering
