# External ID and Confused deputy problem

Adding an External ID to an IAM role’s trust policy is like adding a "unique passcode" to a door that only certain people are allowed to open. While a standard trust policy says, "I trust Account B to enter," the External ID adds, "...but only if Account B also provides this specific secret code."

***

### 1. The Confused Deputy: An ELI5 Explanation

Imagine there is a popular Valet Service (the "Deputy") that parks cars for many different people.

1. The Setup: You give the Valet your car key (an IAM Role ARN) and tell them, "You have permission to park my car."
2. The Flaw: Another person, let’s call him Sneaky Sam, also uses the Valet. Sam knows you use this service. Sam goes to the Valet and says, "Hey, here is a key (your Role ARN). Go move this car for me."
3. The Problem: The Valet looks at the key and says, "Yep, I am authorized to use this key!" The Valet doesn't realize Sam is tricking them into moving _your_ car instead of Sam’s.

In AWS, the Valet is a third-party SaaS vendor (like a monitoring or billing tool). Because the vendor has permission to assume roles in many different customer accounts, a malicious customer could provide _your_ Role ARN to the vendor. The vendor, acting as a "Confused Deputy," would then use its own legitimate permissions to access your data, thinking it's working for the malicious customer.

The Fix: You give the Valet a secret code (External ID) and say, "Only move my car if I am the one asking, and use this code `XYZ-123`." Now, when Sneaky Sam provides your ARN, he doesn't know your secret code, so the Valet's attempt to access your account fails.

***

### 2. End-to-End Steps Guide

This scenario involves two parties: The Vendor (the "Deputy" who needs access) and The Customer (You, who owns the resources).

#### Phase 1: The Vendor (Trusted Account)

1.  Generate a Unique ID: The vendor generates a unique, random string (like a UUID) for you.

    > Note: The vendor should always generate this, not the customer, to ensure it’s unique across all their clients.
2. Provide Account Info: The vendor gives you their AWS Account ID and the External ID.

#### Phase 2: The Customer (You / Trusting Account)

1. Create the Role:
   * Go to the IAM Console -> Roles -> Create role.
   * Select AWS account as the trusted entity.
   * Select Another AWS account and enter the Vendor's Account ID.
   * Crucial Step: Check the box "Require external ID".
   * Enter the External ID provided by the vendor.
2. Attach Permissions:
   * Attach the specific policy (e.g., `ReadOnlyAccess`) that the vendor needs.
3. Finalize & Share:
   * Name the role (e.g., `VendorMonitoringRole`) and click Create.
   * Copy the Role ARN (e.g., `arn:aws:iam::123456789012:role/VendorMonitoringRole`) and send it back to the vendor.

#### Phase 3: The Vendor (Assuming the Role)

The vendor’s application will now call the AWS Security Token Service (STS) to get access. Their code will look like this:

Bash

```
# How the vendor programmatically assumes your role
aws sts assume-role \
  --role-arn "arn:aws:iam::YOUR_ACCOUNT_ID:role/VendorMonitoringRole" \
  --role-session-name "VendorSession" \
  --external-id "THE_SECRET_CODE_THEY_GAVE_YOU"
```

If they don't include that `--external-id`, AWS will reject the request, even though the Account ID matches.

***

#### Summary Table

| **Feature**             | **Without External ID**         | **With External ID**                         |
| ----------------------- | ------------------------------- | -------------------------------------------- |
| Who can trigger access? | Anyone who knows your Role ARN. | Only the Vendor when using your specific ID. |
| Confused Deputy Risk?   | High                            | Mitigated                                    |
| Trust Basis             | Just the Account ID.            | Account ID + Secret "Handshake".             |

