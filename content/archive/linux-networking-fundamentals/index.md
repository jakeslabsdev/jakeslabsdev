+++
title = "Linux Networking Fundamentals"
date = 2026-08-21

[taxonomies]
categories = ["Networking"] 
tags = ["Linux", "Networking", "bash"]
+++

# Networking with GNU/Linux

My first area of research has to be in networking, and thankfully Linux is a perfect operating system to use to explore this as it's completely transparent meaning the whole network stack is now open to you.

I'm running openSUSE Tumbleweed and I use `bash` which is the most common shell in the Linux ecosystem, meaning that the most of the utilities I use in the article should be available to you being prepackaged with every Linux distribution I've come across so far.

# Absolute Basics of Networking

- The internet is made of up billions of devices, and to communicate with each other they need systems of predefined laws and protocols to be able to understand each other and exchange information.
- Devices have network interfaces; Chips on the motherboard which let you connect to another network or device. They are Identified with **MAC address (Media Access Control)** ex: **6a:74:75:5e:57:82**.
- When an interface connects to a network, it's given it's own place in the network which is the **IP address (Internet Protocol)**.
- Could be **ipv4 which is simpler**, or could be the **newer ipv6 with an astronomical amount of possible addresses**, and they can either be **permanent or ephemeral** depending on what the computer is used for (if it's a server etc...).
- Using these addresses, **routers control the flow of information** between computers in a local network, and also to the greater internet through numerous protocols and technologies **across a dynamic web of interconnected systems.**

# Networking on your System

## `ip`; The Kernel's Core Networking Suite

Use this utility to control **interfaces, IP addresses, routing tables & tunnels at the kernel level**.

- `ip a` lists interfaces and **basic network information**.
- `ip link set` to bring an interface **up or down**.
- `ip route` to view default gateways and **routing tables**.
- Find more in the **manual page** with the `man` utility.
**Scope**: Works on any network device.

## `iw`; Wireless Specific Tool

Controls **Wi-Fi specific radio layers & configuration using the kernel interface.**

Use this utility to **communicate directly with a wireless NIC**'s (Network Interface Card) **physical layer or Wi-Fi config**.

- `iw dev ... scan` to scan for **available access points**.
- `iw dev ... link`  to check **signal strength, link speed and associated BSSID**.
- `iw dev ... set type monitor` to set a specific **mode**, in this case monitor.
**Scope**: Wi-Fi only.

## `nmcli`; NetworkManager's Command Line Interface

Controls **high level connection profiles**, switching **Wi-Fi networks**, automatic **DHCP vs static IP configs**, and **credential storage**.

When to use it: When managing how your system connects day to day.
- Connecting to a *new Wi-Fi network or entering a password*
- **Setting static IP** address or **changing DNS servers**
-**Listing saved connection** profiles `nmcli connection show`

**Scope**: High level automation, it uses ip and other tools to communicate with the kernel.

## How to Set a MAC Address

(Replace the ... with your interface name)
- *Step 1*: Find interface name with `ip a`
- *Step 2:* Bring interface down with `sudo ip link set dev ... down`
- *Step 3:* Set new address `sudo ip link set dev ... address ...`
- *Step 4:* Bring interface back up `sudo ip link set dev ... up`
- *Step 5:* Verify the change `ip link show ...`

Also, you could use the macchanger utility to automatically change an interface to a random MAC address with `sudo macchanger -r ...`

## DHCP; Dynamic Host Configuration Protocol

DHCP is a technology which lets servers automatically assign available ip addresses so people don't have to do it themselves.

The easiest way to get a new DHCP allotted ip address right from the DHCP server is this simple method:

- *Step 1:* Take your interface down and give it a new MAC address.
- *Step 2:* Turn the interface back on again.

With a new MAC address, the DHCP server will think it's an entirely new device and assign it a new ip address.

## Setting a Static IP Address

In some cases where you don't want your ip address to change, such as hosting a server, you can set a static ip address to an interface like this:

`sudo ip address add [new_address] dev [interface_name]`

## DNS; Domain Name System

As I wrote earlier in the article, the internet does rely on an interconnected web of devices sending information to each other's IP addresses, but they're hard to remember so we use a technology called DNS so that we can access other computers, networks and services with a human readable name like `jakeslabs.dev`.

A DNS server is a computer working to match domain names with their IP addresses using a large registry, providing a layer of abstraction so you can just pass it domain names and not worry about the IP address.

Many routers function as their own DNS server but you can set up your own or use google's `8.8.8.8` or cloudflare's `1.1.1.1`. The configuration file responsible for the priority list of your systems DNS servers is `/etc/resolv.conf` but in modern distributions the file is actually managed by NetworkManager, which means you should use `nmcli` to make changes as if you manually change the file NetworkManager will overwrite it.

You can also make a small DNS rerouter on your system with /etc/hosts, this file maps custom domain names and you can reroute domains to point to other servers instead of what an external DNS server would. By default it just contains the localhost, which is how your system identifies and exchanges information with itself.

# Final Thoughts

There's much more I'm looking forward to learn on this subject, but to go further would be to leave the scope of this article.

I hope this serves as a brief and simple introduction to networking with Linux!
