# Simulating-a-Home-Network-V2

## Objective
This project is a rebuild of the previous Home Network Simulation and to display my deeper network knowledge.

### Project Notes
- Any passwords used in this project is merely for the demonstration of my knowledge and understanding of Cisco Packet Tracer and should not be practised on real machines.

### Skills Learned
- Securing routers and switches with both local and remote login.
- Subnetting to distribute IP addresses among network and host devices.
- Configuring VLANs to group devices based on network, using both access and trunk ports.
- Integration of wireless AP and configuration of SSID and its 2.4GHz channel
- Static IPv4 address and DHCP configuration.
- ACL
  - For the printer only family devices and network can access, not guests
  - Guest Network can't reach Parent network
- Voice VLAN
  - For Mom's Work
- WiFi Security
- VPN for WFH (To document)
- NAT
- QoS (ToDo)
- SSH Management (ToDo)
- Port Security (ToDo)
- BPDU Guard
- DHCP Snooping
  - Setup on Parent, Son and Daughter SW
- Dynamic ARP Inspect
  - Setup on Parent, Son and Daughter SW
- Disabling unused interfaces on routers and switches for enhanced security
- Banner?
- Syslog (ToDo)
- NTP (ToDo Tomorrow)

### Tools Used
- Cisco Packet Tracer

## Steps

### Configure Names for Switches
<p align="center">
  <img alt="SW Name" src="https://github.com/user-attachments/assets/b3b66742-df86-4647-ac9e-0c877b980006">
</p>

Each router and switch has been configured with a display and host name to clearly distinguish each network device from each other and their current roles.

### Securing the Switches
<p align="center">
  <img alt="Security" src="https://github.com/user-attachments/assets/cdf320de-1595-4edf-9726-64f7c888b95f" />
</p>

__Step 1: Create Admin Account with Privileges__
- Create and 'admin' user which is required to be logged in if the switches are to be configured.
- Apply 'privilege 15' to the account to give it the highest administrative level for network device configuration.
- Use 'secret' as it applies a cryptographic hash on the password and ensures it can't be read in the configuration file.

__Step 2: Require Local Database for Logins__
- rtest

__Step 3: Secure Privileged Mode__
- rtest

### Configure Router Gateway Interface
<p align="center">
  <img alt="Router Configuration" src="https://github.com/user-attachments/assets/b9f96d74-fcf0-4126-b095-05e94850c37e" />
</p>

### Subnetting and DHCP Pools

The family network will be split into 5 subnets. The largest will be the Family network as it's a shared subnet for all family members. All other subnets will have 30 usable addresses for their respective devices.
- Family: a shared subnet for family devices
- Guest: a subnet for guest devices to connect to
- Parents: a subnet for parents work devices
- Son: a subnet for son work devices
- Daughter: a subnet for daughter work devices

| VLAN  | Network | Subnet | Usable Hosts | Gateway (SVI) | DHCP Range |
| :---: | :---: | :---: | :---: | :---: | :---: |
| 10 | Family | 198.132.221.0/26 | 62 | 198.132.221.62 | .1 - 61 |
| 20 | Guest | 198.132.221.64/27 | 30 | 198.132.221.94 | .65 – .93 |
| 30 | Parent | 198.132.221.96/27 | 30 | 198.132.221.126 | .101 – .122 |
| 40 | Son | 198.132.221.128/27 | 30 | 198.132.221.158 | .133 – .154 |
| 50 | Daughter | 198.132.221.160/27 | 30 | 198.132.221.190 | .165 – .186 |


Pool1: Family Device Pool
Net: 198.132.221.0/26, Max: 62 Addresses
DNS: 9.9.9.9
Default Gateway: 198.132.221.254
Reserved: .1 - .10 (For infrastructure and static devices)

Pool2: Guest Device Pool
Net: 198.132.221.64/27, Max: 30 Addresses
DNS: 9.9.9.9
Default Gateway: 198.132.221.254

### Configure Static IPv4 and Gateway Address on PC and Printer

<p align="center">
  <img alt="Static IPv4" src="https://github.com/user-attachments/assets/2c18492d-ca0b-482d-a085-2a1aee713762" />
</p>

__Step 1: Configured Device Addresses and Gateway__

The address range is Class C range for small LAN usage. All devices will have their gateway address set to 198.132.221.254, for interface G0/0 of the router.
- Dad Work Station: 198.132.221.1
- Son PC: 198.132.221.2
- Daughter PC: 198.132.221.3
- Family Printer: 198.132.221.4

__Step 2: Confirm Connectivity__

To confirm that addresses have been configured correctly and have connectivity, all the configured devices should respond to ping. 
<p align="center">
  <img alt="Ping" src="https://github.com/user-attachments/assets/52a90d57-21c6-4124-84fc-f0b2e7e7733f" />
</p>

### Configure DHCP for Wireless Devices and Work Laptops

DHCP will be configured for wireless devices that will be connected on the family and guest network.

<p align="center">
  <img alt="DHCP Pools" src="https://github.com/user-attachments/assets/58b02ea2-2280-4f09-a84b-9f4c2e89da90" />
</p>

__Step 1: Create VLAN and SVI__
<p align="center">
  <img alt="VLAN" src="https://github.com/user-attachments/assets/e2f0be08-0bef-49ea-a87d-2d28dcbb54d0" />
</p>

<p align="center">
  <img alt="SVI" src="https://github.com/user-attachments/assets/adab7c15-af5b-4ed3-91c9-29e940c09a02" />
</p>

- Enable routing through 'ip routing' command
- Need to go to interface and use 'no switchport' command
- Setup IP address for L3 switch interface
- Setup default route on the L3 switch to point to gateway router

<p align="center">
  <img alt="Default Route" src="https://github.com/user-attachments/assets/de7b33ec-1f1d-41bf-92e8-59c92b75c234" />
</p>


### Implement ACLs

THe purpose of the ACL will be fulfill two simple purposes
- To prevent guest devices from accessing the Family Printer
- To prevent guest devices from reaching the parents, son and daughter subnets, which are dedicated for their private or work devices.
To implement photo


## WiFi Security
Family AP Pass: ourFamilyWifi

<p align="center">
  <img alt="Family AP" src="https://github.com/user-attachments/assets/54b43b19-9b51-4197-b2c6-55ef13dd1f91" />
</p>

Guest AP Pass: welcome123

<p align="center">
  <img alt="Guest AP" src="https://github.com/user-attachments/assets/2aa4c6be-6de2-45ee-bedf-42aa287a1f87" />
</p>

There are 2 separate WAPs for the home network. One reserved for the family which has full access to the home network and the other for guests which has limitations on network access.
- Both APs use the 2.4GHz channel. Since both APs are inside the house, Family AP is set to channel 1, while Guest AP is set to 6 to ensure they don't interfere with each other.
- Both APs use WPA2-PSK as the most secure form of authentication, and use AES for encryption.


### Implement Voice VLAN
<p align="center">
  <img alt="VoIP Diagram" src="https://github.com/user-attachments/assets/3d1526fd-0425-4500-aaaa-a64c3d44bbc1" />
</p>

<p align="center">
  <img alt="Adding VLAN 60" src="https://github.com/user-attachments/assets/6e6c594d-b3a8-4a31-ae16-c1ce018ecd10" />
  <img alt="Updated Trunk" src="https://github.com/user-attachments/assets/b6fd5071-cfd3-4fdf-aa95-6b28d2dac158" />
</p>

In the Parent's Subnet, the Mom needed to use VoIP for her work so a phone and an additional VLAN 60 was created to include the traffic of the phone to the subnet without needing to create another trunk port.

VLAN 60 was added on the trunk port as another allowed VLAN.

### DHCP Snooping
<p align="center">
  <img alt="DHCP Snooping" src="https://github.com/user-attachments/assets/4b98cb61-820c-4c91-a1f3-34b4454f77e0" />
</p>

Implemented DHCP snooping on Parent, Son and Daughter switch to prevent hostile DHCP servers from affecting the devices in the subnets.

### Dynamic ARP Inspection
<p align="center">
  <img alt="Dynamic ARP Inspection" src="https://github.com/user-attachments/assets/dba85ec7-a088-4cb9-b040-8d5fb92f9f11" />
</p>

Implemented DHCP snooping on Parent, Son and Daughter switch to prevent ARP Spoofing and poisoning from affecting the devices in the subnets.

### Disable Unused Ports
<p align="center">
  <img alt="Disable Unused Ports" src="https://github.com/user-attachments/assets/faeed67f-b1c0-42b1-a30f-20fb82601961" />
</p>

Unused ports on routers and switches have been disabled to enhance security. This prevents the network devices from being used as either an attack vector or as a source of disruption from unwanted hosts.

### NAT/PAT

__Step 1: Setup Inside and Outside Interfaces__
<p align="center">
  <img alt="In and Out" src="https://github.com/user-attachments/assets/ac7291c9-f78c-44c1-b202-9aae8cb0c59f" />
</p>


__Step 2: Define Network to be Translated__
<p align="center">
  <img alt="ACL" src="https://github.com/user-attachments/assets/477552bb-7ab7-4820-be70-5b6e999b68eb" />
</p>


__Step 3: PAT (Overload)__
<p align="center">
  <img alt="PAT Configuration" src="https://github.com/user-attachments/assets/14c4325d-cf53-493f-86dc-0b4c55721b36" />
</p>
ip nat inside source list 1 interface GigabitEthernet0/1 overload


__Step 4: Setup Routes__

<p align="center">
  <img alt="Default Route" src="https://github.com/user-attachments/assets/e7aeb199-33cb-4674-895d-4d2220e1a6c4" />
</p>

Default route is configured to point to 11.0.0.2

__Step 5: Verify__.
<p align="center">
  <img width="1227" height="916" alt="image" src="https://github.com/user-attachments/assets/4c179975-2266-4dc4-bd26-06ee8b1b632e" />
</p>
Dad Work Station, Son PC and Daughter PC can can reach the Internet Server

<p align="center">
  <img alt="PAT" src="https://github.com/user-attachments/assets/37930ecf-1d85-415a-a6a5-33d73690c983" />
</p>
This is PAT in action, where multiple devices within the home network can reach the Internet Server and are translated into the same external IP but through different ports.

### BPDU Guard

__Step 1: Enable Globally on All Switches__
<p align="center">
  <img alt="BPDU" src="https://github.com/user-attachments/assets/f5e9acdc-1d74-40b9-99ce-0df5628be4e9" />
</p>

__Step 2: Enable Access Ports__
<p align="center">
  <img alt="Port Enable" src="https://github.com/user-attachments/assets/ceaf2c99-51e8-44e7-8f5b-9e8ef3801122" />
</p>

__Step 3: Verify__


### NTP

__Step 1: Turn on NTP on Internet Server__
<p align="center">
  <img alt="NTPServer" src="https://github.com/user-attachments/assets/6de2ea46-0351-49ee-8b1d-7f36142f4ac1" />
</p>

__Step 2: Turn Edge Router Into an NTP Client__
<p align="center">
  <img alt="NTP Client" src="https://github.com/user-attachments/assets/7eaa0ba0-9f5e-41ec-9c39-bc92b860c819" />
</p>

__Step 3: Configure Edge Router as NTP Master for Home Network__
<p align="center">
  <img alt="NTP Auth" src="https://github.com/user-attachments/assets/682e75d3-0b96-4733-a951-2f4ae53dcec6" />
</p>
The key is 'HomeNet2026'

__Step 4: Configure Main Switch as NTP Client to Edge Router__
<p align="center">
  <img width="631" height="211" alt="image" src="https://github.com/user-attachments/assets/bfa10741-e73b-4cef-b7e8-e424ee28193c" />
</p>


### VPN (GRE Tunnels)
- No OSPF on outside routers


<p align="center">
  <img width="1314" height="881" alt="image" src="https://github.com/user-attachments/assets/c8d8da4f-06d0-4bb8-b5a3-cdb179614a77" />
</p>

Tunnel Plan
| Tunnel  | Connection | Edge Side | Company Side | Tunnel Destination |
| :---: | :---: | :---: | :---: | :---: |
| Tunnel1 | Mom > Company A | 172.16.1.1/30 | 172.16.1.2/30 | 14.0.0.2 |
| Tunnel2 | Son > Company B | 172.16.2.1/30 | 172.16.2.2/30 | 15.0.0.2 |
| Tunnel3 | Daughter > Company C | 172.16.3.1/30 | 172.16.3.2/30 | 16.0.0.2 |


__Step 1: Configure Tunnel on Home Side__
<p align="center">
  <img width="279" height="252" alt="image" src="https://github.com/user-attachments/assets/c5bc4b87-8c94-4e5d-8411-5c8befa9a083" />
</p>

__Step 2: Configure Tunnel on Company Side__

Company A
<p align="center">
  <img width="278" height="66" alt="image" src="https://github.com/user-attachments/assets/3641dbb6-5c20-4222-9e35-31fe56f9a20a" />
</p>

Company B
<p align="center">
  <img width="279" height="69" alt="image" src="https://github.com/user-attachments/assets/3ba3babe-9d2b-40a3-bff0-bb6bd08276e0" />
</p>

Company C
<p align="center">
  <img width="273" height="69" alt="image" src="https://github.com/user-attachments/assets/ebd46878-33ec-4e7d-9e28-c287c4ed3308" />
</p>

__Step 3: Confirm Connection__

Mom > Company A
<p align="center">
  <img width="328" height="54" alt="image" src="https://github.com/user-attachments/assets/82d68803-3eb9-493f-bb9c-3d911ea11ae3" />
</p>

Edge Router Tunnel Routes
<p align="center">
  <img width="439" height="275" alt="image" src="https://github.com/user-attachments/assets/7cba394a-4e3b-4f4e-9b59-adeaeb2eb492" />
</p>

Pinging Company Side Tunnel Interface
Company A
<p align="center">
  <img width="486" height="82" alt="image" src="https://github.com/user-attachments/assets/eb3f6d38-0888-417b-abc9-2984d32836b1" />
</p>
