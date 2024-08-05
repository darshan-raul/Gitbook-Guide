# Forward Proxy

#### What is a Forward Proxy Server?

A forward proxy server is an intermediary server that sits between client applications and the internet. The proxy server receives requests from the clients, forwards them to the destination server, and then returns the responses to the clients. This setup is often used to:

* **Improve security** by hiding the client's IP address.
* **Control access** to the internet.
* **Cache content** to improve performance.
* **Monitor and log** internet usage.

#### Setting Up a Forward Proxy Server in Linux

One of the most popular software packages for setting up a forward proxy server on Linux is Squid. Here’s how you can set up and configure Squid as a forward proxy server:

**Step-by-Step Guide to Setting Up Squid Proxy**

1.  **Install Squid:**

    For Debian/Ubuntu:

    ```bash
    sudo apt-get update
    sudo apt-get install squid
    ```

    For CentOS/RHEL:

    ```bash
    sudo yum install squid
    ```
2.  **Configure Squid:**

    The main configuration file for Squid is located at `/etc/squid/squid.conf`. You can edit this file to set up the desired configurations.

    Basic configuration example:

    ```bash
    sudo nano /etc/squid/squid.conf
    ```

    Add or modify the following lines:

    ```conf
    http_port 3128
    acl allowed_ips src 192.168.1.0/24  # Replace with your network
    http_access allow allowed_ips
    ```
3.  **Start and Enable Squid:**

    ```bash
    sudo systemctl start squid
    sudo systemctl enable squid
    ```
4.  **Configure Clients to Use the Proxy:**

    On the client machine, configure the web browser or system settings to use the proxy server. For example, set the proxy address to `http://<proxy-server-ip>:3128`.

#### Controls and Examples

Squid provides a variety of controls to manage and restrict internet usage. Here are some examples:

1.  **Restrict Access to Specific Websites:**

    ```conf
    acl blocked_sites dstdomain .facebook.com .youtube.com
    http_access deny blocked_sites
    ```
2.  **Block Specific File Types:**

    ```conf
    acl blocked_files url_regex -i \.mp3$ \.exe$ \.mp4$
    http_access deny blocked_files
    ```
3.  **Require Authentication:**

    ```bash
    sudo apt-get install apache2-utils  # For htpasswd utility
    sudo htpasswd -c /etc/squid/passwd myuser
    ```

    Add the following to `squid.conf`:

    ```conf
    auth_param basic program /usr/lib/squid/basic_ncsa_auth /etc/squid/passwd
    auth_param basic children 5
    auth_param basic realm Squid proxy-caching web server
    auth_param basic credentialsttl 2 hours
    acl authenticated proxy_auth REQUIRED
    http_access allow authenticated
    ```
4.  **Limit Bandwidth:**

    ```conf
    delay_pools 1
    delay_class 1 1
    delay_parameters 1 128000/128000  # 128 KBps
    acl all src 0.0.0.0/0
    delay_access 1 allow all
    ```
5.  **Cache Management:**

    ```conf
    cache_dir ufs /var/spool/squid 100 16 256  # 100 MB cache size
    maximum_object_size 4096 KB
    minimum_object_size 0 KB
    ```
6.  **Log Internet Usage:**

    Squid logs can be found in `/var/log/squid/access.log`. You can use tools like `sarg` (Squid Analysis Report Generator) to analyze these logs.

    Install and configure `sarg`:

    ```bash
    sudo apt-get install sarg
    sudo nano /etc/sarg/sarg.conf
    ```

    Update the configuration as needed and generate reports:

    ```bash
    sudo sarg
    ```

#### Conclusion

Setting up a forward proxy server with Squid on a Linux machine is straightforward. Squid’s configuration is highly flexible, allowing you to implement various controls to manage and restrict internet access according to your requirements. By following the steps and examples provided, you can effectively set up and customize a forward proxy server for your network.

***

After setting up a forward proxy server like Squid, you need to configure your client machines to route their internet traffic through the proxy server. This will ensure that all HTTP requests are passed through the proxy. Here's how you can achieve that and address non-HTTP requests:

#### Configuring Client Machines to Use the Proxy

**For Web Browsers:**

1. **Google Chrome/Chromium:**
   * Go to `Settings`.
   * Scroll down and click `Advanced`.
   * Under `System`, click `Open proxy settings`.
   * In the `Internet Properties` window, go to the `Connections` tab and click `LAN settings`.
   * Check `Use a proxy server for your LAN` and enter the IP address and port of your Squid proxy server (e.g., `192.168.1.100:3128`).
2. **Firefox:**
   * Go to `Options`.
   * Scroll down to `Network Settings` and click `Settings`.
   * Select `Manual proxy configuration` and enter the IP address and port of your Squid proxy server.

**For the Entire System (Linux):**

To route all HTTP and HTTPS traffic through the proxy server, you need to set environment variables.

1.  Open a terminal and edit the profile file:

    ```bash
    sudo nano /etc/profile
    ```
2.  Add the following lines to set the proxy for all users:

    ```bash
    export http_proxy="http://192.168.1.100:3128"
    export https_proxy="http://192.168.1.100:3128"
    ```
3.  Save the file and apply the changes:

    ```bash
    source /etc/profile
    ```

For non-HTTP requests (like FTP, SSH, etc.), Squid may not handle these directly. You need to use different strategies or additional tools.

#### Handling Non-HTTP Requests

1.  **HTTPS Requests:** Squid can handle HTTPS requests by using the `CONNECT` method. Ensure your Squid configuration allows HTTPS traffic:

    ```conf
    acl SSL_ports port 443
    acl CONNECT method CONNECT
    http_access allow CONNECT SSL_ports
    ```
2.  **FTP Requests:** Squid can be configured to handle FTP requests as well:

    ```conf
    acl FTP proto FTP
    http_access allow FTP
    ```
3.  **Non-Proxy Aware Applications:** For applications that do not natively support proxies (e.g., SSH, certain database clients), you can use tools like `tsocks` or `proxychains` to route their traffic through the proxy.

    **Using proxychains:**

    1.  Install proxychains:

        ```bash
        sudo apt-get install proxychains
        ```
    2.  Edit the proxychains configuration file:

        ```bash
        sudo nano /etc/proxychains.conf
        ```
    3.  Add your Squid proxy server details at the end:

        ```conf
        http 192.168.1.100 3128
        ```
    4.  Use proxychains to run applications through the proxy:

        ```bash
        proxychains <application>
        ```

#### Transparent Proxying

If you want to enforce proxy usage without client-side configuration, you can set up a transparent proxy. This requires routing all traffic through the proxy server using firewall rules. Here’s an example using `iptables`:

1.  **Enable IP Forwarding:**

    ```bash
    echo 1 > /proc/sys/net/ipv4/ip_forward
    ```
2.  **Configure `iptables`:**

    ```bash
    iptables -t nat -A PREROUTING -i eth0 -p tcp --dport 80 -j REDIRECT --to-port 3128
    ```

    This redirects all HTTP traffic (port 80) to the Squid proxy port (3128). For HTTPS (port 443), you'll need additional configuration and may require SSL bumping, which is more complex and may have legal and ethical implications.

#### Conclusion

Configuring a forward proxy server involves setting up the server (e.g., Squid) and ensuring that client machines route their traffic through it. By configuring client browsers or setting system-wide environment variables, you can route HTTP and HTTPS traffic through the proxy. For non-HTTP traffic, using tools like `proxychains` can help. Additionally, setting up a transparent proxy can enforce proxy usage without requiring client-side configuration.
