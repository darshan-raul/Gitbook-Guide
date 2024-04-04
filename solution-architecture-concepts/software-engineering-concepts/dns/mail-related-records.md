# Mail related records

SPF, DKIM, and DMARC are three core email authentication technologies that work together to protect your domain's reputation and combat email fraud. Here's a breakdown of each and how they relate:

**SPF (Sender Policy Framework)**

* **What it does:** SPF defines a list of IP addresses authorized to send emails on behalf of your domain.
* **How it works:**
  1. You publish an SPF record as a TXT record in your domain's DNS.
  2. When a receiving mail server gets an email claiming to be from your domain, it checks the sending IP address against your SPF record.
  3. If the IP is in your SPF record, the email is more likely to be legitimate. If not, it may be flagged as suspicious.

**DKIM (DomainKeys Identified Mail)**

* **What it does:** DKIM uses cryptographic signatures to verify the integrity of email content.
* **How it works:**
  1. You generate a public/private key pair. The public key is added to your domain's DNS.
  2. When you send an email, a unique digital signature based on specific email headers and body content is generated using your private key.
  3. Receiving servers use your public key to verify the signature, ensuring the email hasn't been tampered with in transit.

**DMARC (Domain-based Message Authentication, Reporting, and Conformance)**

* **What it does:** DMARC builds on SPF and DKIM, telling receiving servers what to do if an email fails SPF and/or DKIM checks. It also provides a mechanism for you to receive reports about how your domain's emails are being handled.
* **How it works:**
  1. You create a DMARC policy (as a DNS TXT record) specifying actions (`none`, `quarantine`, `reject`) to take on emails failing authentication.
  2. Receiving servers respect your DMARC policy.
  3. You receive aggregate reports detailing authentication results for your domain.

**How They're Related**

* **Complementary:** SPF and DKIM are the authentication pillars, while DMARC provides instructions for how to handle authentication failures.
* **Enhanced Protection:** Together, they improve email deliverability by showing mail servers that emails from your domain are legitimate.
* **DMARC Reporting:** DMARC's reporting feature gives you insights into potential misuse of your domain, letting you refine your SPF and DKIM settings.

**In Summary**

SPF, DKIM, and DMARC form a robust defense against:

* **Phishing:** Scammers spoofing your email address to send fraudulent emails.
* **Spam:** Mass emails using your domain to bypass filters.
* **Impersonation:** Attacks damaging your brand reputation.

