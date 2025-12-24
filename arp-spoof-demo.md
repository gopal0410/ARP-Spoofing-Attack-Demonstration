#   Project Title
##  Demonstration of ARP Spoofing Attack in a Virtualised Network Environment

##  Description
This project demonstrates an **ARP Spoofing** (ARP Poisoning) attack in a controlled virtual lab to highlight security weaknesses in the ARP. Since ARP does not authenticate responses, it can be exploited to perform **Man-in-the-Middle** (MITM) attacks.
***

## Experimental Setup
The experiment is performed using two virtual machines on the same virtual network:
- Attacker Machine: Parrot OS (username: gopal)
- Victim Machine: Linux Mint (username: 6605901)

Both the virtual machines are connected to the same Layer-2 virtual network.
Virtual Network: Bridged
***

## Attack Overview
In this project, we are deceiving the victim machine to associate attacker's MAC address with the default gateway's IP address. And the gateway is deceived into associating the attacker's MAC address with the victim's IP address. This is the Man-in-the-Middle attack where we are able to redirect traffic.

For the scope of this project, we are only required to check from the victim machine how two IP addresses are mapped to the same MAC address when we initiate MITM attack.
***

## Checking the attacker machine's IP
First, we will begin by checking our own device's IP address.
We will achieve this by using the following command:

`ip addr`

![]()
## Checking for devices and their IPs in the network
Now we will begin by checking the devices that are connected to the network and note their IP addresses. Then we will select the victim machine.
We will check available machines on the network using the following command:

`sudo nmap -sn -T5 -v 192.168.31.0/24`

![](https://github.com/gopal0410/ARP-Spoofing-Attack-Demonstration.git/images/network-scan.png)

Here, our victim machine is mint-Virtualbox. We note its IP address for our ARP Spoofing.
***

## ARP Behaviour Observation
**Before Communication**

Now, we check the arp table from our victim machine. This table shows all the MAC addresses and their respective IP addresses. Here, since we first started in our attacker machine and checked its IP address, it is already mapped. And >192.168.31.1 is the gateway IP address.

![]()

**After ICMP Communication**

Now, in our victim machine we ping devices and then note the arp table. We will ping every/any device in the network using the following command:

`sudo nmap -sn 192.168.31.0/24`

For now, I am pinging every device in the network

Finally, the arp table is observed.

![]()

***

## Attack Launch from attacker machine
Now that we know our own IP address and the victim machine's IP address, we can begin our attack.
We will first sniff the traffic on the **enp0s3** interface by using `sudo ettercap -T -i enp0s3 -M arp:remote /192.168.31.201// /192.168.31.1//`

Now, we will see some traffic. For now, the victim machine opens an http page and the results are visible on the attacker machine as shown below. https pages are highly encrypted and cannot be read normally.

![]()
***

## ARP Behaviour Observation: After Attacker initiates the MITM attack
We again run `arp -n` in our victim machine and observe the ARP table.
It is observed that two different IP addresses has the same MAC address. Hence, we know the victim will know that their packets are being intercepted.

![]()


