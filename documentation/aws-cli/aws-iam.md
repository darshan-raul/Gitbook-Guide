# AWS IAM

## Users 

**List all users:**

`aws iam list-users`

**Get User Info:**

`aws iam get-user --user-name Bob`

**Create User:**

`aws iam create-user --user-name <user_name>`

**Add User to Group:**

`aws iam add-user-to-group --user-name <user_name> --group-name <group_name>`

**Remove User from Group:**

`aws iam remove-user-from-group --user-name <user_name> --group-name <group_name>`

**Delete User:**

`aws iam delete-user --user-name <user_name>`

**Update User:**

`aws iam update-user --user-name <name> --new-user-name <new_user_name>`

**List Groups of User:**

`aws iam list-groups-for-user --user-name <user_name>`

## Groups

**List all groups:**

`aws iam list-groups`

**View group details:**

`aws iam get-group --group-name <group_name>`

**Create Group:**

`aws iam create-group --group-name <group_name>`

**Update Group:**

`aws iam update-group --group-name <group_name> --new-group-name <new_name>`

**Delete Group:**

`aws iam delete-group --group-name <group_name>`

## Roles

**List all roles:**

`aws iam list-roles`

**View role details:**

`aws iam get-role --role-name <role_name>`

**Create Role:**

`aws iam create-role --role-name <role_name> --assume-role-policy-document file://<file_name>.json`

**Update Role:**

`update-role --role-name <value> [--description <value>]`

\*\*\*\*



