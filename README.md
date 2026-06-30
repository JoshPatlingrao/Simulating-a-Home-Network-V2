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
- ACL (ToDo Tomorrow)
  - For the printer only family devices and network can access, not guests
  - Guest Network can't reach Parent, Son and Daughter
  - Work devices can't be reached by family devices but other devices within their networks can
    - I.E. Other devices can't reach Son Work Laptop, but Son PC can reach it
- Voice VLAN (ToDo)
- WiFi Security (ToDo)
- VPN for WFH (ToDo)
- NAT (ToDo)
- QoS (ToDo)
- SSH Management (ToDo)
- Port Security (ToDo)
- BPDU Guard (ToDo)
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
Why were these addresses chosen?
Class of address?
<p align="center">
  <img alt="Static IPv4" src="https://github.com/user-attachments/assets/2c18492d-ca0b-482d-a085-2a1aee713762" />
</p>

__Step 1: Configured Device Addresses and Gateway__

All devices will have their gateway address set to 198.132.221.254, for interface G0/0 of the router. The 
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


### Disable Unused Ports
<p align="center">
  <img alt="Disable Unused Ports" src="https://github.com/user-attachments/assets/faeed67f-b1c0-42b1-a30f-20fb82601961" />
</p>

Unused ports on routers and switches have been disabled to enhance security. This prevents the network devices from being used as either an attack vector or as a source of disruption from unwanted hosts.


### DHCP Snooping
<p align="center">
  <img alt="DHCP Snooping" src="https://github.com/user-attachments/assets/4b98cb61-820c-4c91-a1f3-34b4454f77e0" />
</p>


### Dynamic ARP Inspection
<p align="center">
  <img alt="Dynamic ARP Inspection" src="https://github.com/user-attachments/assets/dba85ec7-a088-4cb9-b040-8d5fb92f9f11" />
</p>
