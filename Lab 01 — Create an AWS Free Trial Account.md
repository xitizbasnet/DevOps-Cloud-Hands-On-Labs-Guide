# Section 1 — Cloud Basics

## Lab 01 — Create an AWS Free Trial Account

> [!IMPORTANT]
> This lab walks you through creating an AWS Free Tier account and applying essential security and cost-management configurations before beginning the remaining labs.

## Objective

Set up a root AWS account with billing alerts and basic security hardening.

## Prerequisites

Before you begin, ensure that you have:

* 📧 A valid email address.
* 📱 A mobile phone for identity verification (OTP).
* 💳 A valid credit or debit card for account verification.
* 🌐 A stable internet connection.

> [!NOTE]
> AWS may place a temporary authorization hold (approximately **₹2**) on your payment method during account verification. This amount is typically released automatically.

---

## Procedure

### Step 1 — Create an AWS Account

1. Navigate to the AWS website:

   * **https://aws.amazon.com**
2. Select **Create an AWS Account**.

### Step 2 — Register Your Account

1. Enter your **email address**.
2. Provide an **AWS account name**.
3. Verify your email address using the **OTP** received.

### Step 3 — Configure the Account Type

1. Select **Personal** as the account type.
2. Enter your **credit/debit card** details.
3. Complete the payment verification process.

### Step 4 — Complete Identity Verification

1. Verify your identity using the **phone OTP**.

### Step 5 — Select a Support Plan

1. Choose the **Basic Support (Free)** plan.
2. Sign in to the **AWS Management Console**.

### Step 6 — Configure the Default AWS Region

1. From the **Region selector** in the upper-right corner of the AWS Console, select:

   ```text
   ap-south-1 (Mumbai)
   ```

### Step 7 — Configure Billing Alerts

1. Navigate to:

   ```text
   Billing → Budgets → Create Budget
   ```

2. Create a billing budget.

3. Set the alert threshold to:

   ```text
   $5 USD
   ```

### Step 8 — Enable Multi-Factor Authentication (MFA)

1. Navigate to:

   ```text
   IAM → Root Account
   ```

2. Enable **Multi-Factor Authentication (MFA)**.

3. Use **Google Authenticator** (or another supported authenticator application).

---

## Best Practice Tips

> [!TIP]
> Following these recommendations will improve the security, governance, and cost management of your AWS environment.

### 🔒 Secure Your AWS Account

* **Never use the root account for daily tasks.**
* Create an **IAM Administrator** user immediately after the account is created.

### 📋 Enable Audit Logging

* Enable **AWS CloudTrail** from **Day 1**.
* The AWS Free Tier includes **one CloudTrail trail** for audit logging.

### 💰 Configure Cost Controls

* Configure billing alerts at:

  * **$1 USD**
  * **$5 USD**

This helps prevent unexpected AWS charges.

### 🏷️ Apply Resource Tags

Tag every AWS resource using the following standard tags:

| Tag             | Value      |
| --------------- | ---------- |
| **Environment** | `Dev`      |
| **Owner**       | `YourName` |
| **Project**     | `Learning` |

---

## Validation

Verify that the following tasks have been completed successfully:

* ✅ AWS Free Tier account created.
* ✅ AWS Region set to **ap-south-1 (Mumbai)**.
* ✅ Billing budget configured with a **$5 USD** alert.
* ✅ MFA enabled for the root account.
* ✅ CloudTrail enabled.
* ✅ Resource tagging strategy prepared for future labs.

---

## Summary

In this lab, you:

* Created an AWS Free Tier account.
* Configured the default AWS Region.
* Enabled MFA for the root account.
* Created a billing budget.
* Applied foundational AWS security and cost-management best practices.

You are now ready to continue with the next lab in the **Cloud Basics** learning path.
