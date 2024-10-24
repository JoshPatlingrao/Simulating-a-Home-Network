# Simulating-a-Home-Network

## Objective
The 
Any passwords used in this project is merely for the demonstration of my knowledge and understanding of Cisco Packet Tracer and should not be practised on real machines.
Words in brackets indicate the full name of the command that's otherwise been typed in shorthand form.

### Skills Learned

### Tools Used
- Cisco Packet Tracer

## Steps

### Setting Router Password

### Setting Hostname for Switch
Insert image here
- Click on the switch device and go to the CLI tab.
- Type 'en(able)' to go into Privileged EXEC mode, then type 'conf(igure) t(erminal)' to go into Global Configuration mode.
- Type 'hostname MySW' to change the name of the switch.

### Securing Switch with Password and Secret
To prevent anyone from directly accessing the Privileged EXEC mode, there must be a password to provide a layer of security. Allowing only the network administrator access to it.
- Click on the switch device and go to the CLI tab.
- Type 'en(able)' to go into Privileged EXEC mode, then type 'conf(igure) t(erminal)' to go into Global Configuration Mode.
- Type 'enable pass(word) MySwitchUser' to set the password of the switch.
- Type 'exit' twice to go back to the User EXEC mode and try to enter Privileged EXEC mode to test the new password.

Running the 'sh(ow) run(ning-config)' command will display the current settings of the switch and will also display the password in clear text. The password needs to be encrypted for further protection as attackers could easily search it up on the CLI.
- Go into Global Configuration mode.
- Run the 'service pass(word-encryption)' command.
- Type 'exit' to go back to Privileged EXEC mode and run 'sh(ow) run(ning-config)' command again to confirm that the password has now been encrypted. It should be a ciphertext.

While the password has now been encrypted with the '7' indicating the type of encryption used. It can still be cracked by attackers. To provide another layer of security we must configure a more secure 'secret' password which has stronger MD5 encryption.
- Go into Global Configuration mode.
- Run the 'enable secret MySWUser' command to set the 'secret' password.
- Type 'exit' twice to go back to the User EXEC mode and try to enter Privileged EXEC mode to test the 'secret' password.
- When in Privileged EXEC mode and run 'sh(ow) run(ning-config)' command again to confirm that the password and secret password are both encrypted. Both should be a ciphertext.

The switch should now be using the secret password whenever Privileged EXEC mode is being accessed. This also has the '5' when running 'sh(ow) run(ning-config)' command, indicating it's using the stronger MD5 hashing algorithm. The last step is to save the running configuration to the startup configuration.
- Go into Privileged EXEC mode and run the 'write' command.
- To confirm it's saved, run 'sh(ow) start(up-config)'. It should match the 'sh(ow) run(ning-config)' output.

This section will be replicated across all switches. Setting the 'password' step can be skipped and go straight to the 'secret' as it provides stronger hashing algorithm by default.

### Configuring IP Addresses for Host Devices
The device IP needs to be configured so it can be referred to using layer 3 address rather than resorting to MAC addresses
- Click on the end point device and go to the 'Config' tab.
- Select the 'FastEthernet0' tab and eneter the IPv4 address, keeping the network section the same and setting any number between 0 and 255 as the machine number.
  - The .0 is for the network address while the .255 is for the broadcast address.
- Set the default gateway to match the router port IP address it is connected to directly or through a switch.
  - This allows the ARP packet to be sent through the 'gateway' router to request for the MAC address that corresponds to the destination IP address to networks outside the LAN.
- To confirm that the address is working try to ping other end point devices from different subnets. If configured correctly, the pinged device should send an ICMP reply packet.

### Configure a Router
- Do the same steps for 'Setting Hostname for Switch' to set the hostname of the router.
- Go to Privileged EXEC mode and type 'show ip interface brief' to view the current setup of the interfaces.
- Run the 'int(erface) giga(bitethernet) 0/0' to select the 0 port, then 'ip ad(dress) 198.132.075.254 255.255.255.0' to set the IP address for the port and the network mask.
- Run 'desc(ription) ## To Wireless Router ##' to indicate where the port is connected.
- Run 'no shutdown' to enable the interface.
- Switch to GigaBit Interface 0/1 using 'int(erface) g0/1' and repeat the steps until G0/2.
- Run 'write' in Privileged EXEC mode to save configuration.

### Manually Configure Interfaces
Modern CISCO devices would automatically configure port settings based on the interface it's connected to, but there may be times where the port needs to be configured manually.
- Select the router and go into Global Configuration mode.
- Select port G0/0. In the case of this project, both MySwitch and Router are connected through their GigaBit ports.
- Run command 'speed 1000' top set the speed to 1000 megabits, equivalent to the Gigabit speed the ports are caopable of.
- Run command 'duplex full' since switches are capable of running half, rather than a hub which can only use half duplex.
  - This specific configuration will cauise the protocol status to be down due to the mismatch. The MySwitch G0/1 port doesn't automatically update and needs to be configured manually to match the full duplex of the G0/0 interface of the router.
- Run 'write' in Privileged EXEC mode to save configuration.

### Disable Unused Interfaces
It is good security practise to disable ports/interfaces that are not in use to prevent attackers from using them to their advantage.
- Go to MySW and go to CLI tab.
  - In its current state the only interfaces that are in use are F0/1, F0/2, F0/3 and G0/1.
- Go into Global Configuration mode and run command 'int(erface) range f0/4 - 24' to simultaneously access all fast ports that are not in use.
- Enter reason for being disabled, 'desc ## Not in use ##'
- Run 'shutdown' to disable the specified range of interfaces.
- Repeat these steps for G0/2 port.
- Run 'write' in Privileged EXEC mode to save configuration.

### Subnetting

198.132.221.0/24
Parent's Network has 5 host devices.
Will use a 198.132.221.0/29, giving 8 addresses and 32 subnets
network address: 198.132.221.0
broadcast address: 198.132.221.7
5x host devices: 198.132.221.1 - .5
router address: 198.132.221.6
subnet mask: 255.255.255.248

My Network has 3 host devices.
Will use a 198.132.221.8/29, giving 8 addresses and 32 subnets
network address: 198.132.221.8
broadcast address: 198.132.221.15
3x host devices: 198.132.221.9 - .11
router address: 198.132.221.14
subnet mask: 255.255.255.248

Sister's network has 1 host device.
Will use a 198.132.221.16/30, giving 4 addresses and 64 subnets
network address: 198.132.221.16
broadcast address: 198.132.221.19
1x host devices: 198.132.221.17
router address: 198.132.221.18
subnet mask: 255.255.255.252

## Summary
