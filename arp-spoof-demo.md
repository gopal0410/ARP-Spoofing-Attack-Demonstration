#  Demonstration of ARP Spoofing Attack in a Virtualised Network Environment

##  Description
This project demonstrates an **ARP Spoofing** (ARP Poisoning) attack in a controlled virtual lab to highlight security weaknesses in the ARP. Since ARP does not authenticate responses, it can be exploited to perform **Man-in-the-Middle** (MITM) attacks.


## Experimental Setup
The experiment is performed using two virtual machines on the same virtual network:
- Attacker Machine: Parrot OS (username: gopal)
- Victim Machine: Linux Mint (username: 6605901)

Both the virtual machines are connected to the same Layer-2 virtual network.
Virtual Network: Bridged


## Attack Overview
In this project, we are deceiving the victim machine to associate attacker's MAC address with the default gateway's IP address. And the gateway is deceived into associating the attacker's MAC address with the victim's IP address. This is the Man-in-the-Middle attack where we are able to redirect traffic.

For the scope of this project, we are only required to check from the victim machine how two IP addresses are mapped to the same MAC address when we initiate MITM attack.

## Checking the attacker machine's IP
First, we will begin by checking our own device's IP address.
We will achieve this by using the following command:

`ip addr`

![ip address](https://github.com/gopal0410/ARP-Spoofing-Attack-Demonstration/blob/master/images/attacker-machine-ss/ip-address.png?raw=true)

## Checking for devices and their IPs in the network
Now we will begin by checking the devices that are connected to the network and note their IP addresses. Then we will select the victim machine.
We will check available machines on the network using the following command:

`sudo nmap -sn 192.168.31.0/24`

Here, -sn -> Ping scan

![devices](https://github.com/gopal0410/ARP-Spoofing-Attack-Demonstration/blob/master/images/attacker-machine-ss/devices-ip.png?raw=true)

Here, our victim machine is **mint-Virtualbox**. We note its IP address for our ARP Spoofing.

## ARP Behaviour Observation
**Before Communication**

Now, we check the arp table using `arp -n` from our victim machine. This table shows all the MAC addresses and their respective IP addresses. Here, since we first started in our attacker machine and checked its IP address, it is already mapped. And `192.168.31.1` is the gateway IP address.

![arp table before communication](https://github.com/gopal0410/ARP-Spoofing-Attack-Demonstration/blob/master/images/victim-machine-ss/arp-scan1.png?raw=true)

**After ICMP Communication**

Now, in our victim machine we ping devices and then note the arp table. We will ping every/any device in the network using the following command:

`sudo nmap -sn 192.168.31.0/24`

For now, I am pinging every device in the network

![pinging every device](https://github.com/gopal0410/ARP-Spoofing-Attack-Demonstration/blob/master/images/victim-machine-ss/ping%20comm.png?raw=true)

Finally, the arp table is observed.

![arp table after icmp communication](https://github.com/gopal0410/ARP-Spoofing-Attack-Demonstration/blob/master/images/victim-machine-ss/arp-scan2.png?raw=true)

## Attack Launch from attacker machine
Now that we know our own IP address and the victim machine's IP address, we can begin our attack.
We will first sniff the traffic on the **enp0s3** interface by using `sudo ettercap -T -i enp0s3 -M arp:remote /192.168.31.201// /192.168.31.1//`

Now, we will see some traffic. For now, the victim machine opens an http page and the results are visible on the attacker machine as shown below. https pages are highly encrypted and cannot be read normally.

![mitm sniff](https://github.com/gopal0410/ARP-Spoofing-Attack-Demonstration/blob/master/images/attacker-machine-ss/MITM-sniff.png?raw=true)

## ARP Behaviour Observation: After Attacker initiates the MITM attack
We again run `arp -n` in our victim machine and observe the ARP table.
It is observed that two different IP addresses has the same MAC address. Hence, we know the victim will know that their packets are being intercepted.

![arp table after mitm atk](https://github.com/gopal0410/ARP-Spoofing-Attack-Demonstration/blob/master/images/victim-machine-ss/arp-scan3.png?raw=true)

## Conclusion
This project successfully demonstrated an ARP Spoofing (ARP Poisoning) attack in a controlled virtualised network environment. The experiment showed that due to the absence of authentication in the Address Resolution Protocol (ARP), an attacker can manipulate ARP tables and mislead network devices.

By observing the victim machine’s ARP table, it was confirmed that multiple IP addresses were mapped to the same MAC address after initiating the attack, indicating a successful Man-in-the-Middle (MITM) condition. This project highlights a fundamental weakness in ARP and emphasizes the need for secure network configurations to prevent such attacks.
