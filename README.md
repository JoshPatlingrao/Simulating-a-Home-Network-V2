# Simulating-a-Home-Network-V2

## Objective
This project is a rebuild of the previous Home Network Simulation and to display my deeper network knowledge.

### Project Notes
### Skills Learned
### Tools Used

## Steps

### Configure Names for Switches
<p align="center">
  <img alt="SW Name" src="https://github.com/user-attachments/assets/b3b66742-df86-4647-ac9e-0c877b980006">
</p>

### Securing the Switches
<p align="center">
  <img alt="Security" src="https://github.com/user-attachments/assets/cdf320de-1595-4edf-9726-64f7c888b95f" />
</p>

__Step 1: Create Admin Account with Privileges__
- rtest

__Step 2: Require Local Database for Logins__
- rtest

__Step 3: Secure Privileged Mode__
- rtest

### Configure Router Gateway Interface
<p align="center">
  <img alt="Router Configuration" src="https://github.com/user-attachments/assets/b9f96d74-fcf0-4126-b095-05e94850c37e" />
</p>

### Configure Static IPv4 and Gateway Address on PC and Printer
Why were these addresses chosen?
Class of address?
<p align="center">
  <img alt="Static IPv4" src="https://github.com/user-attachments/assets/2c18492d-ca0b-482d-a085-2a1aee713762" />
</p>

__Step 1: Configured Device Addresses and Gateway__

All devices will have their gateway address set to 198.132.221.254, for interface G0/0 of the router.
- Dad Work Station: 198.132.221.1
- Son PC: 198.132.221.2
- Daughter PC: 198.132.221.3
- Family Printer: 198.132.221.4

__Step 2: Confirm Connectivity__

To confirm that addresses have been configured correctly and have connectivity, all the configured devices should respond to ping. 
<p align="center">
  <img alt="Ping" src="https://github.com/user-attachments/assets/52a90d57-21c6-4124-84fc-f0b2e7e7733f" />
</p>
