# Reliability vs Availability

This statement highlights the distinction between the concepts of reliability and availability in system design and engineering.

1. **Reliable System Implies Availability**:
   * **Reliability** refers to a system's ability to perform its intended function consistently and without failure over a specified period. A reliable system experiences minimal failures, ensuring that it remains operational and functional.
   * **Availability** is the degree to which a system is operational and accessible when required for use. It is typically expressed as a percentage of uptime.
   * Since a reliable system rarely fails, it tends to have high uptime, making it available when needed. Thus, if a system is reliable, it is generally available.
2. **Available System Does Not Imply Reliability**:
   * A system can be **available** if it is up and running when checked, but it may not be **reliable** if it frequently fails or has performance issues.
   * For example, a system that crashes often but is quickly rebooted might have high availability (because it's frequently up), but low reliability (because it fails frequently).
   * Therefore, an available system can be unreliable if it cannot consistently perform its intended function without failure.

In summary, reliability ensures availability, but availability alone does not guarantee reliability. A reliable system has consistent and dependable performance, leading to high availability. An available system might only be up and running at the moment, regardless of its history or future stability.
