---
description: IP Adresses settings
---

# IP Adresses & DNS

### IP addresses: Address for a device.



The primary difference between a private IP address and a public IP address lies in their scope and usage:

**Public IP Address:**

1. **Global Scope:** A public IP address is globally unique and is used to identify a device or network on the public internet. Every device on the internet must have a unique public IP address to enable communication between devices across the world.
2. **Internet-Facing:** Public IP addresses are used for communication between your local network and external networks, such as the internet. They are assigned by your Internet Service Provider (ISP) and are used to access websites, online services, and other resources on the internet.
3. **Directly Accessible:** Devices with public IP addresses can be accessed from anywhere on the internet, provided the necessary ports are open and there are no firewall restrictions.
4. **Managed by ISPs:** Public IP addresses are typically assigned and managed by ISPs. ISPs allocate these addresses from a limited pool of available addresses, and they may be dynamic (changing periodically) or static (remaining constant).

**Private IP Address:**

1. **Local Scope:** A private IP address is used within a local network, such as a home or office network. It is not globally unique and is only meant for communication within the local network.
2. **Not Routable on the Internet:** Private IP addresses are not directly accessible from the public internet. They are used for devices within your local network to communicate with each other and share a single public IP address for internet access.
3. **Common Private IP Ranges:** Private IP addresses are assigned from reserved address ranges defined by the Internet Assigned Numbers Authority (IANA). Common private IP address ranges include 192.168.0.0 to 192.168.255.255, 172.16.0.0 to 172.31.255.255, and 10.0.0.0 to 10.255.255.255.
4. **NAT Used:** Network Address Translation (NAT) is used by routers to map multiple private IP addresses to a single public IP address, allowing multiple devices in the local network to share the same public IP address when accessing the internet.

In summary, the key difference is that public IP addresses are used for global internet communication and are unique across the internet, while private IP addresses are used within a local network and are not directly routable on the internet. Private IP addresses are essential for conserving the limited pool of available public IP addresses and for enabling multiple devices to share a single public IP address using NAT.

Note: If you compare your IP address by looking at \`ipconfig\` vs whatismyipaddress.com , you will notice that they are different. Because the former is your private IP, whereas the latter is your public IP assigned by your ISP.

### So does every device have its own IP address? like a phone.

Yes, nearly every device that connects to a network, including a phone, is typically assigned its own unique IP address. These IP addresses are used to identify and communicate with each device on the network. Here's how it works:

1. **Local Network (LAN):** In a home or office network, each device, whether it's a phone, computer, tablet, printer, or any other networked device, is assigned a unique private IP address. These private IP addresses are used for communication within the local network and are not directly accessible from the public internet.
2. **Public Internet (WAN):** When a device from your local network needs to access resources on the public internet, your router uses Network Address Translation (NAT) to map all the private IP addresses in your local network to a single public IP address, which is provided by your Internet Service Provider (ISP). This allows all the devices in your local network to share the same public IP address for internet communication.
3. **Public Internet Access:** The public IP address assigned by your ISP is the one visible to the outside world. When a device in your local network communicates with a website or online service, it does so through your router, which translates the private IP address to the public IP address for the external communication. The external service only sees your public IP address, not the private IP addresses of your individual devices.

So, while every device on your local network has its own private IP address for local communication, they share a single public IP address for internet access. This is a way to conserve public IPv4 addresses, as there are far fewer public IPv4 addresses available than the number of devices connected to the internet. With IPv6, there is a vastly larger pool of addresses, which allows for more public IP addresses to be assigned directly to individual devices, reducing the need for NAT in many cases.

### Who assigns this device a private IP address?

Private IP addresses are assigned to devices within a local network by the network's router or gateway device. The process of assigning private IP addresses is typically automated, and it follows certain conventions and standards. Here's how it works:

1. **Dynamic Host Configuration Protocol (DHCP):** DHCP is a network protocol used to automatically assign IP addresses, as well as other network configuration parameters, to devices in a network. The router or gateway device in your local network acts as a DHCP server.
2. **DHCP Server:** When a device, such as a phone, connects to the local network, it sends a DHCP request to the router. The router, which functions as the DHCP server, receives the request and assigns an available private IP address to the device from a pool of addresses defined within specific address ranges.
3. **Lease Period:** The router also assigns a lease period for the IP address, specifying how long the device is allowed to use that IP address. After the lease period expires, the device may need to renew its lease or request a new IP address.
4. **Subnet:** The router also assigns a subnet mask, which defines the range of IP addresses within the local network. Devices within the same subnet can communicate directly with each other.
5. **Gateway and DNS:** The router also provides the assigned device with information about the default gateway (the router's IP address) and the DNS (Domain Name System) servers to use for domain name resolution.
6. **Static IP Addresses:** In some cases, network administrators may manually configure static (fixed) private IP addresses for certain devices on the network. These devices are typically servers or devices that require consistent, unchanging IP addresses.

Overall, the assignment of private IP addresses is a fundamental function of your router, and it uses DHCP to automate the process for most devices on the local network. This ensures that devices can connect to the network without manual configuration and share local network resources while conserving IP address space.

### What is DNS?

A DNS server, or Domain Name System server, is a crucial component of the internet infrastructure. Its primary function is to translate human-readable domain names (like [www.example.com](http://www.example.com/)) into IP addresses that computers use to identify each other on the network. DNS servers help users access websites, send emails, and perform various online activities without having to remember numerical IP addresses.

In a typical home network setup:

1. **Your Device:** When you type a domain name into your web browser (e.g., [www.example.com](http://www.example.com/)), your device needs to know the corresponding IP address to establish a connection.
2. **Router:** Your router, in addition to its role in routing data between devices on your local network and the internet, often functions as a DNS server for the devices connected to it. When your device requests the IP address for a domain name, the router checks its DNS cache to see if it already knows the IP address. If not, it forwards the DNS request to the DNS servers provided by your Internet Service Provider (ISP).
3. **ISP's DNS Servers:** These are DNS servers operated by your ISP. They have larger databases of domain names and IP addresses. If your router doesn't have the IP address for a requested domain in its cache, it asks your ISP's DNS servers for the information.
4. **Public DNS Servers:** Alternatively, instead of using your ISP's DNS servers, you can manually configure your router or device to use public DNS servers, such as those provided by Google (8.8.8.8 and 8.8.4.4) or Cloudflare (1.1.1.1).

In the `ipconfig /all` output you provided earlier, the DNS Servers entry is set to your router's IP address (192.168.1.1), indicating that your router is currently serving as the DNS server for your device. Your router, in turn, may be configured to use your ISP's DNS servers or other public DNS servers.

If you're interested in changing your DNS server settings, you can typically do so in the DNS settings section of your router's configuration page. Using public DNS servers can sometimes result in faster or more reliable DNS resolution, and it can also provide additional features such as enhanced security and filtering options.



### What are the pros and cons of using a public DNS vs an ISP's DNS? 

**ISP DNS Servers:**

**Pros:**

1. **Localized Response:** Since your ISP's DNS servers are often geographically close to you, the response time may be faster.
2. **Service Integration:** Some ISPs offer additional services or optimizations through their DNS servers that are specific to their network.

**Cons:**

1. **Logging and Privacy:** Some ISPs may log DNS requests, potentially raising privacy concerns.
2. **Potential for DNS Hijacking:** In some cases, ISPs may redirect mistyped URLs or certain requests to their own pages, which can be seen as a form of DNS hijacking.

**Public DNS Servers (e.g., Cloudflare, Google):**

**Pros:**

1. **Privacy:** Public DNS servers, like Cloudflare and Google, often emphasize user privacy and commit to not logging DNS queries on a personal level.
2. **Security:** Some public DNS providers offer features like DNS over HTTPS (DoH) or DNS over TLS (DoT) for added security during DNS resolution.
3. **Reliability:** Public DNS servers may be distributed globally, providing potentially more robust and reliable service.
4. **Additional Features:** Some public DNS services offer features like content filtering or threat blocking.

**Cons:**

1. **Geographical Distance:** Public DNS servers may not be as geographically close to you as your ISP's DNS servers, potentially leading to slightly longer response times.
2. **Lack of Integration:** Public DNS services may not integrate with your ISP's network-specific optimizations or services.
