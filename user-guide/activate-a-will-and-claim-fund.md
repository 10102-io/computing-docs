# Activate a will and claim fund

### **Table of contents** <a href="#cy3oh4wk17z3" id="cy3oh4wk17z3"></a>

[Check will's status & claim fund](activate-a-will-and-claim-fund.md#check-wills-status-and-claim-fund)

[Once the will is activated](activate-a-will-and-claim-fund.md#once-the-will-is-activated)

[Claim remaining fund](activate-a-will-and-claim-fund.md#claim-remaining-fund)

### Check will's status & claim fund

Beneficiary can click **Check will’s status & Claim Fund**, which will prompt the system to check if enough time has passed for the will to be activated.

<figure><img src="https://lh7-rt.googleusercontent.com/docsz/AD_4nXecNsKnKfiM2kDxm3lNMyoPq-Y41W3Nd2HznJvHwqKzHFhihNIPMcPAKmIV5xMpxjRmBlXBZsejhDPbSNzJo1zcunHFkdGZMRU00KfGQXleteCB1vbxi0UAcz5-aVJtwt-5O9Fe6ni0fBiHu7yU_oXcucxz?key=oRY7UDjlY8HFRVTJFFE-MA" alt=""><figcaption></figcaption></figure>

* If the required time has passed, the beneficiary will be eligible to claim fund. By claiming fund, the beneficiary will pay a gas fee, and the will will execute for all assets and beneficiaries. The status of the will will become **Activated**.
* If not enough time has passed, the system will prompt an error. The will remains not activated.

### Once the will is activated

* If the will is an **Inheritance** will, or a **Forwarding** will created with a Safe wallet, listed beneficiaries will be added as co-signers of the designated Safe wallet. The minimum signatures required to execute a transaction previously set in the Safe wallet remains unchanged.
* If the will is a **Forwarding** will created with an EOA, all approved ERC-20 tokens and deposited ETH  will be sent the the Ethereum public addresses of  all listed beneficiaries.

### Claim remaining fund

*   For a **Forwarding** will created with a EOA, the system can handle a maximum of 100 transactions at a time. If there are more than 100 transactions needed in the asset distribution process (eg. there are 4 beneficiaries and 25+ assets), the system will distribute assets in multiple batches. The button **Claim Remaining Fund** will be available if there are more assets to be claimed in the will contract.\


    <figure><img src="../.gitbook/assets/30.png" alt=""><figcaption></figcaption></figure>
* Once all assets are distributed, the system will show that asset distribution is complete.&#x20;

<figure><img src="../.gitbook/assets/Screen Shot 2024-12-17 at 6.09.11 PM.png" alt="" width="563"><figcaption></figcaption></figure>
