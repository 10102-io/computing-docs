# MVP Build

## 1. Configure inheritance will type

* Designate beneficiar(ies). Options to choose from:&#x20;
  * Ethereum public address
  * Generate a new Ethereum public address, and a QR code that contain the private key
  * Option to add another beneficiary
    * If n beneficiaries are inputted, have option to select up to n required approvers/co-signers
* Configure Triggers (select multiple)
  * Lack of outgoing transactions
  * Lack of signing messages
* The user will select “Amount of inactivity time after which the will kicks in”
  * e.g X month(s), Y year(s)

## 2. Forwarding will type

Specify which assets should be sent to which Ethereum accounts. This option is more specific and straightforward, as it doesn't require co-signers to potentially work together as is the case with inheritance.

* Triggers and duration will be same with Inheritance
* Add beneficiary: Ethereum address&#x20;
* Asset allocation: select available asset to send to each added beneficiary
* If there’s anything left, suggest Inheritance flow.

### 2.1. Destruction template

* Same logic in terms of triggers, duration
* Pre-filled with Ethereum genesis address



## Superseding will

A new contract will be created and the old contract will be destroyed/deleted. This capability can be performed only by the initial owner of a will.



