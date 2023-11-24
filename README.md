# RMIT-cybersecurity-implement-network-security
RMIT Cert 4 Cybersecuirty Implement network security infrastructure for an organisation

# Summary and Purpose of Assessment

This assessment is designed to allow the student to demonstrate their skills and knowledge in the design and implementation of a security perimeter for an organisation. This requires the student to demonstrate the following:
- Configure secure administrative access to network devices
- Plan and design firewall solution
- Implement firewall technologies
- Investigate new firewall technologies
- Configure perimeter to the secure network
- Implement intrusion prevention systems (IPS)
- Plan, design and configure network devices to provide secure fallover and redundancy
- Demonstrate the fundamental operations of Cryptographic systems
- Define and demonstrate the fundamentals of Virtual Private Networks (VPNs)
- Plan, design and configure a VPN solution
- Test and verify design performance

# Assessment Instructions

Students are required to complete a range of tasks to review existing network topologies and security solutions, to identify the range of security risks to the organisation, and to design a secure network perimeter for the protection of the client data.

# What

The following tasks must be completed in this assessment:
You have recently been employed as a Cyber Security Consultant for IT Assurance Services. IT Assurance Services specialises in the provision of ICT services to a range of small and medium enterprises, including the conduct of cyber security vulnerability assessments and the subsequent design and implementation of risk mitigation solutions to secure client systems.

Your employer has asked you to review the existing network infrastructure for their client Jojo Pty Ltd and to design and implement an effective security perimeter. As part of this you are required to document all stages of the network and security perimeter design to explain the purpose and functionality of each aspect.

The following topology is currently implemented:

![image](https://github.com/Jasmine-108/RMIT-cybersecurity-implement-network-security/assets/151819725/52f3398f-c504-49d4-b8bd-9dd7e3e10d8c)

|                           Adressing Table                       |
|                               :---:                             |

| Device | Interface    | IP Address      | Subnet Mask     | Default Gateway |
|  :---: | :---:        | :---:           |    :---:        |      :---:      |
| R1     | G0/0         | 209.165.200.233 | 255.255.255.248 | N/A             |
|        | S0/0/0 (DCE) | 12.12.12.1      | 255.255.255.252 | N/A             |
|        | Loopback 1   | 192.168.20.1    | 255.255.255.0   | N/A             |
| R2     | S0/0/0       | 12.12.12.2      | 255.255.255.252 | N/A             |
|        | S0/0/1 (DCE) | 23.23.23.2      | 255.255.255.252 | N/A             |
| R3     | G0/1         | 192.168.30.1    | 255.255.255.0   | N/A             | 
|        | S0/0/1       | 23.23.23.1      | 255.255.255.252 | N/A             |
| S1     | VLAN 1       | 192.168.10.11   | 255.255.255.0   | 192.168.10.1    |
| S2     | VLAN 1       | 192.168.10.12   | 255.255.255.0   | 192.168.10.1    |
| S3     | VLAN 1       | 192.168.30.11   | 255.255.255.0   | 192.168.30.1    |
| ASA    | VLAN 1 (E0/1)| 192.168.10.1    | 255.255.255.0   | N/A             |
|        | VLAN 2 (E0/0)| 209.165.200.234 | 255.255.255.247 | N/A             |
| PC-A   | NIC          | 192.168.10.2    | 255.255.255.0   | 192.168.10.1    |
| PC-B   | NIC          | 192.168.10.3    | 255.255.255.0   | 192.168.10.1    | 
| PC-C   | NIC          | 192.168.30.3    | 255.255.255.0   | 192.168.30.1    |


# Task

Using the information in the case study, complete the following steps to design and implement a security perimeter for Jojo Pty Ltd. For all tasks where you are required to configure network devices and services, you must include the configuration scripts and commands that you have used within the Documented System Design. Where possible screenshots should also be taken to confirm that devices and services have been configured and operate correctly.

# Router commands

===R1===

```
int g0/0
ip ospf 1 area 0
int s0/0/0
ip ospf 1 area 0
router ospf 1
router-id 1.1.1.1
passive-interface g0/0
int Loopback1
192.168.20.1 255.255.255.0
ip ospf 1 area 0
do wr
do sh ip int brief
```
====Site-to-site VPN====

```
conf t
access-list 101 permit ip 192.168.20.1 0.0.0.255 192.168.30.0 0.0.0.255
crypto isakmp policy 10
encryption aes 256
authentication pre-share
hash sha
group 5
lifetime 3600
exit
crypto isakmp key ciscovpnpa55 address 23.23.23.1
crypto ipsec transform-set VPN-SET esp-aes 256 esp-sha-hmac
crypto map CMAP 10 ipsec-isakmp
set peer 23.23.23.1
set pfs group5
set transform-set VPN-SET
match address 101
exit
interface S0/0/0
crypto map CMAP
end
```

===R2===

```
int s0/0/0
ip ospf 1 area 0
int s0/0/1
ip ospf 1 area 0
router ospf 1
router-id 2.2.2.2
do wr
do sh ip int brief
```
===R3===

```
hostname R3
int g0/1
ip ospf 1 area 0
int s0/0/1
ip ospf 1 area 0
router ospf 1
router-id 3.3.3.3
passive-interface g0/1
do wr
do sh ip int brief
```

====Firewall configs====

```
zone security IN-ZONE
zone security OUT-ZONE
access-list 110 permit ip 172.30.3.0 0.0.0.255 any
access-list 110 deny ip any any
class-map type inspect match-all INTERNAL-CLASS-MAP
match access-group 110
exit
policy-map type inspect IN-2-OUT-PMAP
class type inspect INTERNAL-CLASS-MAP
inspect
zone-pair security IN-2-OUT-ZPAIR source IN-ZONE destination OUT-ZONE
service-policy type inspect IN-2-OUT-PMAP
exit
interface g0/1
zone-member security IN-ZONE
exit
interface s0/0/1
zone-member security OUT-ZONE
end
```

====IPS configs===

```
mkdir ipsdir

conf t
ip ips config location flash:ipsdir
ip ips name IPS-RULE
ip ips signature-category
category all
retired true
exit
category ios_ips basic
retired false
exit
exit

interface s0/0/1
ip ips IPS-RULE in
exit

ip ips signature-definition
signature 2004 0
status
retired false
enabled true
exit
engine
event-action produce-alert
event-action deny-packet-inline
exit
exit
exit
```

====Site-to-site VPN====

```
conf t
access-list 101 permit ip 192.168.30.0 0.0.0.255 192.168.20.1 0.0.0.255
crypto isakmp policy 10
encryption aes 256
authentication pre-share
hash sha
group 5
lifetime 3600
exit
crypto isakmp key ciscovpnpa55 address 12.12.12.1
crypto ipsec transform-set VPN-SET esp-aes 256 esp-sha-hmac
crypto map CMAP 10 ipsec-isakmp
set peer 12.12.12.1
set pfs group5
set transform-set VPN-SET
match address 101
exit
interface S0/0/1
crypto map CMAP
end
```
===S1===

```
hostname S1
int fa0/1
switchport mode trunk
switchport trunk native vlan 1
switchport nonegotiate
int vlan1
ip address 192.168.10.11 255.255.255.0
no shut
ip default-gateway 192.168.10.1
end
```

===S2===

```
hostname S2
int fa0/1
switchport mode trunk
switchport trunk native vlan 1
switchport nonegotiate
int vlan1
ip address 192.168.10.12 255.255.255.0
no shut
ip default-gateway 192.168.10.1
end
```

===S3===

```
hostname S3
int vlan1
ip address 192.168.30.11 255.255.255.0
no shut
ip default-gateway 192.168.30.1
end
```

Ping between the routers and ping between Loopback 1 and PC-C should be successful. Take a screenshot to confirm that this is successful.

![image](https://github.com/Jasmine-108/RMIT-cybersecurity-implement-network-security/assets/151819725/52a19c71-ef77-4230-adea-b68c01f2784a)

![image](https://github.com/Jasmine-108/RMIT-cybersecurity-implement-network-security/assets/151819725/45332b5d-b137-4923-8f82-806c82dd3e27)


Ping the Lo1 interface on R1 from PC-C. On R3, use the show crypto ipsec sa command to verify that the number of packets is more than 0, 
which indicates that the IPsec VPN tunnel is working. Take screen shot

![image](https://github.com/Jasmine-108/RMIT-cybersecurity-implement-network-security/assets/151819725/1d4d427c-64e1-4a69-b3f1-4d31cdaf3081)


Test a Brute Force Attack while trying to login through Telnet on the perimeter router

![image](https://github.com/Jasmine-108/RMIT-cybersecurity-implement-network-security/assets/151819725/f2936529-49b3-4148-9fe7-e8fba672117a)

Complete a Denial of Service (DOS) attack (ping-t) to test IPS on the perimeter router

![image](https://github.com/Jasmine-108/RMIT-cybersecurity-implement-network-security/assets/151819725/5af7fe9f-1425-4ebf-b4f7-2e39865afe4c)

Test that traffic is encrypted that travels across the VPN between R1 and R3

![image](https://github.com/Jasmine-108/RMIT-cybersecurity-implement-network-security/assets/151819725/71f2110e-4951-4ff5-ac68-c288e71765ef)
