# dns over https

DNS over HTTPS (DoH) works by taking the traditional process of looking up a website's IP address and wrapping it in the same encryption used for secure websites, making your DNS queries private and tamper-proof.

Here’s a step-by-step look at how it works **in action**, compared to the traditional DNS process:

| Step                      | Traditional DNS (Insecure)                                                                                            | DNS over HTTPS (DoH)                                                                                                                                                 |
| ------------------------- | --------------------------------------------------------------------------------------------------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **1. Request Initiation** | You type `www.example.com` into your browser. A plaintext DNS query is created.                                       | You type `www.example.com`. Your browser or OS creates a DNS query and **encrypts** it as an HTTPS request.                                                          |
| **2. Query Transport**    | The unencrypted query is sent over **UDP/TCP port 53**. It is visible to your ISP and anyone on the network.          | The encrypted query is sent to a DoH server's specific URL (e.g., `https://dns.example/dns-query`) over **HTTPS port 443**. It blends with other secure web traffic. |
| **3. Resolution Path**    | Query goes to your ISP's resolver, then through a hierarchy of root and authoritative servers to find the IP address. | The encrypted query goes directly to the **DoH resolver** (e.g., Cloudflare, Google). This resolver performs the lookup hierarchy on your behalf.                    |
| **4. Response Return**    | The IP address (`93.184.216.34`) is sent back as a plaintext response, vulnerable to spoofing or alteration.          | The DoH resolver encrypts the IP address answer inside an HTTPS response and sends it back.                                                                          |
| **5. Result**             | Your browser receives the IP and connects to the website. The entire query/response was exposed.                      | Your browser decrypts the response, obtains the IP, and connects. The query and destination were hidden from your local network.                                     |

#### 🔧 Where and How DoH is Implemented

DoH can be activated at different levels, affecting what traffic on your device is protected:

* **In Web Browsers**: The most common method. Browsers like **Firefox, Chrome, and Edge** have built-in DoH settings. When enabled, **only DNS queries from that specific browser** are encrypted. Queries from other apps on your device still use traditional DNS unless the OS is configured.
* **At the Operating System Level**: Configuring DoH in your OS (like Windows 11 or macOS) encrypts **all DNS queries from every application** on the device, providing system-wide protection.

#### ⚠️ Important Considerations

While DoH enhances privacy, its implementation has trade-offs:

* **Security & Privacy vs. Visibility**: The encryption that protects you from eavesdroppers also makes DNS traffic invisible to network security tools. This can bypass **corporate web filters, parental controls, or security monitoring** that rely on inspecting DNS queries. In enterprise settings, it's often recommended to use an internal DoH resolver instead of public ones to maintain security policies.
* **Centralization Concern**: DoH can centralize DNS traffic with a few large public providers (like Google or Cloudflare), giving them broad visibility into browsing patterns, even though the traffic is encrypted between you and them.

