# Identity

<figure><img src="../.gitbook/assets/image (1) (1) (1) (1) (1) (1) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

IAM allows you to grant granular access to specific Google Cloud resources and prevent access to other resources. It follows the security principle of least privilege, where users only have the permissions they need.

IAM works by defining who (identity) has what access (role) for which resource. Permissions are grouped into roles, which are then granted to authenticated principals (users, service accounts, groups, etc.).

An IAM policy, or allow policy, defines and enforces the roles granted to each principal for a resource. When a principal tries to access a resource, IAM checks the resource's policy to determine if the action is permitted.

IAM supports a resource hierarchy, where resources inherit policies from their parent resources. This allows policies to be set at the organization, folder, project, or individual resource level.

IAM provides predefined roles for most Google Cloud services, as well as the ability to create custom roles. Roles contain collections of permissions that can be granted to principals.

The IAM API is eventually consistent, meaning changes may take time to fully propagate and affect access checks.

The page also covers key IAM concepts like resources, permissions, principals (users, service accounts, groups), and the IAM consistency model.

Citations: \[1] https://cloud.google.com/iam/docs/overview
