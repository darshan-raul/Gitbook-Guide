# Nat

References:&#x20;

{% embed url="https://www.routeralley.com/guides/nat.pdf" %}

SNAT (Source Network Address Translation) and DNAT (Destination Network Address Translation) are two types of NAT used to modify IP addresses in network packets as they pass through a router or firewall. Here’s a detailed explanation of each and their differences:

#### SNAT (Source Network Address Translation)

**Purpose**: SNAT is used to change the source IP address of outbound traffic.

**Common Use Cases**:

* **Internet Access**: Allows devices on a private network to communicate with external networks (such as the internet) using a single public IP address or a pool of public IP addresses.
* **Masking Internal IPs**: Hides the internal IP addresses of devices within a network from external observers.

**How It Works**:

* When a packet leaves the internal network, the router or firewall replaces the source IP address (the private IP address) with a public IP address.
* The router keeps track of this translation in a table so that when a response comes back, it can reverse the translation and forward the response to the correct internal device.

**Example**:

* Internal IP: 192.168.1.10
* Public IP: 203.0.113.5

Before SNAT:

```
Source IP: 192.168.1.10 -> Destination IP: 198.51.100.20
```

After SNAT:

```
Source IP: 203.0.113.5 -> Destination IP: 198.51.100.20
```

#### DNAT (Destination Network Address Translation)

**Purpose**: DNAT is used to change the destination IP address of inbound traffic.

**Common Use Cases**:

* **Port Forwarding**: Allows external devices to access services on a private network (such as a web server) by translating the public IP address to a private IP address.
* **Load Balancing**: Distributes incoming requests to multiple servers within a private network.

**How It Works**:

* When a packet arrives at the router or firewall from an external network, the router changes the destination IP address to an internal IP address.
* The router keeps track of this translation so that it can correctly route responses from the internal server back to the external client.

**Example**:

* Public IP: 203.0.113.5
* Internal IP: 192.168.1.10

Before DNAT:

```
Source IP: 198.51.100.20 -> Destination IP: 203.0.113.5
```

After DNAT:

```
Source IP: 198.51.100.20 -> Destination IP: 192.168.1.10
```

#### Key Differences

1. **Direction of Translation**:
   * **SNAT**: Modifies the source IP address of outbound packets.
   * **DNAT**: Modifies the destination IP address of inbound packets.
2. **Use Cases**:
   * **SNAT**: Commonly used for allowing internal devices to access external networks.
   * **DNAT**: Commonly used for allowing external devices to access internal services.
3. **Implementation**:
   * **SNAT**: Typically used for outgoing connections from a private network to a public network.
   * **DNAT**: Typically used for incoming connections from a public network to a private network.

#### Summary

SNAT and DNAT are both essential tools for network address translation, serving different purposes depending on the direction of the traffic and the specific requirements of the network. SNAT is primarily used for translating the source IP address of outbound traffic, allowing internal devices to communicate with external networks. In contrast, DNAT is used for translating the destination IP address of inbound traffic, enabling external devices to access internal network services.
