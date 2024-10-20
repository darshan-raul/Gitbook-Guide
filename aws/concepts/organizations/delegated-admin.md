# Delegated Admin

The **Delegated Administrator** feature in AWS allows you to assign specific administrative privileges to AWS accounts within your organization for managing specific services. This is a part of **AWS Organizations** and helps decentralize management of AWS resources by delegating specific administrative tasks to trusted accounts without granting full control over the entire organization.

#### Key Features of Delegated Administrator:

1. **Delegation of Service-Specific Administrative Tasks**: Instead of requiring the AWS management account (root account) to manage all resources and services, you can assign an account within the organization to be the "delegated administrator" for a specific service. This delegated account gets permissions to manage that service across other accounts within the organization.
2. **Security and Least Privilege**: By using delegated administrators, you limit the number of accounts with full access to manage resources, improving security and adhering to the principle of least privilege. This helps in reducing the risk associated with centralizing too many permissions in a single management account.
3. **Centralized Control via AWS Organizations**: The organization management account (the root account) retains overall control and can designate which member accounts will be delegated administrators for particular services.
4. **Separation of Duties**: Delegated administrator roles help in creating clear boundaries between teams or departments within an organization, each responsible for managing different AWS services.

***

#### How It Works:

* **Setting up a Delegated Administrator**:
  1. The **management account** (root account) of an AWS Organization assigns a specific AWS member account as the **delegated administrator** for an AWS service.
  2. Once assigned, the **delegated administrator** account can manage resources and configuration for that AWS service across other accounts in the organization.
* **Example Use Case**:
  * You might assign one account as the **delegated administrator** for **AWS IAM Access Analyzer** to handle security roles and policies across all accounts.
  * Another account could be the **delegated administrator** for **AWS Security Hub**, tasked with overseeing security and compliance checks organization-wide.
* **Supported AWS Services**: Not all AWS services support delegated administrators, but key services like **AWS Config**, **AWS GuardDuty**, **AWS Security Hub**, **AWS Firewall Manager**, and **AWS License Manager** do.

***

#### Example Workflow:

Let's say you're managing an organization with multiple AWS accounts, and you want to delegate the management of **AWS Security Hub** to a security team within your organization.

1. The organization’s **management account** assigns one of the accounts as the **delegated administrator** for **AWS Security Hub**.
2. The **delegated administrator** account can then:
   * Enable and configure **AWS Security Hub** across other accounts.
   * View security findings and compliance data from those accounts.
   * Manage security controls and settings without using the management account.
3. The **management account** maintains the ability to revoke this delegated administrator role or manage the service directly if necessary.

***

#### Benefits:

* **Scalability**: It allows large organizations with multiple AWS accounts to scale administrative control by decentralizing the management of AWS services.
* **Operational Efficiency**: Teams or departments can manage specific services relevant to their roles without the need for constant involvement from the central organization management account.
* **Improved Security**: By limiting access to only certain accounts, you can reduce the security risks of having too many permissions in a single central account.

***

#### Important Notes:

* Only one member account can be assigned as the **delegated administrator** for a particular service.
* The **management account** can assign or revoke the delegated administrator status at any time.
* The **management account** retains overall control and can still perform tasks in the service, even if there is a delegated administrator in place.

***

#### Conclusion:

The **Delegated Administrator** feature in AWS provides a way to distribute administrative responsibilities for specific services across accounts, improving security, flexibility, and operational efficiency within organizations using multiple AWS accounts.

