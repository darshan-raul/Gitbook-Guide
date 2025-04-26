# dnsmasq

Below is an overview of **dnsmasq**, a lightweight DNS forwarder and DHCP server, along with step-by-step examples showing how to install, configure, and use it in real-world scenarios.

### Summary

**dnsmasq** is free, open-source software that provides DNS caching, DHCP services, router advertisement, TFTP and PXE network boot features—all in a single, small-footprint daemon ideal for home networks, embedded devices, or virtualized environments. <mark style="background-color:orange;">It can speed up DNS resolution by caching queries, serve local hostnames defined in</mark> <mark style="background-color:orange;"></mark><mark style="background-color:orange;">`/etc/hosts`</mark><mark style="background-color:orange;">, allocate IP addresses via DHCP (static or dynamic), implement basic ad-blocking, and even support PXE boot for diskless machines</mark>. In the sections that follow, you’ll learn how to install dnsmasq on a Linux host, configure basic DNS and DHCP functionality, and explore practical use cases such as setting up a local caching DNS server, providing DHCP for a LAN, blocking ads via host-file blacklisting, and enabling PXE boot provisioning.

### What Is dnsmasq?

* **Definition**: dnsmasq is a lightweight DNS, DHCP, TFTP, PXE and router-advertisement server designed for small networks. It combines multiple networking services in one daemon to simplify configuration and reduce resource usage ([The Kelleys](https://thekelleys.org.uk/dnsmasq/doc.html?utm_source=chatgpt.com)).
* **Origins & Licensing**: Created by Simon Kelley in 2001, dnsmasq is written in C and released under the GNU GPL v2 or v3 ([Wikipedia](https://en.wikipedia.org/wiki/Dnsmasq?utm_source=chatgpt.com)).
* **Use Cases**: Commonly embedded in home-router firmware, IoT gateways, smartphones for tethering, virtual-network bridges, and small office/home office environments ([The Kelleys](https://thekelleys.org.uk/dnsmasq/doc.html?utm_source=chatgpt.com)).

### Key Features

1. **DNS Forwarding & Caching**\
   dnsmasq listens on a local IP (e.g., 127.0.0.1), caches upstream DNS query results, and serves them to clients to improve lookup performance and reduce external queries ([Wikipedia](https://en.wikipedia.org/wiki/Dnsmasq?utm_source=chatgpt.com), [The Kelleys](https://thekelleys.org.uk/dnsmasq/doc.html?utm_source=chatgpt.com)).
2. **DHCP Server**\
   Supports both dynamic and static leases, multiple subnets, DHCPv6, and integrates seamlessly with its DNS component so that DHCP-assigned hosts appear in DNS lookups ([Debian Wiki](https://wiki.debian.org/dnsmasq?utm_source=chatgpt.com), [Wikipedia](https://en.wikipedia.org/wiki/Dnsmasq?utm_source=chatgpt.com)).
3. **Local Hostname Resolution**\
   Automatically reads `/etc/hosts` to resolve local machine names not present in public DNS, letting you refer to local devices by memorable names ([Wikipedia](https://en.wikipedia.org/wiki/Dnsmasq?utm_source=chatgpt.com)).
4. **Router Advertisement (RA)**\
   Can send IPv6 RA messages to advertise network prefixes to IPv6-capable clients ([The Kelleys](https://thekelleys.org.uk/dnsmasq/docs/dnsmasq-man.html?utm_source=chatgpt.com)).
5. **Network Boot (PXE/TFTP)**\
   Provides BOOTP, PXE, and TFTP support for diskless booting of machines over the network ([The Kelleys](https://thekelleys.org.uk/dnsmasq/docs/dnsmasq-man.html?utm_source=chatgpt.com)).
6. **Ad-Blocking & NXDOMAIN Filtering**\
   By loading custom host-file blacklists (e.g., ad-server domains mapped to 0.0.0.0), dnsmasq can block ads at the network level. It can also filter out bogus NXDOMAIN responses injected by some ISPs ([Wikipedia](https://en.wikipedia.org/wiki/Dnsmasq?utm_source=chatgpt.com)).
7. **IPv6 & DNSSEC Support**\
   Fully supports IPv6 DNS queries and validation via DNSSEC ([Wikipedia](https://en.wikipedia.org/wiki/Dnsmasq?utm_source=chatgpt.com)).

### Installation

#### Debian/Ubuntu

```bash
sudo apt update
sudo apt install dnsmasq
```

The package includes sane defaults; the main configuration file is `/etc/dnsmasq.conf` ([Debian Wiki](https://wiki.debian.org/dnsmasq?utm_source=chatgpt.com)).

#### Arch Linux

```bash
sudo pacman -Syu dnsmasq
```

After installation, systemd unit files (`dnsmasq.service`) are provided. Configuration lives in `/etc/dnsmasq.conf` and `/etc/dnsmasq.d/` ([ArchWiki](https://wiki.archlinux.org/title/Dnsmasq?utm_source=chatgpt.com), [Arch Linux](https://archlinux.org/packages/extra/x86_64/dnsmasq/?utm_source=chatgpt.com)).

### Basic Configuration

Most use cases require minimal edits to `/etc/dnsmasq.conf`:

```ini
# Listen only on the loopback interface
interface=lo

# Never forward plain names (no dots)
domain-needed

# Don’t forward addresses in the “example.com” domain
bogus-nxdomain=example.com

# Upstream DNS servers
server=8.8.8.8
server=8.8.4.4

# Read additional host-file entries
addn-hosts=/etc/dnsmasq.hosts
```

If no options are set, dnsmasq will use `/etc/resolv.conf` and `/etc/hosts` by default ([The Kelleys](https://thekelleys.org.uk/dnsmasq/docs/setup.html?utm_source=chatgpt.com)).

Enable and start:

```bash
sudo systemctl enable --now dnsmasq
```

### Practical Examples

#### 1. Local DNS Caching

Modify `/etc/resolv.conf` to use the local cache:

```ini
nameserver 127.0.0.1
```

Now all DNS queries from your machine go through dnsmasq’s cache, reducing external lookups and speeding up repeated queries ([Reddit](https://www.reddit.com/r/archlinux/comments/g54bjl/how_to_use_dnsmasq_for_caching/?utm_source=chatgpt.com)).

#### 2. Serving DHCP on a LAN

Add to `/etc/dnsmasq.d/dhcp.conf`:

```ini
# DHCP range and lease time
dhcp-range=192.168.1.100,192.168.1.200,12h

# Static lease for server
dhcp-host=00:11:22:33:44:55,server1,192.168.1.10
```

Clients on the 192.168.1.0/24 network will receive addresses from .100 to .200, while “server1” always gets .10 ([Debian Wiki](https://wiki.debian.org/dnsmasq?utm_source=chatgpt.com)).

#### 3. Blocking Ads Network-wide

Create `/etc/blocklist.txt` containing:

```
0.0.0.0 ads.example.com
0.0.0.0 tracker.example.net
```

Then in `dnsmasq.conf`:

```ini
addn-hosts=/etc/blocklist.txt
```

Restart dnsmasq; requests to blacklisted domains now resolve to 0.0.0.0 ([Wikipedia](https://en.wikipedia.org/wiki/Dnsmasq?utm_source=chatgpt.com)).

#### 4. PXE Boot for Diskless Clients

Enable TFTP and PXE in `/etc/dnsmasq.d/pxe.conf`:

```ini
enable-tftp
tftp-root=/srv/tftp
pxe-prompt="Boot: "
pxe-service=0,"Install Linux",pxelinux
```

Place PXELINUX files under `/srv/tftp`; on network boot, clients receive IP via DHCP and download bootloader via TFTP ([The Kelleys](https://thekelleys.org.uk/dnsmasq/docs/dnsmasq-man.html?utm_source=chatgpt.com)).

### Troubleshooting & Tips

* **DNS Not Resolving**: Ensure `/etc/resolv.conf` points to 127.0.0.1 after starting dnsmasq; check `systemctl status dnsmasq` for errors ([Arch Linux Forums](https://bbs.archlinux.org/viewtopic.php?id=284300\&utm_source=chatgpt.com)).
* **Permission Denied**: If binding to low-numbered ports (<1024), dnsmasq must run as root or you must grant capabilities (`cap_net_bind_service`) ([The Kelleys](https://thekelleys.org.uk/dnsmasq/docs/dnsmasq-man.html?utm_source=chatgpt.com)).
* **Interference with NetworkManager**: NetworkManager can manage its own dnsmasq instance; disable one or the other to avoid conflicts ([YouTube](https://www.youtube.com/watch?v=n2eajB9Qsws\&utm_source=chatgpt.com)).

### Conclusion

dnsmasq excels at simplifying small-network DNS and DHCP needs with minimal configuration and resource usage. Whether you need a local caching resolver, a lightweight DHCP server, ad-blocking, or PXE boot services, dnsmasq provides a unified solution that “just works” on a wide range of Unix-like systems. For more advanced options, consult the official manual at thekelleys.org.uk ([The Kelleys](https://thekelleys.org.uk/dnsmasq/doc.html?utm_source=chatgpt.com), [The Kelleys](https://thekelleys.org.uk/dnsmasq/docs/setup.html?utm_source=chatgpt.com)).
