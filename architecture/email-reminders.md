# Email Reminders

With a premium subscription, users can configure email notifications for themselves and all designated beneficiaries. Advanced notice periods can be customized by specifying the number of days prior to each activation event.&#x20;

For more details, see [Premium Features - Email Reminders](../user-guide/premium-features/#email-reminders).

The email notification feature is supported by the following on-chain contracts and off-chain services:

* **Router Contract**\
  The contract responsible for creating new legacy contracts.
* **Settings Contract**\
  Allows the contract owner to configure settings prior to activation, including email addresses, beneficiaries, and reminder parameters.
* **Automation Contract**\
  Periodically checks the status of legacy contracts. Whenever a user updates their reminder settings (for example, changing email addresses, beneficiaries, or reminder timing), the updated information is recorded in this contract.
* **Chainlink Automation (Cron Job)**\
  An off-chain automation service provided by Chainlink that periodically invokes the Automation Contract to evaluate activation timelines and determine whether notification conditions have been met.
* **Chainlink Functions Contract**\
  Used to securely trigger off-chain actions, including initiating email delivery workflows to owners and beneficiaries when notification conditions are satisfied.
* **Mailjet Email Delivery Service**\
  Email delivery to common providers such as Gmail and Outlook relies on the **SMTP protocol**. To support this, 10102 integrates **Mailjet**, an email delivery service that provides SMTP server infrastructure for sending transactional emails.

Due to Chainlink’s decentralized, multi-node execution model, the same email request may be executed by multiple oracle nodes for redundancy and reliability. Since Mailjet does not natively deduplicate identical SMTP requests, this can occasionally result in duplicate emails being sent.

This behavior is a known tradeoff of decentralized oracle execution combined with traditional email infrastructure and does not affect the correctness or security of on-chain logic.

### Workflow

<figure><img src="../.gitbook/assets/unknown.png" alt=""><figcaption></figcaption></figure>

#### **Step 1: Legacy Contract Creation**

When a user creates a legacy contract, the **Router Contract** deploys and initializes the following components:

* **Legacy Contract**\
  Stores the core legacy logic, including beneficiaries and activation parameters.
* **Automation Contract**\
  Stores notification-related configuration and activation metadata used for monitoring trigger conditions.
* **Chainlink Automation Cron Job**\
  A scheduled job created via Chainlink Automation, configured to run once per day.

#### **Step 2: Trigger Condition Check**

On each scheduled run, the Chainlink Automation Cron Job performs the following steps:

* The Cron Job calls the `checkUpkeep` method on the **Automation Contract**.
* The Automation Contract queries the **Legacy Contract** to retrieve:
  * Beneficiary addresses
  * Legacy end time / activation timestamp
* The Automation Contract queries the **Settings Contract** to retrieve:
  * Email recipients (owner and beneficiaries)
  * Advance notice period (“before activation” timing)
* Using this information, the Automation Contract evaluates whether any notification trigger conditions have been met (for example, whether the current time falls within a configured reminder window).

The Automation Contract then returns the result of the trigger evaluation.

#### **Step 3: Notification Execution**

The Chainlink Automation Cron Job receives the evaluation result.

* If the result is **false**, no action is taken.
* If the result is **true**, the Cron Job invokes the `sendEmail` method on the **Chainlink Functions Contract**.
* Chainlink Functions then execute the off-chain workflow to send email notifications to the relevant beneficiaries and/or the owner.



<br>
