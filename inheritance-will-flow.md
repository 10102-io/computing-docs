# Inheritance will flow

### **Create will flow** <a href="#x66hygxf4de9" id="x66hygxf4de9"></a>

* Step 1: Will owner create Safe wallet in [https://app.safe.global](https://app.safe.global/) and it is set as will contract ( Safe Proxy)
* Step 2: After set will contract in Safe Proxy, we will set guard in [https://app.safe.global](https://app.safe.global/) or in Computing Will site and use Set Guard Function of SafeGuard
* Step 3: Besides set guard, we set module in [https://app.safe.global](https://app.safe.global/) or in Computing Will site to set Module function
* Step 4: After setting guard and set module, the will owner can configure will details with: list asset, list beneficiaries, customization of trigger,..

### **Activate will flow** <a href="#id-5hdkpt7pd7fh" id="id-5hdkpt7pd7fh"></a>

* Step 1: Beneficiary call activate will to Safe module contract ( beneficiary pay gas fee)
* Step 2:If enough minimum required signatures -> SafeModule check the latest timestamps transaction in SafeGuard
* Step 3: Safe Module execTransactionFromModule to SafeProxy ( the will contract)
* Step 4: SafeProxy ( the will contract) transfer money from SafeWallet to SafeModule and divide to beneficiary -> Beneficiary can receive funds
