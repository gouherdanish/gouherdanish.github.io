---
layout: post
title: "AWS Notes - IAM"
date: 2025-03-03
tags: ["Cloud"]
---

IAM stands for Identity and Access Management; Global Service

---

### IAM : Users and Groups

- Root account created by default; should not use it or share it
- Instead, create users in IAM
- One user represent one person in the organization
- Multiple users can be grouped together
- Groups only contain users not other groups
- There may be cases when 
    - a user may not belong to any group
    - a user may belong to multiple groups

---

### IAM : Policy

- Users and groups can be assigned policies which are Json documents 
- Policies define the permissions that the users/groups are allowed to do
- Don't give users more permissions that they need
    - Follow **Least Privilege Principle**
    - If a user needs access to 3 services, just create permissions for that
- If we assing a policy specific to a standalone user who doesn't belong to any group, it's called **Inline Policy**

---

### IAM : Policy Structure

VIS - Version, Id and Statement
- Version - 2012-10-17 (always)
- Id - identifier
- Statement - one or more individual statements

SEPAR - Sid, Effect, Principal, Action, Resource
- Sid - Statement identifier (optional)
- Effect - whether the statement allows or denies access
- Principal - which account/user/role the policy is applied to
- Action - which actions the policy allows or denies
- Resource - which resources the policy is applied to

<img src="{{site.url}}/images/aws/aws-policy-def.png" alt="Policy Def">

---

### IAM - Password Policy

- Require minimum length
- Require specific character
- Require to change passwords
- Require password to expire
- Prevent password re-use

---

### IAM - MFA

- Users (exp Admin users) have access to delete resources from AWS account
- Must protect Root user and IAM users
- MFA - Multi-factor Authentication
    - password (that you know) + security device (that you own)
    - Even if the password is hacked/stolen, account is not compromised
- MFA devices in AWS
    - Virtual MFA device 
        - Support for multiple tokens from multiple accounts/users on a single device (phone app)
        - e.g. Google Authenticator, Authy
    - Universal 2nd factor (U2F) Security Key
        - Yubikey - physical device provided by Yubico (3rd party)
        - supports multiple root and IAM users using a single security key
    - Hardware Key Fob MFA device
        - Provided by Gemalto (3rd party)
    - Hardware Key Fob MFA device for AWS GovCloud (US)
        - Provided by SurePassID (3rd party)

---

### How can users access AWS

- AWS Management Console
- AWS CLI
- AWS SDK (boto3)
