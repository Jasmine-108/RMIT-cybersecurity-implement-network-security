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
