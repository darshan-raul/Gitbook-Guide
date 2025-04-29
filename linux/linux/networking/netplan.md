# netplan

**Netplan** is a modern network configuration utility for Linux systems, particularly Ubuntu. Introduced in Ubuntu 17.10, it replaces the traditional `/etc/network/interfaces` file, providing a unified and declarative approach to network configuration using YAML files. ([Netplan network configuration tutorial for beginners - LinuxConfig](https://linuxconfig.org/netplan-network-configuration-tutorial-for-beginners?utm_source=chatgpt.com))

***

#### 🔧 What Is Netplan?

Netplan serves as an abstraction layer that simplifies network configuration by allowing administrators to define network settings in YAML format. These configurations are then translated into settings for the underlying network management tools.

***

#### ⚙️ How Netplan Works

1. **Configuration Files**: Netplan reads YAML configuration files located in `/etc/netplan/`.
2. **Renderers**: It supports two primary backends, known as "renderers":
   * **systemd-networkd**: Ideal for servers and headless systems.
   * **NetworkManager**: Suited for desktop environments and systems requiring dynamic network management.
3. **Application**: During system boot, Netplan processes the YAML files and generates corresponding configuration files for the selected renderer, applying the network settings accordingly. ([Canonical Netplan](https://netplan.io/?utm_source=chatgpt.com), [Netplan network configuration tutorial for beginners - LinuxConfig](https://linuxconfig.org/netplan-network-configuration-tutorial-for-beginners?utm_source=chatgpt.com), [Netplan systemd-networkd and NetworkManager trio : r/Ubuntu](https://www.reddit.com/r/Ubuntu/comments/16oizuj/netplan_systemdnetworkd_and_networkmanager_trio/?utm_source=chatgpt.com))

***

#### 📝 Example Configuration

Here's a sample Netplan configuration that sets a static IP address using `systemd-networkd`: ([Network configuration using Netplan - Akamai TechDocs](https://techdocs.akamai.com/cloud-computing/docs/network-configuration-using-netplan?utm_source=chatgpt.com))

```yaml
network:
  version: 2
  renderer: networkd
  ethernets:
    enp3s0:
      dhcp4: no
      addresses: [192.168.1.100/24]
      gateway4: 192.168.1.1
      nameservers:
        addresses: [8.8.8.8, 8.8.4.4]
```

To apply this configuration:

```bash
sudo netplan apply
```

***

#### 🛠️ Netplan Commands

* **`netplan generate`**: Generates configuration files for the chosen renderer.
* **`netplan apply`**: Applies the current configuration.
* **`netplan try`**: Applies the configuration temporarily, allowing rollback if issues are detected.
* **`netplan set`**: Modifies settings in the configuration files.
* **`netplan get`**: Displays the current configuration. ([How to Use the Netplan Network Configuration Tool on Linux](https://www.linux.com/topic/distributions/how-use-netplan-network-configuration-tool-linux/?utm_source=chatgpt.com))

***

#### 📚 Learn More

For detailed documentation and advanced configurations, visit the [official Netplan documentation](https://netplan.io/). ([Canonical Netplan](https://netplan.io/?utm_source=chatgpt.com))

***
