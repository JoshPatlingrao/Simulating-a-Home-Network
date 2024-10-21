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

While the password has now been encrypted with the ;7; indicating the type of encryption used. It can still be cracked by attackers. To provide another layer of security we must configure a more secure 'secret' password which has stronger MD5 encryption.
- Go into Global Configuration mode.
- Run the 'enable secret MySWUser' command to set the 'secret' password.
- Type 'exit' twice to go back to the User EXEC mode and try to enter Privileged EXEC mode to test the 'secret' password.
- When in Privileged EXEC mode and run 'sh(ow) run(ning-config)' command again to confirm that the password and secret password are both encrypted. Both should be a ciphertext.

The switch should now be using the secret password whenever Privileged EXEC mode is being accessed. This also has the '5' when running 'sh(ow) run(ning-config)' command, indicating it's using the stronger MD5 hashing algorithm. The last step is to save the running configuration to the startup configuration.
- Go into Privileged EXEC mode and run the 'write' command.
- To confirm it's saved, run 'sh(ow) start(up-config)'. It should match the 'sh(ow) run(ning-config)' output.

This section will be replicated across all switches. Setting the 'password' step can be skipped and go straight to the 'secret' as it provides stronger hashing algorithm by default.

### Configuring IP Addresses

### Configure a Router
- Do the same steps for 'Setting Hostname for Switch' to set the hostname of the router.
- Go to Privileged EXEC mode and type 'show ip interface brief' to view the current setup of the interfaces.
- Run the 'int(erface) giga(bitethernet) 0/0' to select the 0 port, then 'ip ad(dress) 198.132.075.1 255.255.255.0' to set the IP address for the port and the network mask.
- Run 'desc(ription) ## To Wireless Router ##' to indicate where the port is connected.
- Run 'no shutdown' to enable the interface.
- Switch to GigaBit Interface 0/1 using 'int(erface) g0/1' and repeat the steps until G0/2.

## Summary
