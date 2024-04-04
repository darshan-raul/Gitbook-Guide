# Non functional requirements

Here's a breakdown of common non-functional requirements (NFRs) in solution architecture, along with explanations and examples:

**1. Performance**

* **Scalability:** The ability of the system to handle increasing workloads (users, transactions, data) without significant performance degradation.
  * **Example:** "The system should handle up to 5000 concurrent users with response times under 2 seconds for 95% of transactions."
* **Availability:** The proportion of time the system is up and functioning. Often expressed as a percentage.
  * **Example:** "The system should have 99.99% uptime (excluding planned maintenance)."
* **Response Time:** The time taken for the system to respond to a user request.
  * **Example:** "The webpage should load fully within 3 seconds under normal load."

**2. Security**

* **Confidentiality:** Protecting sensitive data from unauthorized access.
  * **Example:** "All user data must be encrypted at rest and in transit."
* **Integrity:** Ensuring data is accurate and hasn't been tampered with.
  * **Example:** "The system must use mechanisms like digital signatures to prevent unauthorized modification of financial transactions."
* **Authentication:** Verifying the identity of users before granting access.
  * **Example:** "The system should implement two-factor authentication for all administrative accounts."
* **Authorization:** Defining and enforcing access permissions for different roles/users.
  * **Example:** "Standard users should only be able to view their own data, not modify data of others."

**3. Usability**

* **Ease of Use:** How intuitive and easy the system is to learn and use.
  * **Example:** "A new user should complete a core task within 5 minutes without training."
* **Accessibility:** Ensuring the system is usable by people with disabilities.
  * **Example:** "The system should comply with WCAG 2.1 AA accessibility guidelines."

**4. Reliability**

* **Fault Tolerance:** The ability of the system to continue operating even when components fail.
  * **Example:** "The system should have redundant web servers to handle individual server failures."
* **Disaster Recovery:** The ability to recover the system and its data after a major outage or disaster.
  * **Example:** "The system should have a data backup and replication strategy with recovery point and time objectives (RPO/RTO) of less than 4 hours."

**5. Maintainability**

* **Modifiability:** How easy it is to make changes to the system.
  * **Example:** "Adding a new feature should take less than two developer weeks."
* **Testability:** How easy it is to test changes to the system.
  * **Example:** "The system should have unit tests covering 80% of the codebase."

**6. Other Considerations**

* **Operational Costs:** The ongoing costs of maintaining and running the system.
* **Compliance:** Adherence to laws, regulations, and industry standards.
* **Portability:** The ability to move the system to different environments (e.g., different cloud providers).
* **Supportability:** The ease of troubleshooting and providing user support.

**Important Notes:**

* The importance and specific metrics for NFRs vary wildly between projects.
* NFRs should be defined early on and have measurable targets so they can be considered throughout the design and implementation of the system.

Let me know if you want a deeper dive into a specific category or would like examples tailored to a certain type of solution!
