# Architecture using Safe wallet

![](<../.gitbook/assets/0 (1).png>)

Link draw.io: [flow.drawio - draw.io (diagrams.net)](https://app.diagrams.net/#G1nT\_94NsXZNhopJYV-lInEC7njBCxfqwg#%7B%22pageId%22%3A%228dWKgsQEE3mBGTgm3GKU%22%7D)

### **Prepare 2 contract:** <a href="#vyh116ocntri" id="vyh116ocntri"></a>

Firstly, we need to prepare 2 smart contract including: Safe module and SafeGuard

1. Safe Module contract is prepared to execute logic when activating will
2. SafeGuard contract is prepared to save latest timestamp transaction of safe wallet

### **Create will flow:** <a href="#x66hygxf4de9" id="x66hygxf4de9"></a>

* Step 1: Will owner create Safe wallet in [https://app.safe.global](https://app.safe.global/) and it is set as will contract ( Safe Proxy)
* Step 2: After set will contract in Safe Proxy, we will set guard in [https://app.safe.global](https://app.safe.global/) or in Computing Will site and use Set Guard Function of SafeGuard
* Step 3: Besides set guard, we set module in [https://app.safe.global](https://app.safe.global/) or in Computing Will site to set Module function
* Step 4: After setting guard and set module, the will owner can configure will details with: list asset, list beneficiaries, customization of trigger,..

### **Activate will flow:** <a href="#id-5hdkpt7pd7fh" id="id-5hdkpt7pd7fh"></a>

* Step 1: Beneficiary call activate will to Safe module contract ( beneficiary pay gas fee)
* Step 2:If enough minimum required signatures -> SafeModule check the latest timestamps transaction in SafeGuard
* Step 3: Safe Module execTransactionFromModule to SafeProxy ( the will contract)
* Step 4: SafeProxy ( the will contract) transfer money from SafeWallet to SafeModule and divide to beneficiary -> Beneficiary can receive funds
